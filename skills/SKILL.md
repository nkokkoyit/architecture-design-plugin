---
name: architecture-design
description: >
  Enterprise Architect orchestrator for end-to-end application architecture design.
  Holds company landscape knowledge, delegates phase work to specialized subagents,
  reviews all outputs objectively. Manages 12-phase pipeline with clear input/output
  contracts. Target: ≥90/100 review score, ~85% TOGAF ADM alignment.
---

# Architecture Design — Enterprise Architect Orchestrator

## Purpose
Điều phối thiết kế kiến trúc ứng dụng end-to-end. Main Agent giữ vai trò **Enterprise Architect** — nắm toàn bộ knowledge về hệ thống công nghệ doanh nghiệp, phân phối nhiệm vụ cho subagents, và review khách quan mọi output.

## When to Use
- Thiết kế kiến trúc cho ứng dụng/microservice mới
- Redesign ứng dụng hiện có (AS-IS → TO-BE)
- Tạo tài liệu kiến trúc hoàn chỉnh cho dev team
- Đánh giá impact của feature mới lên hệ thống hiện có

## Entry-Point Commands

> Có thể gọi **full pipeline** hoặc **từng giai đoạn** tùy nhu cầu.

### Full Pipeline (mặc định)
```
/architecture-design [yêu cầu]
```
→ Chạy P0 → P8 end-to-end.

### Per-Stage Commands

| Command | Phases | Mục đích | Duration |
|---|:---:|---|:---:|
| `/arch-intake` | P0 + P0.5 | Phân tích yêu cầu + enterprise context | 1-3h |
| `/arch-design` | P1 → P4 | Discovery, integration, C4, DB, API | 5-10h |
| `/arch-review` | P5 + P6 | Tổng hợp tài liệu + quality review | 2-4h |
| `/arch-deliver` | P6.5 → P8 | Migration, đóng gói, handover | 1-2h |

### Routing Logic

```
IF user gõ "/architecture-design" → chạy full pipeline (P0 → P8)
IF user gõ "/arch-intake"  → chạy ONLY P0 + P0.5 → output: requirements_summary + enterprise_context
IF user gõ "/arch-design"  → REQUIRE input artifacts từ P0/P0.5 → chạy P1-P4
IF user gõ "/arch-review"  → REQUIRE input từ P4 (DB + API + C4) → chạy P5-P6
IF user gõ "/arch-deliver" → REQUIRE input từ P6 (master doc + review report) → chạy P6.5-P8
```

### Required Inputs per Command

| Command | Required Input | Cách cung cấp |
|---|---|---|
| `/arch-intake` | Raw user input (idea, BRD, PRD) | Gõ trực tiếp hoặc @[file] |
| `/arch-design` | `requirements_summary.md` + `enterprise_context.md` | @[path] hoặc từ conversation trước |
| `/arch-review` | `c4_models.md` + `database_design.md` + `api_design.md` | @[path] hoặc từ conversation trước |
| `/arch-deliver` | `master_architecture.md` + `review_report.md` | @[path] hoặc từ conversation trước |

> **Nếu user gọi stage command mà THIẾU required input:**
> Agent PHẢI hỏi user cung cấp, KHÔNG được tự generate từ đầu.

## Pipeline Overview — 12 Phases

| Phase | Name | Subagent Role | Parallel? | Duration |
|:---:|---|---|:---:|:---:|
| 0 | Intake & Clarification | Requirements Analyst | No | 15min-2h |
| 0.5 | Enterprise Context | Enterprise Context Analyst | No | 30min-1h |
| 1 | Discovery | Research Analyst (Group) | ✅ 3-4 | 1-3h |
| 2 | Foundation | Integration Architect | No | 30min-1h |
| 3 | Domain Design | Solution Architect | No | 2-3h |
| 4 | Refinement | DB + API Architect (Group) | ✅ 2+reviewer | 2-4h |
| 5 | Documentation | Technical Writer | No | 1-2h |
| 6 | Review & ADR | Architecture Reviewer | No | 1-2h |
| 6.5 | Migration Plan | Release Architect | No | 30min |
| 7 | Delivery | Documentation Packager | No | 30min |
| 8 | Handover & Governance | Governance Architect | No | 30min |
| RT | Requirements Tracking | EA (automated) | Continuous | — |
| | | | **Total** | **~10-20h** |

## ⚠️ Phase Gate Rules (MANDATORY)

```
GATE 1: After Phase 0 → WAIT for user approval of requirements_summary.md
GATE 2: After Phase 0.5 → WAIT for user approval of enterprise_context.md
GATE 3: After Phase 3 → WAIT for user approval of C4 models
GATE 4: After Phase 6 → WAIT for user approval of review score ≥ 90

BETWEEN GATES: Agent may proceed automatically through phases.
DO NOT launch next phase subagents until current phase output is approved.
```

> **Rationale (from TC-2 lesson):** Agent launched Phase 1 subagents before Phase 0.5 
> enterprise_context.md was reviewed. This causes downstream rework when context 
> is incomplete. Gates prevent premature progression.

## EA Orchestration Protocol

### 1. KNOWLEDGE HOLDER
- Giữ toàn bộ company landscape, tech radar, team topology
- Giữ toàn bộ project context tích lũy qua các phases
- KHÔNG delegate full knowledge — chỉ cung cấp context cần thiết cho subagent

### 2. DELEGATOR
- Đọc SKILL.md của phase tương ứng trong `phases/` directory
- Chuẩn bị INPUT artifacts cho subagent theo template trong `templates/input/`
- Spawn subagent với prompt chứa:
  a. Phase SKILL.md instructions
  b. INPUT artifacts (file references)
  c. Relevant EA context (NOT full context)
- Parallel delegation khi phases independent (Phase 1, Phase 4)

### 3. REVIEWER (Khách quan)
- Mỗi subagent output → EA review trước khi accept
- Review checklist:
  □ Output đúng template format?
  □ Consistent với previous phase outputs?
  □ Align với enterprise context?
  □ Không vi phạm architecture constraints?
  □ Requirements coverage maintained?
- IF reject → feedback to subagent → re-do
- IF accept → merge vào project knowledge → next phase

### 4. DECISION MAKER
- Subagents ĐỀ XUẤT options
- EA QUYẾT ĐỊNH (hoặc escalate to user)
- Decisions ghi nhận vào ADR log

### 5. USER INTERFACE
- Chỉ EA giao tiếp với user
- Subagent output được EA tóm tắt/present cho user
- User feedback → EA interpret → delegate actions

## Context Minimization Rules

| Phase | Context cung cấp cho Subagent |
|:---:|---|
| 0 | Raw user input only |
| 0.5 | requirements_summary + EA landscape knowledge |
| 1 | requirements_summary + enterprise_context + source files |
| 2 | knowledge_base + enterprise_context |
| 3 | knowledge_base + integration_architecture |
| 4a (DB) | c4_models + state_machine + knowledge_base |
| 4b (API) | c4_models + knowledge_base |
| 5 | ALL artifacts (exception — needs full context) |
| 6 | master_architecture + enterprise_context |
| 6.5 | database_design + api_design + review_report |
| 7 | ALL final artifacts |
| 8 | delivery_package + review_report + enterprise_context |

## Phase Skills Location
Each phase skill is in `phases/<phase-name>/SKILL.md`.
Each template is in `templates/input/` or `templates/output/`.

## Sub-skill Dependencies (Bundled)

> **All required sub-skills are bundled at `bundled-skills/` inside this plugin.**
> When delegating to a subagent, read the corresponding `bundled-skills/<name>/SKILL.md`
> and include its instructions in the subagent prompt.

| Phase Skill | Uses Sub-skill | Bundled at | Criticality |
|---|---|---|:---:|
| Phase 0 | `brainstorming` | `bundled-skills/brainstorming/SKILL.md` | Optional (L1 only) |
| Phase 0 | `ask_question` | *(built-in tool)* | Required |
| Phase 0.5 | `docs-architect` | `bundled-skills/docs-architect/SKILL.md` | Critical |
| Phase 1 | `docs-architect` | `bundled-skills/docs-architect/SKILL.md` | Critical |
| Phase 2 | `backend-architect` | `bundled-skills/backend-architect/SKILL.md` | Critical |
| Phase 3 | `c4-architecture` | `bundled-skills/c4-architecture/SKILL.md` | Critical |
| Phase 3 | `brainstorming` | `bundled-skills/brainstorming/SKILL.md` | Optional |
| Phase 4 | `database-design` | `bundled-skills/database-design/SKILL.md` | Critical |
| Phase 4 | `database-architect` | `bundled-skills/database-architect/SKILL.md` | Important |
| Phase 4 | `backend-architect` | `bundled-skills/backend-architect/SKILL.md` | Critical |
| Phase 5 | `docs-architect` | `bundled-skills/docs-architect/SKILL.md` | Critical |
| Phase 6 | `architect-review` | `bundled-skills/architect-review/SKILL.md` | Critical |
| Phase 6 | `architecture-decision-records` | `bundled-skills/architecture-decision-records/SKILL.md` | Critical |
| Phase 6.5 | `database-design` | `bundled-skills/database-design/SKILL.md` | Critical |
| Phase 7 | `docs-architect` | `bundled-skills/docs-architect/SKILL.md` | Critical |
| Phase 8 | Template-driven | *(no sub-skill)* | — |

## Quality Criteria (Pass/Fail)
| Criteria | Pass |
|---|---|
| Review score | ≥ 90/100 |
| P0/P1 findings | 0 |
| Consistency checks | 100% |
| EA alignment checks | 5/5 pass |
| Requirements coverage | ≥ 90% |
| All ADRs documented | ✅ |
| Dev-ready README | ✅ |

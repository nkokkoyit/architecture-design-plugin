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

## Sub-skill Dependencies
| Phase Skill | Uses Sub-skills |
|---|---|
| Phase 0 | `brainstorming`, `ask_question` |
| Phase 0.5 | `docs-architect` |
| Phase 1 | `docs-architect`, research subagents |
| Phase 2 | `backend-architect` |
| Phase 3 | `c4-architecture`, `brainstorming` |
| Phase 4 | `database-design`, `database-architect`, `backend-architect` |
| Phase 5 | `docs-architect` |
| Phase 6 | `architect-review`, `architecture-decision-records` |
| Phase 6.5 | Template-driven + `database-design` |
| Phase 7 | `docs-architect`, file operations |
| Phase 8 | Template-driven |

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

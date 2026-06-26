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
Orchestrate end-to-end application architecture design. The Main Agent acts as the **Enterprise Architect** — holds all knowledge about the enterprise technology landscape, delegates tasks to subagents, and objectively reviews all outputs.

## When to Use
- Design architecture for a new application/microservice
- Redesign an existing application (AS-IS → TO-BE)
- Create comprehensive architecture documentation for the dev team
- Assess the impact of a new feature on the existing system

## Entry-Point Commands

> You can invoke the **full pipeline** or **individual stages** depending on your needs.

### Full Pipeline (default)
```
/architecture-design [request]
```
→ Runs P0 → P8 end-to-end.

### Per-Stage Commands

| Command | Phases | Purpose | Duration |
|---|:---:|---|:---:|
| `/arch-intake` | P0 + P0.5 | Analyze requirements + enterprise context | 1-3h |
| `/arch-design` | P1 → P4 | Discovery, integration, C4, DB, API | 5-10h |
| `/arch-review` | P5 + P6 | Consolidate documentation + quality review | 2-4h |
| `/arch-deliver` | P6.5 → P8 | Migration, packaging, handover | 1-2h |

### Routing Logic

```
IF user types "/architecture-design" → run full pipeline (P0 → P8)
IF user types "/arch-intake"  → run ONLY P0 + P0.5 → output: requirements_summary + enterprise_context
IF user types "/arch-design"  → REQUIRE input artifacts from P0/P0.5 → run P1-P4
IF user types "/arch-review"  → REQUIRE input from P4 (DB + API + C4) → run P5-P6
IF user types "/arch-deliver" → REQUIRE input from P6 (master doc + review report) → run P6.5-P8
```

### Required Inputs per Command

| Command | Required Input | How to Provide |
|---|---|---|
| `/arch-intake` | Raw user input (idea, BRD, PRD) | Type directly or @[file] |
| `/arch-design` | `requirements_summary.md` + `enterprise_context.md` | @[path] or from previous conversation |
| `/arch-review` | `c4_models.md` + `database_design.md` + `api_design.md` | @[path] or from previous conversation |
| `/arch-deliver` | `master_architecture.md` + `review_report.md` | @[path] or from previous conversation |

> **If user invokes a stage command MISSING required input:**
> Agent MUST ask user to provide it. DO NOT self-generate from scratch.

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
- Hold the entire company landscape, tech radar, team topology
- Hold the entire project context accumulated across phases
- DO NOT delegate full knowledge — only provide the context necessary for the subagent

#### Knowledge Base Loading Protocol
```
AT STARTUP (before Phase 0):
  1. Read `knowledge-base/kb_config.md` for configured paths
  2. IF company_kb path exists → read `company-landscape.md` (REQUIRED)
  3. Read all other .md files in the KB directory
  4. Store in EA context as "company knowledge"

AT PHASE 0.5:
  5. IF KB loaded → skip Q&A for sections already covered
  6. ONLY ask user about GAPS not in the KB
  7. IF KB NOT loaded → fall back to full Q&A (original behavior)
```

#### KB Directory Location (configurable)
Priority order:
1. Path specified in `knowledge-base/kb_config.md` → `company_kb` field
2. Workspace `.agents/knowledge/` directory
3. If neither exists → full Q&A mode (no KB)

### 2. DELEGATOR
- Read the SKILL.md of the corresponding phase in the `phases/` directory
- Prepare INPUT artifacts for the subagent using templates in `templates/input/`
- Spawn subagent with a prompt containing:
  a. Phase SKILL.md instructions
  b. INPUT artifacts (file references)
  c. Relevant EA context (NOT full context)
- Use parallel delegation when phases are independent (Phase 1, Phase 4)

### 3. REVIEWER (Objective)
- Every subagent output → EA reviews before accepting
- Review checklist:
  □ Does the output follow the template format?
  □ Is it consistent with previous phase outputs?
  □ Does it align with the enterprise context?
  □ Does it violate any architecture constraints?
  □ Is requirements coverage maintained?
- IF reject → feedback to subagent → re-do
- IF accept → merge into project knowledge → next phase

### 4. DECISION MAKER
- Subagents PROPOSE options
- EA DECIDES (or escalates to user)
- Decisions are recorded in the ADR log

### 5. USER INTERFACE
- Only the EA communicates with the user
- Subagent output is summarized/presented by the EA to the user
- User feedback → EA interprets → delegates actions

## Context Minimization Rules

| Phase | Context Provided to Subagent |
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

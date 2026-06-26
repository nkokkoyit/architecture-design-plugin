---
name: phase-0-intake
description: "Classify input maturity (L1-L4), structured Q&A to clarify requirements before architecture design begins."
---

# Phase 0: Intake & Clarification

## Role: Requirements Analyst
## Effort: 15 min (L4) → 2h (L1)

## Purpose
Classify input and clarify requirements before starting the design process.

## ⚠️ CRITICAL RULE: Classify BEFORE Q&A

> **Classify based on the RAW USER INPUT initially provided, NOT based on the quality of user answers.**
>
> Examples:
> - User types "I want to build a meeting room booking system" → **L1** (even if the user gives very detailed answers later)
> - User submits a 2-page BRD → **L3** (even if the BRD is still missing a lot of information)
> - User submits PRD + Swagger + mockup → **L4**

## Step 0.1 — Classify Input (IMMEDIATELY)

Read the raw user input and classify IMMEDIATELY. Notify the user of the classified level.

**Sample output:**
```
📋 Input Classification: Level 1 — Just Idea
→ Deep Q&A needed (15-20 questions) to clarify requirements before design.
```

### Classification Criteria

| Level | Identifying Characteristics | Action |
|:---:|---|---|
| **L4** | Has ≥ 2 of: PRD/SRS, Swagger/OpenAPI, UI mockup/HTML | Skip Q&A → output directly |
| **L3** | Has a structured document (BRD, Feature Spec) WITH clear sections (Objectives, Actors, Requirements) | Light Q&A — 3-5 supplementary questions |
| **L2** | Has a semi-structured description (email, slide, meeting notes) WITH some specific details | Medium Q&A — 8-12 questions |
| **L1** | Only has an idea, 1-5 sentence description, NO attached document | Deep Q&A — 15-20 questions + brainstorming |

### Boundary Between L1 and L2
- If user input < 100 words AND no attached document → **L1**
- If user input has a numbered list or sections but no document → **L2**
- If uncertain between L1 and L2 → choose **L1** (conservative — asking more is better than missing info)

## Step 0.2 — Q&A Loop (by Level)

> **MUST read `templates/input/intake_checklist.tmpl.md`** before starting Q&A.

### L1: Deep Q&A — 15-20 questions, 2-3 rounds

```
ROUND 1 (7-8 questions) — Vision, Scope, Actors:
  - Describe the idea in more detail?
  - What problem does it solve and for whom?
  - Greenfield or extension of an existing system?
  - MVP scope vs full vision?
  - Who will use it? (roles, personas)
  - Access channels? (Web, Mobile, API)
  - Is authentication needed? What type?

  → Wait for user response

ROUND 2 (5-6 questions) — Domain, Integration, Technical:
  - Which business domain does it belong to?
  - Regulated compliance? (PCI-DSS, HIPAA...)
  - Which systems does it integrate with?
  - Is there a Swagger/API doc?
  - Any technology stack constraints?
  - Team size and timeline?

  → Wait for user response

ROUND 3 (3-5 questions) — Data, NFR, Gaps:
  - What sensitive data? (PII, financial)
  - SLA expectations? (uptime, response time)
  - Data retention?
  - [Any remaining gaps from checklist]

  → Wait for user response → Generate output
```

### L2: Medium Q&A — 8-12 questions, 1-2 rounds

```
ROUND 1 (8-10 questions):
  - Cover all checklist categories not present in the input
  - Focus: Integration, NFR, Compliance, Data

  → IF gaps remain → ROUND 2 (2-3 questions)
  → ELSE → Generate output
```

### L3: Light Q&A — 3-5 questions, 1 round

```
ROUND 1 (3-5 questions):
  - Are NFRs clearly defined?
  - Compliance requirements?
  - Are integration points complete?
  - Rollback/DR strategy?
  - API versioning?

  → Generate output
```

### L4: Skip Q&A
```
  Proceed directly to output generation.
  (May ask 1-2 clarification questions if a critical gap is found)
```

## Step 0.3 — Generate Output

MUST use template: `templates/input/requirements_summary.tmpl.md`

Fill ALL sections in template:
1. Vision & Scope
2. Actors & Personas
3. Functional Requirements (FR-001, FR-002...)
4. Non-Functional Requirements (NFR-001...) — MUST have specific targets
5. Constraints (CON-001...)
6. Integration Points — with direction, protocol
7. Open Questions — anything still unclear

Create initial `requirements_traceability.md` using `templates/output/requirements_traceability.tmpl.md`.

## Input

| Source | Content |
|---|---|
| User | Raw input (any format: BRD, SRS, PRD, email, idea, swagger, UI mockup) |

## Output

| Artifact | Template | Description |
|---|---|---|
| `requirements_summary.md` | `templates/input/requirements_summary.tmpl.md` | Vision, actors, FR/NFR/Constraints, integration points |
| `requirements_traceability.md` | `templates/output/requirements_traceability.tmpl.md` | Initial matrix (Design/DB/API = TBD) |

## Sub-skills Used
- `brainstorming` (if L1 — Just Idea)
- `ask_question` (all levels except L4)

## Handoff to EA
Return `requirements_summary.md` + `requirements_traceability.md` to Main Agent for review.
**WAIT** for EA approval before Phase 0.5. Do NOT proceed automatically.

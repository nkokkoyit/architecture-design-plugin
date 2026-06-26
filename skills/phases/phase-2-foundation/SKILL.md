---
name: phase-2-foundation
description: "Design integration architecture: AS-IS vs TO-BE flows, architecture pattern selection, service interaction matrix."
---

# Phase 2: Foundation — Integration Architecture

## Role: Integration Architect
## Effort: 1h

## Purpose
Design integration flows and determine key architecture decisions.

## Steps

### 2.1 — AS-IS Flow (if redesign)
Draw the current-state sequence diagram. Identify pain points.

### 2.2 — TO-BE Flow
Design the new flow. Highlight improvements vs AS-IS.

### 2.3 — Architecture Pattern Decision
Select the appropriate pattern: BFF, API Gateway, CQRS, Event-Driven, etc.
Justify choice based on requirements + enterprise context.

### 2.4 — Service Interaction Matrix

Map: [New Service] →calls→ [Downstream Services]
Include: API path, method, data exchanged, SLA.

### 2.5 — Technology Validation
Validate chosen tech stack against `enterprise_context.md` → `technology_constraints.approved_stack`.

## Input

| Source | Content |
|---|---|
| Phase 1 | `knowledge_base.md` |
| Phase 0.5 | `enterprise_context.md` |

## Output

| Artifact | Template | Description |
|---|---|---|
| `integration_architecture.md` | `templates/output/integration_architecture.tmpl.md` | AS-IS/TO-BE flows, pattern decision, interaction matrix |

## Sub-skills Used
- `backend-architect`

## Handoff to EA
Return `integration_architecture.md`. EA reviews alignment with enterprise landscape.

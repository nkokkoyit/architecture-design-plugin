---
name: phase-3-domain-design
description: "C4 architecture modeling (L1-L4), state machine design, domain-specific feature designs."
---

# Phase 3: Domain Design

## Role: Solution Architect
## Effort: 2-3h

## Purpose
Thiết kế C4 models, state machine, và domain-specific features.

## Steps

### 3.1 — C4 Models
1. **Level 1: System Context** — Actors, external systems
2. **Level 2: Container Diagram** — Deployable units, databases, services
3. **Level 3: Component Diagram** — Internal modules of the new service
4. **Level 4: Class Diagram** *(optional)* — Key classes, methods, relationships
   > L4 là optional cho service-level design. Chỉ include nếu service có domain model phức tạp (≥10 classes).
   > Nếu skip L4, ghi rõ lý do trong output.

All diagrams in Mermaid format.
Each level includes description table.

### 3.2 — State Machine
- Enumerate ALL states + transitions
- Define guards and triggers
- Document backward transition rules
- Identify webhook-driven vs user-driven transitions

### 3.3 — Domain Feature Designs (if needed)
For complex sub-features (e.g., voucher, payment), create separate `feature_designs/[name]_design.md`.

### 3.4 — Update Requirements Traceability
MAP: requirement → C4 component, state

## Input

| Source | Content |
|---|---|
| Phase 1 | `knowledge_base.md` |
| Phase 2 | `integration_architecture.md` |

## Output

| Artifact | Template | Description |
|---|---|---|
| `c4_models.md` | `templates/output/c4_models.tmpl.md` | L1-L4 diagrams + description tables |
| `state_machine.md` | (embedded or separate) | State diagram, transitions, rules |
| `feature_designs/*.md` | (as needed) | Domain-specific designs |
| `requirements_traceability.md` | (update) | MAP design coverage |

## Sub-skills Used
- `c4-architecture`
- `brainstorming` (for feature options)

## EA Checkpoint
⚠️ EA MUST review C4 models before Phase 4. Check:
- L2 Container Diagram khớp với enterprise landscape
- Không vi phạm bounded context rules
- State count matches requirements

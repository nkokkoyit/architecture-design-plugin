---
name: phase-5-documentation
description: "Consolidate all design artifacts into a single master architecture document. Target: 40-70KB, self-contained."
---

# Phase 5: Documentation — Master Architecture Document

## Role: Technical Writer
## Effort: 1-2h

## Purpose
Consolidate all artifacts into a single, complete master document.

## Document Structure

```
master_architecture.md:
├── 1. Executive Summary
├── 2. C4 Level 1: System Context
├── 3. C4 Level 2: Container Diagram
├── 4. C4 Level 3: Component Diagram
├── 5. C4 Level 4: Class Diagram (optional — include only if service has complex domain model)
├── 6. State Machine & Business Flow
│   ├── 6.1 State Diagram
│   ├── 6.2 Backward Transitions
│   ├── 6.3 Sequence Diagram (TO-BE)
│   └── 6.4 Sequence Diagram (AS-IS)
├── 7. API Design — BFF Endpoints
├── 8. Downstream Integration Mapping
├── 9. Database Design (ERD + principles)
├── 10. Feature Designs (if any)
├── 11. Data Flow Matrix
├── 12. Data Mapping (DB → Downstream)
├── 13. Security & Compliance
├── 14. Resilience & Observability
├── 15. Deployment Architecture
└── 16. Architecture Decision Records (ADR)
```

## Rules
1. Merge content from source artifacts — do NOT just link to them
2. Ensure all Mermaid diagrams render correctly
3. Add cross-references between sections
4. Include table of contents with anchor links
5. Target: 40-70KB, self-contained (reader needs only this doc)
6. C4 Level 4 is optional — include only for services with complex domain models (≥10 classes)

## Input
| Source | Content |
|---|---|
| ALL previous phases | All artifacts created so far |

## Output
| Artifact | Template | Description |
|---|---|---|
| `master_architecture.md` | `templates/output/master_architecture.tmpl.md` | Complete architecture document |

## Sub-skills Used
- `docs-architect`

## Handoff to EA
Return `master_architecture.md`. EA does final read-through before Phase 6 review.

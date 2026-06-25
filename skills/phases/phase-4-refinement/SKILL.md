---
name: phase-4-refinement
description: "Database schema design + API endpoint design with peer review loops. Parallel execution with cross-artifact validation."
---

# Phase 4: Refinement — DB & API Design

## Role: DB Architect + API Architect (Parallel Group)
## Effort: 2-4h (parallel, with review loops)

## Purpose
Thiết kế chi tiết database schema và API endpoints.

## Parallel Strategy

EA spawns 2 subagents đồng thời:

| Subagent | Task | Sub-skill | Output |
|---|---|---|---|
| A: DB Designer | Database schema, ERD, DDL, indexes | `database-design` | `database_design.md` |
| B: API Designer | Endpoint contracts, sequences, errors | `backend-architect` | `api_design.md` |

THEN (sequential):
| Subagent | Task | Sub-skill | Output |
|---|---|---|---|
| C: DB Reviewer | Peer review DB design | `database-architect` | Review findings |

## Review Loop

```
LOOP (max 3 iterations):
  1. Subagent C reviews database_design.md
  2. IF clean → EA approves
  3. ELSE → Subagent A fixes → Subagent C re-reviews
```

## Cross-Artifact Validation (EA does this)

After both subagents complete:
1. DB columns ↔ API request/response fields
2. API endpoints ↔ Swagger downstream schemas
3. State machine states ↔ DB enum values
4. Data mapping: DB → downstream API payloads

## Steps

### 4.1 — Database Design
- Technology choice (engine, ORM, UUID strategy)
- ERD diagram (Mermaid)
- Full DDL scripts (CREATE TABLE, INDEX, TRIGGER)
- Enum definitions
- Data mapping: BFF DB → downstream payloads
- Caching strategy
- Storage estimate

### 4.2 — API Design
- Endpoint table (path, method, step, description)
- Per-endpoint detail (request/response schema, sequence diagram)
- Error handling strategy (HTTP status codes)
- Downstream API mapping table

### 4.3 — Update Requirements Traceability
MAP: requirement → DB table, API endpoint

## Input
| Source | Content |
|---|---|
| Phase 3 | `c4_models.md`, `state_machine.md` |
| Phase 1 | `knowledge_base.md`, `nfr_matrix.md` |

## Output
| Artifact | Template | Description |
|---|---|---|
| `database_design.md` | `templates/output/database_design.tmpl.md` | ERD, DDL, indexes, data mapping |
| `api_design.md` | `templates/output/api_design.tmpl.md` | Endpoints, schemas, sequences |
| `requirements_traceability.md` | (update) | MAP DB + API columns |

## Sub-skills Used
- `database-design` (DB Designer)
- `database-architect` (DB Reviewer)
- `backend-architect` (API Designer)

## Handoff to EA
Return both artifacts. EA cross-validates consistency before Phase 5.

---
name: requirements-tracking
description: "Continuous requirements traceability: extract, enrich, map, verify across all phases. Automated gap detection."
---

# Requirements Tracking — Continuous

## Role: Main Agent (EA) — Automated
## Trigger: End of each phase

## Purpose
Maintain the traceability matrix throughout the pipeline. Ensure every requirement is implemented.

## Lifecycle

| Phase | Action | Column updated |
|:---:|---|---|
| 0 | CREATE — Extract reqs from input | id, description, source, priority |
| 1 | ENRICH — Add discovered reqs | (new rows) |
| 3 | MAP DESIGN — Link to C4 component, state | design_coverage |
| 4 | MAP IMPL — Link to DB table, API endpoint | db_table, api_endpoint |
| 6 | VERIFY — Gap check, coverage % | status |
| 6.5 | MAP TEST — Link to test plan | test_plan |
| 8 | FINAL — Include in handover | (finalize) |

## Matrix Schema

| Column | Type | Description |
|---|---|---|
| `req_id` | string | FR-001, NFR-001, CON-001 |
| `description` | string | Requirement text |
| `source` | string | PRD §X, User Q&A, EA Decision |
| `priority` | enum | Must / Should / Could |
| `design_coverage` | string | C4 component, state, design doc |
| `db_table` | string | Table name(s) |
| `api_endpoint` | string | Endpoint path(s) |
| `test_plan` | string | Test type + name |
| `status` | enum | ✅ Covered / ☐ Not yet |

## Gap Detection Rules

```
RULE 1: Every FR must have design_coverage
  → IF missing → WARNING

RULE 2: Every Must-priority req must have db_table OR api_endpoint
  → IF missing → ERROR (block delivery)

RULE 3: Every NFR must have test_plan
  → IF missing → WARNING

RULE 4: Coverage % = count(✅) / count(total)
  → IF < 90% → BLOCK delivery
  → IF 90-99% → WARNING in review
  → IF 100% → PASS
```

## Output

| Artifact | Description |
|---|---|
| `requirements_traceability.md` | Living document, updated each phase |

## Note
This is NOT a subagent task — Main Agent (EA) maintains this directly as part of orchestration.

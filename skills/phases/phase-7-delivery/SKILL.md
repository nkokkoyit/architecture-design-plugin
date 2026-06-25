---
name: phase-7-delivery
description: "Package all final artifacts into a structured delivery folder with README and reading order."
---

# Phase 7: Delivery — Dev Package

## Role: Documentation Packager
## Effort: 30 min

## Purpose
Đóng gói tất cả artifacts thành delivery folder cho dev team.

## Package Structure

```
delivery_package/
├── README.md                    # Index, reading order, key ADRs
├── master_architecture.md       # Main doc
├── api_design.md               # API contracts
├── database_design.md          # DDL, ERD, mapping
├── migration_plan.md           # Deployment & rollout
├── enterprise_context.md       # Landscape, dependencies
├── feature_designs/            # Domain-specific designs
├── adr_responses.md            # ADR records
├── review_report.md            # Quality evidence
└── requirements_traceability.md # Coverage matrix
```

## README.md Content
1. Project name + version + date
2. Status (review score, verdict)
3. File index table (name, size, description)
4. Reading order recommendation
5. Key ADRs quick reference
6. Swagger file references

## Steps
1. Create delivery folder
2. Copy all final artifacts
3. Generate README.md from template
4. Verify all files present

## Input

| Source | Content |
|---|---|
| ALL phases | All final artifacts |

## Output

| Artifact | Description |
|---|---|
| `delivery_package/` | Complete folder ready for dev team |
| `delivery_package/README.md` | Index and guide |

## Handoff to EA
Return package. EA does final verification, then presents to user.

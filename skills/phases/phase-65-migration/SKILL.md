---
name: phase-65-migration
description: "Migration & rollout plan: deployment strategy, DB migration scripts, dependency coordination, pre-go-live checklist, risk assessment."
---

# Phase 6.5: Migration & Rollout Plan

## Role: Release Architect
## Effort: 30 min

## Purpose
Kế hoạch triển khai an toàn: deploy thế nào, rollback ra sao, ai phối hợp.

## Sections

### 1. Deployment Strategy
- Pattern: Canary / Blue-Green / Rolling / Big Bang
- Feature flags (if any)
- Traffic splitting plan
- Rollback trigger + time target

### 2. Database Migration
- Migration scripts ordered list (V001, V002...)
- Each: type (DDL/DML), reversible?, risk level
- Rollback plan for DB

### 3. Integration Coordination
- Dependency matrix: team, what needed, deadline, status
- API versioning strategy

### 4. Pre-Go-Live Checklist
- Technical: DB tested, smoke test, perf test, security scan, monitoring, runbook, rollback tested
- Business: UAT sign-off, marketing ready, support briefed, compliance

### 5. Risk Assessment
- Risk, probability, impact, mitigation

### 6. Rollout Phases (if phased)
- Phase, scope, duration, success criteria, go/no-go

## Input

| Source | Content |
|---|---|
| Phase 4 | `database_design.md`, `api_design.md` |
| Phase 6 | `review_report.md` |
| Phase 0.5 | `enterprise_context.md` |

## Output

| Artifact | Template | Description |
|---|---|---|
| `migration_plan.md` | `templates/output/migration_plan.tmpl.md` | Complete deployment & migration plan |

## Sub-skills Used
- Template-driven (`templates/output/migration_plan.tmpl.md`)
- `database-design` (for script ordering)

## Handoff to EA
Return `migration_plan.md`. EA validates coordination plan with enterprise context.

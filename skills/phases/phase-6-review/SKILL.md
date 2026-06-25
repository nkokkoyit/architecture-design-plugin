---
name: phase-6-review
description: "Architecture quality review: 16 standard pillars + 5 EA alignment checks. Scoring, findings, ADR generation."
---

# Phase 6: Review & ADR

## Role: Architecture Reviewer (Objective)
## Effort: 1-2h (including fix loops)

## Purpose
Quality gate — đánh giá chất lượng tài liệu kiến trúc.

## Review Protocol

### 16 Standard Pillars

| # | Pillar | Weight |
|:---:|---|:---:|
| 1-4 | C4 Models (L1, L2, L3, L4) | 5 each |
| 5 | State Machine | 10 |
| 6 | Sequence Diagrams | 5 |
| 7 | API Design | 10 |
| 8 | Downstream Integration | 5 |
| 9 | Database Design | 10 |
| 10 | Feature Design | 5 |
| 11 | Data Flow | 5 |
| 12 | Data Mapping | 5 |
| 13 | Security | 10 |
| 14 | Resilience & Observability | 5 |
| 15 | Deployment | 5 |
| 16 | ADR Quality | 5 |
| **Total** | | **100** |

### 5 EA Alignment Checks (read `references/ea-review-lens.md`)

| # | EA Check | Pass criteria |
|:---:|---|---|
| EA-1 | Bounded Context alignment | No cross-context coupling |
| EA-2 | Data ownership clear | No shared DB anti-pattern |
| EA-3 | Team topology fit | Stream-aligned or platform team |
| EA-4 | Observability integrated | Connected to enterprise monitoring |
| EA-5 | API versioning documented | Policy exists |

### Finding Severity

| Severity | Meaning | Action |
|---|---|---|
| P0 Critical | Architecture fundamentally flawed | BLOCK — must fix |
| P1 High | Missing important section | BLOCK — fix before delivery |
| P2 Medium | Inconsistency or gap | FIX in current cycle |
| P3 Low | Cosmetic | OPTIONAL |

## Review Loop

> **RULE: Always run AT LEAST 1 fix cycle, even if score ≥ 90.**
> Rationale: TC-3 showed P2 findings deferred to "fix during implementation" 
> when score was 94. These should be fixed in the review phase itself.

```
ROUND 1 — Initial Review:
  1. Score 16 pillars + 5 EA checks
  2. Document ALL findings (P0-P3) with fix suggestions
  3. Generate ADRs for key decisions

ROUND 2 — Mandatory Fix Cycle:
  4. Fix ALL P0 + P1 findings (mandatory)
  5. Fix ALL P2 findings (mandatory)
  6. Fix P3 findings (recommended, skip if cosmetic-only)
  7. Re-score affected pillars

  → IF score ≥ 90 AND P0+P1+P2 = 0 → PASS
  → ELSE → ROUND 3

ROUND 3 — Final Fix (if needed, max):
  8. Fix remaining findings
  9. Final score
  → IF score ≥ 90 AND P0+P1 = 0 → PASS (P2 remaining = WARNING)
  → ELSE → FAIL — escalate to user
```

## Requirements Verification
Run gap check on `requirements_traceability.md`:
- Coverage must be ≥ 90%
- All Must-have requirements must have design + DB/API mapping
- Flag any ☐ items

## Input

| Source | Content |
|---|---|
| Phase 5 | `master_architecture.md` |
| Phase 0.5 | `enterprise_context.md` (EA lens) |

## Output

| Artifact | Description |
|---|---|
| `review_report.md` | Score, pillar scores, findings, verdict |
| `adr_responses.md` | ADRs in MADR format |
| `requirements_traceability.md` | (update) VERIFY coverage |

## Sub-skills Used
- `architect-review`
- `architecture-decision-records`

## Handoff to EA
EA is the final reviewer. EA decides PASS/FAIL and whether to loop.

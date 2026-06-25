---
name: phase-1-discovery
description: "Deep-dive research: parallel analysis of UI flows, API schemas, entity models, NFR extraction. Produces consolidated knowledge base."
---

# Phase 1: Discovery

## Role: Research Analyst (Parallel Group)
## Effort: 1-2h (parallel execution)

## Purpose
Xây dựng knowledge base chi tiết từ raw inputs + enterprise context.

## Parallel Subagent Strategy

EA spawns 3-4 subagents đồng thời (tùy input availability):

| Subagent | Task | Input | Output |
|---|---|---|---|
| A: UI Analyst | Phân tích UI flow, steps, data per step | HTML/mockup/Figma | `ui_flow_analysis` |
| B: API Extractor | Extract request/response schemas from Swagger | Swagger/OpenAPI files | `api_schema_extraction` |
| C: Codebase Analyst | Analyze existing codebase (if redesign) | Source code | `existing_architecture` |
| D: NFR Extractor | Extract and formalize NFR from all sources | All inputs + enterprise_context | `nfr_matrix` |

## Consolidation (EA does this)

After all subagents complete:
1. Merge outputs into `knowledge_base.md`
2. Cross-reference UI fields ↔ API schemas ↔ entity models
3. Identify gaps (UI field with no API endpoint, etc.)
4. Update `requirements_traceability.md` with discovered requirements

## Input

| Source | Content |
|---|---|
| Phase 0 | `requirements_summary.md` |
| Phase 0.5 | `enterprise_context.md` |
| User | Source files: Swagger, UI mockup, existing code |

## Output

| Artifact | Template | Description |
|---|---|---|
| `knowledge_base.md` | `templates/output/knowledge_base.tmpl.md` | UI flows, API schemas, entity models, business rules |
| `nfr_matrix.md` | `templates/input/nfr_matrix.tmpl.md` | Performance, Security, Availability targets |
| `requirements_traceability.md` | (update) | ENRICH: add discovered requirements |

## Sub-skills Used
- `docs-architect` (analyze documents)
- Research subagents (parallel)

## Handoff to EA
Return `knowledge_base.md` + `nfr_matrix.md`. EA consolidates and reviews before Phase 2.

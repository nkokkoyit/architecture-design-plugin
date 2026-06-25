---
name: phase-8-handover
description: "Post-delivery governance: architecture contract, RACI ownership, operational runbook, change management process."
---

# Phase 8: Handover & Governance

## Role: Governance Architect
## Effort: 30 min

## Purpose
Định nghĩa governance sau delivery: ai own gì, thay đổi thế nào, monitor ra sao.

## Sections

### 1. Architecture Contract
- Scope agreement (committed vs out-of-scope)
- Architecture constraints (extracted from ADRs) — KHÔNG được phá vỡ
- Deviation process (how to change architecture)

### 2. Ownership Matrix (RACI)
- Per component: Responsible, Accountable, Consulted, Informed
- On-call & escalation path

### 3. Operational Runbook
- Health monitoring metrics + alert thresholds
- Common scenarios & response procedures
- Maintenance tasks (scheduled jobs, cleanup)

### 4. Change Management
- Architecture Change Request (ACR) template
- Change classification: Cosmetic → Minor → Major → Breaking → Architecture
- Approval matrix per classification

### 5. Knowledge Transfer Checklist
- Walk through master doc (1h)
- Demo flow on staging (30 min)
- Review state machine edge cases (30 min)
- Review ADR decisions (30 min)
- Provisioned access (Git, DB, monitoring)

## Input

| Source | Content |
|---|---|
| Phase 7 | `delivery_package/` |
| Phase 6 | `review_report.md` |
| Phase 0.5 | `enterprise_context.md` |

## Output

| Artifact | Template | Description |
|---|---|---|
| `handover_governance.md` | `templates/output/handover_governance.tmpl.md` | Contract, RACI, Runbook, Change Mgmt |

## Sub-skills Used
- Template-driven

## Handoff to EA
Return `handover_governance.md`. Add to delivery package. Architecture design complete.

# Handover & Governance

> **Project:** [TODO]
> **Date:** [TODO]

## 1. Architecture Contract

### Scope Agreement
| Dimension | Committed | Out of Scope |
|---|---|---|
| [TODO: Endpoints] | [TODO] | [TODO] |
| [TODO: Database] | [TODO] | [TODO] |
| [TODO: Integrations] | [TODO] | [TODO] |

### Architecture Constraints (DO NOT BREAK)
| # | Constraint | Rationale | ADR |
|:---:|---|---|---|
| C1 | [TODO] | [TODO] | ADR-[TODO] |

### Deviation Process
1. Tạo ADR mới (MADR template)
2. Đánh giá impact (upstream/downstream)
3. Review với Architecture owner
4. Cập nhật master architecture document
5. Cập nhật traceability matrix

## 2. Ownership Matrix (RACI)

| Component | Responsible | Accountable | Consulted | Informed |
|---|---|---|---|---|
| [TODO] | [TODO] | [TODO] | [TODO] | [TODO] |

### On-call & Escalation
```
Level 1: Alert → On-call dev (15 min)
Level 2: Dev cannot resolve → Tech Lead (30 min)
Level 3: Service down > 30 min → SRE + Architect
```

## 3. Operational Runbook

### Health Monitoring
| Metric | Tool | Alert Threshold | Action |
|---|---|---|---|
| [TODO] | [TODO] | [TODO] | [TODO] |

### Common Scenarios
| Scenario | Symptom | Response |
|---|---|---|
| [TODO] | [TODO] | [TODO] |

### Maintenance Tasks
| Task | Schedule | Procedure |
|---|---|---|
| [TODO] | [TODO] | [TODO] |

## 4. Change Management

### Change Classification
| Type | Approval | Example |
|---|---|---|
| Cosmetic | Dev self-approve | Rename variable |
| Minor | Tech Lead | Add optional API field |
| Major | Tech Lead + Architect | New integration |
| Breaking | Tech Lead + Architect + PO | API breaking change |
| Architecture | Full ACR process | New service, pattern change |

### ACR Template
```markdown
# ACR-[NUMBER]: [Title]
Requestor: [Name] | Date: [Date] | Priority: [P1-P4]
## Current State
## Proposed Change
## Impact Analysis
## ADR Reference
## Approval: [ ] Arch Owner  [ ] Tech Lead  [ ] Security
```

## 5. Knowledge Transfer Checklist
- [ ] Architecture document walkthrough (1h)
- [ ] Demo flow on staging (30 min)
- [ ] State machine + edge cases (30 min)
- [ ] ADR decisions review (30 min)
- [ ] Runbook walkthrough (15 min)
- [ ] Access provisioned (Git, DB, monitoring)
- [ ] On-call rotation updated

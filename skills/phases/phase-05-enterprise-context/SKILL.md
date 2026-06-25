---
name: phase-05-enterprise-context
description: "Analyze enterprise context: system landscape, business domain, dependencies, team topology, compliance requirements."
---

# Phase 0.5: Enterprise Context Analysis

## Role: Enterprise Context Analyst
## Effort: 30 min — 1h

## Purpose
Đặt ứng dụng mới vào bức tranh doanh nghiệp. Hiểu "nó ngồi ở đâu" trước khi thiết kế chi tiết.

## ⚠️ CRITICAL RULE: PHẢI hỏi Enterprise Context chủ động

> **KHÔNG được skip bước hỏi enterprise context.**
> Nếu user chưa cung cấp landscape info, PHẢI hỏi trước khi tạo output.
> KHÔNG được infer/đoán enterprise context từ user input — PHẢI xác nhận với user.

## Step 0.5.1 — Gather Enterprise Context (BẮT BUỘC HỎI)

Hỏi user những câu sau (nếu chưa có trong input):

```
Để đặt hệ thống mới vào bức tranh doanh nghiệp, tôi cần biết:

1. 🏢 System Landscape:
   - Công ty hiện có bao nhiêu services/applications?
   - Liệt kê các services chính liên quan tới dự án này?
   - Có API Gateway / Service Mesh chung không?

2. 🔧 Technology Stack:
   - Tech stack chuẩn công ty (language, framework, DB, cloud)?
   - Có technology radar không? Stack nào approved / deprecated?

3. 👥 Team Topology:
   - Team nào sẽ own service mới?
   - Các team liên quan (upstream/downstream)?
   - Communication pattern? (collaboration / X-as-a-service)

4. 🔒 Compliance & Security:
   - Có yêu cầu compliance đặc biệt? (PCI-DSS, HIPAA, Insurance regs)
   - Data classification policy?
   - Encryption requirements?

5. 📊 Monitoring & Operations:
   - Monitoring platform hiện có? (Prometheus, DataDog, AppInsights...)
   - Logging platform? (ELK, CloudWatch...)
   - On-call process?
```

> **Nếu user không có document landscape:** Hỏi user mô tả bằng lời. Ít nhất phải biết: có bao nhiêu services, stack chính, ai own gì.
>
> **Nếu user đã cung cấp context trong BRD:** Xác nhận lại và hỏi gaps (thường thiếu: monitoring, team topology, compliance).

## Step 0.5.2 — Map Dependencies

For each integration point in `requirements_summary.md`:

1. **Upstream** (services calling INTO new service):
   - Service name, API protocol, data flow
   - Owner team, SLA

2. **Downstream** (services new service CALLS OUT to):
   - Service name, API protocol, data flow
   - Owner team, SLA
   - Failure strategy (circuit breaker, fallback)

3. **Data ownership:**
   - Mỗi entity thuộc service nào?
   - Source-of-truth ở đâu?

4. **Compliance mapping:**
   - PII fields → encryption requirement
   - Retention period per data category

## Step 0.5.3 — Impact Analysis

| Dimension | Question | MUST answer |
|---|---|:---:|
| Data | Service mới tạo data entity mới hay dùng existing? | ✅ |
| API | Có cần expose API cho services khác không? | ✅ |
| Event | Publish events nào? Subscribe events nào? | ✅ |
| Performance | Thêm load lên downstream services bao nhiêu? | ✅ |
| Security | PII mới? Access control changes? | ✅ |

## Step 0.5.4 — Generate Output

MUST use template: `templates/input/enterprise_context.tmpl.md`

Fill ALL sections:
1. System Landscape (Mermaid diagram BẮT BUỘC)
2. Service Catalog table
3. Dependency Matrix (Upstream + Downstream)
4. Business Domain (bounded context, capability, data ownership)
5. Technology Constraints (approved stack, governance rules)
6. Team Topology (owning team, collaborating teams)
7. Compliance Requirements

## Input

| Source | Content |
|---|---|
| Phase 0 | `requirements_summary.md` |
| EA Knowledge | Company landscape, tech radar, team topology |
| User (BẮT BUỘC hỏi) | Enterprise context info |

## Output

| Artifact | Template | Description |
|---|---|---|
| `enterprise_context.md` | `templates/input/enterprise_context.tmpl.md` | Landscape diagram, dependency matrix, domain mapping, compliance |

## Sub-skills Used
- `docs-architect` (analyze existing documentation)

## Handoff to EA
Return `enterprise_context.md`. EA validates against own knowledge before approving.
**WAIT** for EA approval before Phase 1. Do NOT proceed automatically.

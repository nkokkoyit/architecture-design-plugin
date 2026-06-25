---
name: phase-0-intake
description: "Classify input maturity (L1-L4), structured Q&A to clarify requirements before architecture design begins."
---

# Phase 0: Intake & Clarification

## Role: Requirements Analyst
## Effort: 15 min (L4) → 2h (L1)

## Purpose
Phân loại đầu vào, làm rõ yêu cầu trước khi bắt đầu thiết kế.

## ⚠️ CRITICAL RULE: Classify BEFORE Q&A

> **Classify dựa trên RAW USER INPUT ban đầu, KHÔNG PHẢI dựa trên quality of user answers.**
>
> Ví dụ:
> - User gõ "Tôi muốn xây dựng hệ thống đặt phòng họp" → **L1** (dù user trả lời rất chi tiết sau đó)
> - User gửi BRD 2 trang → **L3** (dù BRD còn thiếu nhiều thông tin)
> - User gửi PRD + Swagger + mockup → **L4**

## Step 0.1 — Classify Input (NGAY LẬP TỨC)

Đọc raw user input và classify NGAY. Thông báo cho user biết level đã classify.

**Output mẫu:**
```
📋 Input Classification: Level 1 — Just Idea
→ Cần Q&A sâu (15-20 câu) để làm rõ yêu cầu trước khi thiết kế.
```

### Classification Criteria

| Level | Đặc điểm nhận diện | Action |
|:---:|---|---|
| **L4** | Có ≥ 2 trong: PRD/SRS, Swagger/OpenAPI, UI mockup/HTML | Skip Q&A → output directly |
| **L3** | Có document cấu trúc (BRD, Feature Spec) VỚI sections rõ ràng (Objectives, Actors, Requirements) | Q&A nhẹ — 3-5 câu bổ sung |
| **L2** | Có mô tả semi-structured (email, slide, meeting notes) VỚI một số chi tiết cụ thể | Q&A trung bình — 8-12 câu |
| **L1** | Chỉ có ý tưởng, 1-5 câu mô tả, KHÔNG có document đính kèm | Q&A sâu — 15-20 câu + brainstorming |

### Ranh giới L1 vs L2
- Nếu user input < 100 words VÀ không có document đính kèm → **L1**
- Nếu user input có numbered list hoặc sections nhưng không có document → **L2**
- Nếu nghi ngờ giữa L1 và L2 → chọn **L1** (conservative — hỏi nhiều hơn tốt hơn thiếu)

## Step 0.2 — Q&A Loop (theo Level)

> **MUST read `templates/input/intake_checklist.tmpl.md`** trước khi bắt đầu Q&A.

### L1: Q&A Sâu — 15-20 câu, 2-3 rounds

```
ROUND 1 (7-8 câu) — Vision, Scope, Actors:
  - Mô tả ý tưởng chi tiết hơn?
  - Giải quyết vấn đề gì cho ai?
  - Greenfield hay extension hệ thống hiện có?
  - MVP scope vs full vision?
  - Ai sẽ sử dụng? (roles, personas)
  - Kênh truy cập? (Web, Mobile, API)
  - Cần authentication không? Loại gì?

  → Chờ user trả lời

ROUND 2 (5-6 câu) — Domain, Integration, Technical:
  - Thuộc business domain nào?
  - Regulated compliance? (PCI-DSS, HIPAA...)
  - Tích hợp với hệ thống nào?
  - Có Swagger/API doc không?
  - Technology stack bị ràng buộc?
  - Team size và timeline?

  → Chờ user trả lời

ROUND 3 (3-5 câu) — Data, NFR, Gaps:
  - Dữ liệu nhạy cảm nào? (PII, tài chính)
  - SLA expectations? (uptime, response time)
  - Data retention?
  - [Bất kỳ gap nào còn lại từ checklist]

  → Chờ user trả lời → Generate output
```

### L2: Q&A Trung bình — 8-12 câu, 1-2 rounds

```
ROUND 1 (8-10 câu):
  - Cover tất cả checklist categories chưa có trong input
  - Focus: Integration, NFR, Compliance, Data

  → IF gaps remain → ROUND 2 (2-3 câu)
  → ELSE → Generate output
```

### L3: Q&A Nhẹ — 3-5 câu, 1 round

```
ROUND 1 (3-5 câu):
  - NFR có rõ ràng không?
  - Compliance requirements?
  - Integration points đầy đủ chưa?
  - Rollback/DR strategy?
  - API versioning?

  → Generate output
```

### L4: Skip Q&A
```
  Proceed directly to output generation.
  (Có thể hỏi 1-2 câu clarification nếu thấy gap critical)
```

## Step 0.3 — Generate Output

MUST use template: `templates/input/requirements_summary.tmpl.md`

Fill ALL sections in template:
1. Vision & Scope
2. Actors & Personas
3. Functional Requirements (FR-001, FR-002...)
4. Non-Functional Requirements (NFR-001...) — PHẢI có target cụ thể
5. Constraints (CON-001...)
6. Integration Points — với direction, protocol
7. Open Questions — bất kỳ điều gì chưa rõ

Create initial `requirements_traceability.md` using `templates/output/requirements_traceability.tmpl.md`.

## Input

| Source | Content |
|---|---|
| User | Raw input (any format: BRD, SRS, PRD, email, idea, swagger, UI mockup) |

## Output

| Artifact | Template | Description |
|---|---|---|
| `requirements_summary.md` | `templates/input/requirements_summary.tmpl.md` | Vision, actors, FR/NFR/Constraints, integration points |
| `requirements_traceability.md` | `templates/output/requirements_traceability.tmpl.md` | Initial matrix (Design/DB/API = TBD) |

## Sub-skills Used
- `brainstorming` (if L1 — Just Idea)
- `ask_question` (all levels except L4)

## Handoff to EA
Return `requirements_summary.md` + `requirements_traceability.md` to Main Agent for review.
**WAIT** for EA approval before Phase 0.5. Do NOT proceed automatically.

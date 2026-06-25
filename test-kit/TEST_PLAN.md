# 🧪 Test Kit — Architecture Design Plugin v1.0

> **Mục tiêu:** Kiểm tra plugin hoạt động đúng 12-phase pipeline  
> **Phương pháp:** 3 kịch bản tăng dần độ phức tạp  
> **Thời gian ước tính:** ~2h cho cả 3 kịch bản

---

## Tổng quan 3 Kịch bản

| # | Kịch bản | Input Level | Phases tested | Thời gian |
|:---:|---|:---:|---|:---:|
| TC-1 | Smoke Test — Skill Recognition | — | Skill loading | 5 min |
| TC-2 | Just Idea — Đặt phòng họp | L1 | P0 → P0.5 | 15 min |
| TC-3 | Full Pipeline — E-Wallet Top-up | L3 | P0 → P8 (all) | 1-2h |

---

## TC-1: Smoke Test — Skill Recognition

### Mục đích
Xác nhận plugin được load đúng trong conversation mới.

### Bước thực hiện

**Bước 1:** Mở conversation MỚI (bắt buộc)

**Bước 2:** Gõ prompt sau:

```
Bạn có biết plugin architecture-design không? 
Hãy liệt kê tất cả phases và sub-skills tương ứng.
```

### Kết quả mong đợi

| # | Kiểm tra | Pass criteria | ☐ |
|:---:|---|---|:---:|
| 1.1 | Agent nhận diện plugin | Trả lời "có" hoặc liệt kê được | ☐ |
| 1.2 | Liệt kê đủ 12 phases | P0, P0.5, P1-P8, RT | ☐ |
| 1.3 | Mô tả đúng vai trò EA | "Orchestrator", "giữ knowledge", "review" | ☐ |

**Bước 3:** Gõ tiếp:

```
Đọc file phases/phase-4-refinement/SKILL.md và tóm tắt parallel strategy cho tôi.
```

| # | Kiểm tra | Pass criteria | ☐ |
|:---:|---|---|:---:|
| 1.4 | Agent đọc được sub-skill | Tóm tắt nội dung phase 4 | ☐ |
| 1.5 | Mô tả parallel | "2 subagents DB + API song song, reviewer, loop max 3" | ☐ |

**PASS TC-1:** ≥ 4/5 checks pass

---

## TC-2: Just Idea — Hệ thống Đặt phòng họp

### Mục đích
Test Phase 0 (Q&A loop) + Phase 0.5 (Enterprise Context) + Input classification.

### Bước thực hiện

**Bước 1:** Mở conversation MỚI

**Bước 2:** Gõ prompt:

```
/architecture-design

Tôi muốn xây dựng hệ thống đặt phòng họp nội bộ cho công ty.
Nhân viên có thể xem lịch trống, đặt phòng, hủy đặt.
Có thêm màn hình TV trước mỗi phòng hiển thị lịch đặt phòng hôm nay.
```

### Phase 0: Intake — Kỳ vọng

| # | Kiểm tra | Pass criteria | ☐ |
|:---:|---|---|:---:|
| 2.1 | Classify L1 | Agent xác nhận đây là "Just Idea" / Level 1 | ☐ |
| 2.2 | Đọc phase-0 skill | Agent tham chiếu intake SKILL.md | ☐ |
| 2.3 | Q&A bắt đầu | Agent hỏi batch câu hỏi đầu tiên (5-8 câu) | ☐ |
| 2.4 | Hỏi đúng category | Bao gồm: scope, actors, integration, constraints | ☐ |

**Trả lời Q&A (cung cấp cho agent):**

```
1. Scope: MVP — chỉ đặt phòng, chưa cần đặt thiết bị (projector, whiteboard)
2. Actors: 
   - Nhân viên (web app) — đặt/hủy phòng
   - Admin (web) — quản lý phòng, cấu hình
   - TV Display (web kiosk) — hiển thị lịch phòng
3. Authentication: SSO qua Azure AD (đã có hệ thống)
4. Integration:
   - Azure AD (SSO + danh sách nhân viên)
   - Google Calendar API (sync lịch cá nhân)
   - Email service (gửi xác nhận booking)
5. Stack: React + NestJS + PostgreSQL (công ty dùng chuẩn này)
6. Team: 2 BE + 1 FE, timeline 6 tuần
7. NFR: max 50 concurrent users, response < 1s
8. Data: không có PII nhạy cảm, retention 1 năm
9. Compliance: không yêu cầu đặc biệt
```

| # | Kiểm tra | Pass criteria | ☐ |
|:---:|---|---|:---:|
| 2.5 | Q&A ≤ 3 rounds | Không hỏi quá 3 rounds sau khi trả lời | ☐ |
| 2.6 | Output requirements_summary | Tạo artifact đúng template format | ☐ |
| 2.7 | FR liệt kê đủ | ≥ 5 FR: đặt phòng, hủy, xem lịch, admin, TV display | ☐ |
| 2.8 | NFR có target | Performance, availability có số cụ thể | ☐ |
| 2.9 | Integration points | Azure AD, Google Calendar, Email — 3 điểm | ☐ |

### Phase 0.5: Enterprise Context — Kỳ vọng

| # | Kiểm tra | Pass criteria | ☐ |
|:---:|---|---|:---:|
| 2.10 | Hỏi enterprise context | Agent hỏi về landscape, existing services | ☐ |
| 2.11 | Map dependencies | Upstream: Azure AD. Downstream: Google, Email | ☐ |

**Cung cấp context:**

```
Enterprise context:
- Công ty dùng Azure cloud, K8s, PostgreSQL
- Existing: Auth Service (Azure AD B2C), Notification Service (SendGrid), 
  Employee Portal (React SPA)
- Monitoring: Azure Application Insights
- Team topology: product team, 3-5 devs, stream-aligned
```

| # | Kiểm tra | Pass criteria | ☐ |
|:---:|---|---|:---:|
| 2.12 | Output enterprise_context | Tạo artifact với landscape diagram | ☐ |
| 2.13 | Dependency matrix | ≥ 3 services mapped | ☐ |

**Dừng ở đây cho TC-2.**

Gõ: `Dừng ở Phase 0.5. Tôi muốn review output trước khi tiếp.`

### Đánh giá TC-2

**PASS TC-2:** ≥ 10/13 checks pass

---

## TC-3: Full Pipeline — E-Wallet Top-up Service

### Mục đích
Test TOÀN BỘ 12 phases, bao gồm parallel subagents, review loop, migration plan.

### Input: BRD + Enterprise Context (Level 3)

**Bước 1:** Mở conversation MỚI

**Bước 2:** Copy toàn bộ prompt dưới đây:

````
/architecture-design

## BRD: E-Wallet Top-up Service

### 1. Business Objective
Xây dựng microservice xử lý nạp tiền (top-up) cho ví điện tử nội bộ.
Khách hàng nạp tiền qua nhiều kênh (bank transfer, QR code, thẻ cào).
Service này là backend, không có UI riêng — được gọi từ Mobile App BFF.

### 2. Actors
- **Mobile App** (gọi qua BFF) — người dùng cuối
- **Admin Portal** — xem lịch sử, refund, cấu hình limit
- **Payment Gateway** — VNPay, Momo (downstream)
- **Core Banking** — ghi nhận balance (downstream)
- **Notification Service** — push/SMS xác nhận (downstream)

### 3. Functional Requirements
- FR-001: Tạo top-up request (chọn kênh, nhập số tiền)
- FR-002: Redirect tới payment gateway → nhận callback
- FR-003: Xác nhận top-up thành công → cập nhật balance qua Core Banking
- FR-004: Gửi notification sau khi hoàn tất
- FR-005: Admin xem lịch sử top-up (filter theo user, status, date range)
- FR-006: Admin thực hiện refund (partial/full)
- FR-007: Rate limiting — max 5 top-up/user/ngày
- FR-008: Min 10,000 VND, Max 50,000,000 VND per transaction

### 4. Non-Functional Requirements
- NFR-001: Response time P99 < 500ms (excluding payment gateway)
- NFR-002: Throughput 100 TPS peak
- NFR-003: Availability 99.95%
- NFR-004: PII encryption (phone, bank account)
- NFR-005: Audit log mọi transaction
- NFR-006: Idempotency — retry safe

### 5. Constraints
- CON-001: Stack bắt buộc: Java 17 + Spring Boot 3 + PostgreSQL 16
- CON-002: Deploy trên K8s, shared cluster
- CON-003: Event-driven communication via Kafka
- CON-004: Tuân thủ PCI-DSS cho payment data

### 6. Integration Points
| System | Direction | Protocol | Owner |
|---|---|---|---|
| Mobile App BFF | Upstream (calls us) | REST | Mobile Team |
| VNPay | Downstream | REST + Webhook | External |
| Momo | Downstream | REST + Webhook | External |
| Core Banking API | Downstream | gRPC | Core Team |
| Notification Service | Downstream | Kafka event | Platform Team |
| Audit Service | Downstream | Kafka event | Platform Team |

### 7. Enterprise Context
- Thuộc hệ sinh thái **Digital Wallet Platform**
- Existing services: Auth (Keycloak), API Gateway (Kong), Config Server (Spring Cloud)
- Shared infra: PostgreSQL cluster, Redis cluster, Kafka cluster, ELK stack
- Monitoring: Prometheus + Grafana, Jaeger tracing
- Team: Wallet Team — 4 BE + 1 QA, stream-aligned
- Business domain: Payment & Settlement bounded context
````

### Checklist theo Phase

#### Phase 0: Intake

| # | Kiểm tra | Pass criteria | ☐ |
|:---:|---|---|:---:|
| 3.1 | Classify L3 | "Structured BRD" / Level 3 | ☐ |
| 3.2 | Q&A nhẹ | ≤ 5 câu hỏi bổ sung (rollback? DR? API versioning?) | ☐ |
| 3.3 | Output requirements_summary | Đúng template, đủ FR/NFR/CON | ☐ |
| 3.4 | Traceability created | Initial matrix với 8 FR + 6 NFR + 4 CON | ☐ |

**Trả lời Q&A nếu agent hỏi:**
```
- Rollback strategy: refund gọi lại payment gateway, reverse Core Banking entry
- DR: standard — RPO < 5min (Kafka replay), RTO < 30min
- API versioning: URL path (/v1/, /v2/)
- Rate limit storage: Redis (shared cluster)
- Webhook retry: payment gateway retry 3 lần, 30s interval
```

#### Phase 0.5: Enterprise Context

| # | Kiểm tra | Pass criteria | ☐ |
|:---:|---|---|:---:|
| 3.5 | Skip Q&A hoặc minimal | Context đã cung cấp trong BRD §7 | ☐ |
| 3.6 | Landscape diagram | Mermaid diagram có ≥ 6 services | ☐ |
| 3.7 | Dependency matrix đủ | 6 integration points mapped | ☐ |

#### Phase 1: Discovery

| # | Kiểm tra | Pass criteria | ☐ |
|:---:|---|---|:---:|
| 3.8 | Knowledge base created | Entities: TopUpRequest, Transaction, RefundRecord | ☐ |
| 3.9 | Business rules extracted | Rate limit, min/max amount, idempotency | ☐ |
| 3.10 | NFR matrix formalized | 6 NFRs với target cụ thể | ☐ |

#### Phase 2: Foundation

| # | Kiểm tra | Pass criteria | ☐ |
|:---:|---|---|:---:|
| 3.11 | TO-BE sequence diagram | BFF → TopUp Service → Payment GW → callback → Core Banking | ☐ |
| 3.12 | Pattern decision | Event-driven + REST hybrid (justified) | ☐ |
| 3.13 | Interaction matrix | ≥ 6 rows (1 upstream + 5 downstream) | ☐ |

#### Phase 3: Domain Design

| # | Kiểm tra | Pass criteria | ☐ |
|:---:|---|---|:---:|
| 3.14 | C4 L1 | System context diagram | ☐ |
| 3.15 | C4 L2 | Container: App, DB, Redis, Kafka | ☐ |
| 3.16 | C4 L3 | Components: Controller, Service, Repository, EventPublisher | ☐ |
| 3.17 | State machine | ≥ 6 states: INITIATED→PENDING→PROCESSING→SUCCESS/FAILED→REFUNDED | ☐ |
| 3.18 | Webhook transition | Payment callback triggers PROCESSING→SUCCESS/FAILED | ☐ |

#### Phase 4: Refinement

| # | Kiểm tra | Pass criteria | ☐ |
|:---:|---|---|:---:|
| 3.19 | DB design created | ERD + DDL + indexes | ☐ |
| 3.20 | ≥ 3 tables | topup_requests, transactions, refunds (minimum) | ☐ |
| 3.21 | PII encryption noted | phone, bank_account columns | ☐ |
| 3.22 | API design created | Endpoint table + per-endpoint detail | ☐ |
| 3.23 | ≥ 5 endpoints | create, callback, get-status, list (admin), refund | ☐ |
| 3.24 | Idempotency design | idempotency_key trong request hoặc DB constraint | ☐ |
| 3.25 | Review loop | ≥ 1 review iteration | ☐ |

#### Phase 5: Documentation

| # | Kiểm tra | Pass criteria | ☐ |
|:---:|---|---|:---:|
| 3.26 | Master doc created | ≥ 10 sections | ☐ |
| 3.27 | Self-contained | Có C4, state, API, DB, security sections | ☐ |
| 3.28 | Size reasonable | 20-60KB | ☐ |

#### Phase 6: Review

| # | Kiểm tra | Pass criteria | ☐ |
|:---:|---|---|:---:|
| 3.29 | Review score | ≥ 85/100 (first round) | ☐ |
| 3.30 | EA checks | 5 checks documented | ☐ |
| 3.31 | Findings documented | Severity + fix suggestion per finding | ☐ |
| 3.32 | ADRs generated | ≥ 3 ADRs (pattern, DB, payment integration) | ☐ |
| 3.33 | Fix loop | Agent tự fix findings rồi re-review | ☐ |
| 3.34 | Final score | ≥ 90/100 after fix loop | ☐ |

#### Phase 6.5: Migration

| # | Kiểm tra | Pass criteria | ☐ |
|:---:|---|---|:---:|
| 3.35 | Deployment strategy | Canary/Blue-Green with rationale | ☐ |
| 3.36 | DB migration scripts | Ordered list V001, V002... | ☐ |
| 3.37 | Pre-go-live checklist | Technical + Business items | ☐ |
| 3.38 | Risk assessment | ≥ 3 risks with mitigation | ☐ |

#### Phase 7: Delivery

| # | Kiểm tra | Pass criteria | ☐ |
|:---:|---|---|:---:|
| 3.39 | README generated | File index + reading order | ☐ |
| 3.40 | Package complete | ≥ 8 files referenced | ☐ |

#### Phase 8: Handover

| # | Kiểm tra | Pass criteria | ☐ |
|:---:|---|---|:---:|
| 3.41 | Architecture contract | Constraints + deviation process | ☐ |
| 3.42 | RACI matrix | Per component ownership | ☐ |
| 3.43 | Runbook | Health monitoring + common scenarios | ☐ |
| 3.44 | Change classification | 5 levels (Cosmetic→Architecture) | ☐ |
| 3.45 | KT checklist | ≥ 5 handover items | ☐ |

#### Requirements Tracking

| # | Kiểm tra | Pass criteria | ☐ |
|:---:|---|---|:---:|
| 3.46 | Traceability updated | Matrix updated qua các phases | ☐ |
| 3.47 | Coverage ≥ 90% | Must-have reqs all mapped | ☐ |
| 3.48 | Gap alerts | Flagged nếu có missing | ☐ |

### Đánh giá TC-3

**PASS TC-3:** ≥ 38/48 checks pass (≥ 80%)

---

## Bảng Chấm điểm Tổng hợp

### Per-Phase Score

| Phase | Max checks | Pass target | Weight |
|:---:|:---:|:---:|:---:|
| TC-1: Smoke | 5 | ≥ 4 | 10% |
| TC-2: P0+P0.5 | 13 | ≥ 10 | 20% |
| TC-3: Full pipeline | 48 | ≥ 38 | 70% |
| **Total** | **66** | **≥ 52** | **100%** |

### Quality Dimensions

| Dimension | Checks | Description |
|---|---|---|
| **Skill Loading** | 1.1-1.5 | Plugin nhận diện, sub-skill đọc đúng |
| **Input Classification** | 2.1, 3.1 | L1/L3 classify chính xác |
| **Q&A Quality** | 2.3-2.5, 3.2 | Hỏi đúng, không hỏi dư |
| **Template Compliance** | 2.6, 3.3, 3.6 | Output đúng template format |
| **Architecture Depth** | 3.14-3.18 | C4 + State machine đầy đủ |
| **Design Consistency** | 3.19-3.25 | DB ↔ API cross-validation |
| **Review Loop** | 3.29-3.34 | Score, findings, fix, re-review |
| **TOGAF Alignment** | 3.35-3.45 | Migration, Handover, Governance |
| **Traceability** | 3.46-3.48 | Requirements coverage maintained |

### Verdict

| Score Range | Verdict | Action |
|---|---|---|
| ≥ 55/66 (83%) | 🟢 **PASS** | Plugin production-ready |
| 45-54 (68-83%) | 🟡 **CONDITIONAL** | Fix specific phases, re-test failed areas |
| < 45 (< 68%) | 🔴 **FAIL** | Review SKILL.md instructions, major revision needed |

---

## Phụ lục: Các lỗi thường gặp và cách debug

| Symptom | Root Cause | Fix |
|---|---|---|
| Agent không follow pipeline | Main SKILL.md không được đọc | Kiểm tra plugin.json → skills/SKILL.md path |
| Skip Q&A khi nên hỏi | L1 bị classify thành L4 | Cập nhật classification criteria trong P0 SKILL.md |
| Template bị ignore | Agent không view_file template | Thêm "MUST read template at [path]" vào phase SKILL.md |
| Review score luôn = 100 | Agent tự review thiếu khách quan | Tách reviewer subagent riêng, cấm self-review |
| Phase 6.5/8 bị skip | Agent không biết phases mới | Kiểm tra main SKILL.md liệt kê đủ 12 phases |
| Traceability không update | EA quên update giữa phases | Thêm explicit trigger "UPDATE traceability" cuối mỗi phase |
| Parallel không hoạt động | Agent chạy sequential | Thêm "SPAWN parallel subagents" instruction rõ ràng |

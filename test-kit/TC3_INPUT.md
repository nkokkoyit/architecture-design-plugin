# 🧪 TC-3 Input — E-Wallet Top-up Service (Copy & Paste)

Sử dụng prompt này để test full pipeline. Copy toàn bộ nội dung dưới đây vào conversation mới.

---

## Prompt (copy từ đây)

```
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
```

---

## Q&A Answers (nếu agent hỏi)

Copy trả lời sau nếu agent hỏi câu hỏi bổ sung:

```
Trả lời bổ sung:

1. Rollback/Refund strategy: 
   - Refund gọi lại payment gateway API (POST /refunds)
   - Reverse entry trên Core Banking (gRPC ReverseTransaction)
   - Cần approval nếu refund > 5,000,000 VND

2. Disaster Recovery:
   - RPO < 5 min (Kafka replay từ committed offset)
   - RTO < 30 min (K8s auto-restart + DB failover)

3. API versioning: URL path based (/v1/, /v2/)

4. Rate limit storage: Redis (shared cluster, key: user_id:date, TTL 24h)

5. Webhook retry policy: 
   - Payment gateway retry 3 lần, interval 30s → 60s → 120s
   - Nếu 3 lần fail → mark PENDING_MANUAL_CHECK

6. Concurrency control: 
   - Optimistic locking trên transaction record
   - Distributed lock (Redis) cho rate limit check

7. Monitoring SLO:
   - Success rate > 99.5%
   - P99 latency < 500ms
   - Error budget: 0.05% / tháng
```

---

## Phase Transition Prompts

Nếu agent dừng giữa chừng hoặc hỏi có muốn tiếp tục:

```
# Sau Phase 0 + 0.5:
Tiếp tục Phase 1 Discovery. Phân tích entities và business rules.

# Sau Phase 3:
Tiếp tục Phase 4. Thiết kế database và API chi tiết.

# Sau Phase 4:
Tiếp tục Phase 5-6. Tổng hợp master document rồi review.

# Sau Phase 6:
Tiếp tục Phase 6.5-8. Migration plan, delivery package, và handover.

# Nếu agent hỏi có skip phase nào:
Chạy full pipeline, không skip phase nào. Tôi muốn test hết.
```

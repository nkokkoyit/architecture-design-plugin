# 🚀 Hướng dẫn Cài đặt, Sử dụng & Test Skill Architecture-Design

---

## 1. Cài đặt Skill

### Cách 1: Copy vào Global Skills (Khuyến nghị)

```powershell
# Copy skill vào thư mục global config
Copy-Item -Path "D:\Job\Source\mblskill\architecture-design" `
          -Destination "C:\Users\thangtt\.gemini\config\skills\architecture-design" `
          -Recurse -Force

# Verify
Get-ChildItem "C:\Users\thangtt\.gemini\config\skills\architecture-design\SKILL.md"
```

> Sau khi copy, skill sẽ **tự động được phát hiện** trong mọi conversation mới.

### Cách 2: Symlink (không duplicate files)

```powershell
# Tạo symbolic link
New-Item -ItemType SymbolicLink `
  -Path "C:\Users\thangtt\.gemini\config\skills\architecture-design" `
  -Target "D:\Job\Source\mblskill\architecture-design"
```

### Cách 3: Đăng ký qua skills.json (cho shared teams)

Tạo file `C:\Users\thangtt\.gemini\config\skills.json`:

```json
{
  "entries": [
    { "path": "D:/Job/Source/mblskill" }
  ]
}
```

### Xác nhận cài đặt

Mở conversation mới, gõ bất kỳ câu nào liên quan tới "thiết kế kiến trúc" — agent sẽ tự nhận diện skill `architecture-design` và đọc SKILL.md.

---

## 2. Cách Sử dụng

### Full Pipeline (mặc định)

```
/architecture-design [yêu cầu của bạn]
```
→ Chạy toàn bộ 12 phases, từ phân tích yêu cầu đến đóng gói delivery.

### Per-Stage Commands (chạy từng giai đoạn)

| Command | Phases | Khi nào dùng |
|---|:---:|---|
| `/arch-intake` | P0 + P0.5 | Chỉ muốn phân tích yêu cầu + xác định enterprise context |
| `/arch-design` | P1 → P4 | Đã có requirements, cần thiết kế C4 + DB + API |
| `/arch-review` | P5 + P6 | Đã có design, cần tổng hợp document + review chất lượng |
| `/arch-deliver` | P6.5 → P8 | Đã có master doc, cần migration plan + đóng gói + handover |

**Ví dụ:**
```
# Chỉ chạy intake
/arch-intake Tôi muốn xây dựng hệ thống đặt phòng họp

# Tiếp tục design từ kết quả intake trước
/arch-design @[path/to/requirements_summary.md] @[path/to/enterprise_context.md]

# Review document có sẵn
/arch-review @[path/to/c4_models.md] @[path/to/database_design.md] @[path/to/api_design.md]
```

### Gọi tự nhiên (agent tự detect)

```
Thiết kế kiến trúc cho hệ thống quản lý đơn hàng online
```

### Flow hoạt động

```
Bạn gửi yêu cầu
    ↓
Agent đọc SKILL.md (Main Orchestrator)
    ↓
Phase 0: Agent phân loại input (L1-L4)
    ↓
Phase 0: Q&A loop nếu cần
    ↓
Phase 0.5: Hỏi enterprise context
    ↓
Phase 1-7: Agent tự điều phối qua các phases
    ↓
Phase 6: Review + scoring
    ↓
Phase 7: Đóng gói delivery package
    ↓
Phase 8: Handover & governance doc
```

---

## 3. Test Scenarios

### 📋 Test 1: Level 1 — Just Idea (Đơn giản nhất)

**Mục đích:** Test Phase 0 Q&A loop + input classification

**Prompt:**
```
/architecture-design

Tôi muốn xây dựng một hệ thống đặt lịch khám bệnh online cho phòng khám tư nhân.
Bệnh nhân có thể tự đặt lịch qua web, chọn bác sĩ, chọn khung giờ.
```

**Kỳ vọng:**
- [ ] Agent classify input = **Level 1** (Just Idea)
- [ ] Agent đọc `phases/phase-0-intake/SKILL.md`
- [ ] Agent hỏi Q&A theo checklist trong `templates/input/intake_checklist.tmpl.md`
- [ ] Hỏi ít nhất 10-15 câu: scope, actors, domain, integrations, constraints
- [ ] Tạo `requirements_summary.md` theo template
- [ ] Tạo initial `requirements_traceability.md`

**Trả lời Q&A mẫu:**
```
- Greenfield, MVP trước
- Web only, không cần mobile
- Không cần login (đặt bằng SĐT + OTP)
- Tích hợp: Google Calendar API, SMS gateway
- Stack: Node.js, PostgreSQL
- Team: 3 dev, timeline 2 tháng
- Compliance: không có yêu cầu đặc biệt
```

**Đánh giá:**
| Tiêu chí | Pass? |
|---|---|
| Classify đúng L1 | ☐ |
| Q&A đầy đủ checklist items | ☐ |
| Output đúng template format | ☐ |
| FR/NFR/Constraints listed | ☐ |
| Open questions identified | ☐ |

---

### 📄 Test 2: Level 3 — BRD (Trung bình)

**Mục đích:** Test Phase 0 + Phase 0.5 + Phase 1-3

**Prompt:**
```
/architecture-design

BRD: Hệ thống Loyalty Points cho chuỗi cửa hàng café

1. Business Objective:
   - Tăng tỷ lệ khách quay lại từ 30% lên 50%
   - Mỗi đơn hàng tích điểm 1% giá trị
   - Điểm dùng để đổi voucher giảm giá

2. Actors:
   - Khách hàng (app mobile)
   - Nhân viên POS (scan QR)
   - Admin (quản lý campaign)

3. Integrations:
   - POS system (REST API, swagger attached)
   - Payment gateway (VNPay)
   - Push notification service

4. Constraints:
   - Dùng chung infrastructure hiện có: K8s, PostgreSQL, Redis
   - Team: 5 backend + 2 frontend

Swagger POS: [đính kèm file nếu có]
```

**Kỳ vọng:**
- [ ] Classify = **Level 3** (Structured BRD)
- [ ] Q&A nhẹ (3-5 câu): NFR? Compliance? Rollback?
- [ ] Phase 0.5: Hỏi about system landscape, existing services
- [ ] Phase 1: Phân tích swagger (nếu có), extract entities
- [ ] Phase 2: Integration flow POS → Loyalty → Payment
- [ ] Phase 3: C4 diagrams + State machine (point lifecycle)

---

### 📦 Test 3: Level 4 — Full PRD + Swagger (End-to-end)

**Mục đích:** Test toàn bộ pipeline 12 phases

**Prompt:**
```
/architecture-design

Thiết kế kiến trúc hoàn chỉnh cho hệ thống bảo hiểm online.

PRD: @[path/to/prd.md]
UI: @[path/to/mockup.html]
Swagger Term Life: @[path/to/swagger/term-life.json]
Swagger Partner: @[path/to/swagger/partner-service.json]

Enterprise context:
- Thuộc hệ sinh thái Digital Insurance Platform
- Existing services: Auth, Notification, Payment, File Storage
- Stack: Java Spring Boot, PostgreSQL 16, Redis, K8s
- Team: 4 BE + 2 FE, own by Digital Products team
```

**Kỳ vọng (full pipeline):**
- [ ] Phase 0: Classify L4, skip Q&A
- [ ] Phase 0.5: Map landscape từ context provided
- [ ] Phase 1: Parallel subagents phân tích UI + Swagger
- [ ] Phase 2: AS-IS → TO-BE flow
- [ ] Phase 3: C4 L1-L4 + State machine
- [ ] Phase 4: DB design + API design + peer review loop
- [ ] Phase 5: Master architecture document
- [ ] Phase 6: Review ≥ 90/100 + ADRs
- [ ] Phase 6.5: Migration plan
- [ ] Phase 7: Delivery package
- [ ] Phase 8: Handover doc
- [ ] RT: Traceability matrix ≥ 90% coverage

---

## 4. Kiểm tra Agent đọc đúng Skill

### Debug: Xác nhận skill loaded

Trong chat, hỏi:
```
Bạn có biết skill architecture-design không? Liệt kê các phases.
```

Agent nên trả lời đúng 12 phases (0 → 8 + RT).

### Debug: Xác nhận đọc phase skill

```
Hãy đọc phases/phase-4-refinement/SKILL.md và tóm tắt parallel strategy.
```

Agent nên trả lời: "2 subagents (DB + API) chạy song song, sau đó 1 reviewer, loop max 3 lần."

### Debug: Xác nhận template

```
Hãy đọc templates/output/migration_plan.tmpl.md và liệt kê các sections.
```

Agent nên liệt kê: Deployment Strategy, Deployment Sequence, DB Migration, Integration Coordination, Pre-Go-Live Checklist, Risk Assessment, Rollout Phases.

---

## 5. Tips sử dụng hiệu quả

### 5.1 Cung cấp Enterprise Context sớm

```
/architecture-design

[Yêu cầu]

Enterprise context của công ty:
- Hệ sinh thái: 30+ microservices trên K8s
- Shared: API Gateway (Kong), Auth (Keycloak), Notification (FCM + SMS)
- Stack: Java 17, Spring Boot 3, PostgreSQL, Redis, Kafka
- Monitoring: Prometheus + Grafana, Jaeger tracing
- Team topology: stream-aligned teams, 4-6 devs each
```

→ Giúp agent skip Phase 0.5 Q&A, đi thẳng vào design.

### 5.2 Incremental — Không cần chạy hết 1 lần

```
# Chạy Phase 0-3 trước
/architecture-design thiết kế kiến trúc cho [X] — dừng ở Phase 3 để tôi review C4

# Sau khi approve, tiếp tục
Continue Phase 4-7
```

### 5.3 Skip phases không cần

```
/architecture-design thiết kế API cho [X]
— Skip Phase 3 (đã có C4)
— Skip Phase 6.5 (chưa cần migration plan)
— Focus: Phase 4 (DB + API) + Phase 5 (Document) + Phase 6 (Review)
```

### 5.4 Tham chiếu artifacts có sẵn

```
/architecture-design

Redesign payment module dựa trên:
- DB hiện tại: @[path/to/current_db_design.md]
- API hiện tại: @[path/to/current_api.md]
- Swagger mới: @[path/to/new_swagger.json]
```

---

## 6. Checklist đánh giá chất lượng Skill

Sau mỗi test, đánh giá:

| # | Tiêu chí | Score (1-5) |
|:---:|---|:---:|
| 1 | Agent classify input level đúng? | ☐ |
| 2 | Q&A đầy đủ, không hỏi dư? | ☐ |
| 3 | Output artifacts đúng template format? | ☐ |
| 4 | Phase sequencing đúng dependency? | ☐ |
| 5 | Parallel execution khi có thể? | ☐ |
| 6 | EA review trước khi chuyển phase? | ☐ |
| 7 | Cross-artifact consistency? | ☐ |
| 8 | Requirements traceability maintained? | ☐ |
| 9 | Review score ≥ 90? | ☐ |
| 10 | Delivery package đầy đủ? | ☐ |

**Target:** Average ≥ 4.0/5.0 trên 3 test scenarios.

---

## 7. Troubleshooting

| Vấn đề | Nguyên nhân | Fix |
|---|---|---|
| Agent không nhận skill | SKILL.md không ở đúng thư mục | Verify path: `~/.gemini/config/skills/architecture-design/SKILL.md` |
| Agent không đọc phase skills | Không có `view_file` vào `phases/` | Thêm instruction: "Read phase skill before delegating" |
| Template không được dùng | Agent bỏ qua template | Thêm explicit: "MUST use template at [path]" |
| Subagent không write được | Permission denied | Grant write permission cho output directory |
| Q&A quá nhiều rounds | Input classification sai | Check L1-L4 criteria alignment |

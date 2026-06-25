# EA Review Lens — 5 Enterprise Architecture Alignment Checks

Sử dụng 5 câu hỏi này tại Phase 6 (Review) để đánh giá architecture alignment với enterprise context.

## EA-1: Bounded Context Alignment

**Question:** Service mới có align với Bounded Context đã define không?

**Pass criteria:**
- Không cross-context data coupling (không share DB với service khác)
- Entity ownership rõ ràng (mỗi entity chỉ thuộc 1 context)
- Communication giữa contexts qua API hoặc events (không qua shared state)

**Red flags:**
- Shared database tables giữa services
- Direct DB access from another service
- Circular dependencies giữa contexts

---

## EA-2: Data Ownership

**Question:** Data ownership có rõ ràng không?

**Pass criteria:**
- Mỗi entity có 1 service là master owner
- Other services chỉ hold cached/derived copies
- Source-of-truth documented rõ ràng

**Red flags:**
- 2+ services cùng write vào same entity
- Không rõ ai là source-of-truth
- Shared database anti-pattern

---

## EA-3: Team Topology Fit

**Question:** Deployment strategy có align với team topology không?

**Pass criteria:**
- Service boundaries align với team boundaries (Conway's Law)
- Team có đủ autonomy deploy independently
- Clear ownership → clear accountability

**Red flags:**
- 1 service requires 3+ teams to deploy
- No clear owner for the service
- Deployment blocked by external team

---

## EA-4: Observability Integration

**Question:** Monitoring/alerting có integrate vào enterprise platform chung không?

**Pass criteria:**
- Logs/metrics/traces gửi về central platform
- Alert routing theo on-call schedule chung
- Dashboards accessible cho relevant stakeholders
- Correlation IDs for distributed tracing

**Red flags:**
- Separate monitoring silo
- No alerting configured
- No distributed tracing

---

## EA-5: API Versioning & Deprecation

**Question:** API versioning strategy có documented không?

**Pass criteria:**
- Version strategy defined (URL path, header, etc.)
- Breaking change policy documented
- Deprecation timeline for old versions
- Consumer notification process

**Red flags:**
- No versioning at all
- Breaking changes without notice
- No deprecation policy

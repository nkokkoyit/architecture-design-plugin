# Intake Checklist — Q&A per Input Level

## Level 1 — Just Idea (15-20 câu)

### Vision & Scope
- [ ] Mô tả ngắn gọn ý tưởng trong 2-3 câu?
- [ ] Giải quyết vấn đề gì cho ai?
- [ ] Greenfield (mới) hay extension (mở rộng hệ thống hiện có)?
- [ ] MVP scope vs Full vision?

### Actors & Users
- [ ] Ai sẽ sử dụng hệ thống? (roles, personas)
- [ ] Kênh truy cập? (Web, Mobile, API, Internal)
- [ ] Cần authentication không? Loại gì?

### Business Domain
- [ ] Thuộc business domain nào?
- [ ] Có regulated compliance không? (PCI-DSS, HIPAA, Insurance...)
- [ ] SLA expectations? (uptime, response time)

### Integration
- [ ] Tích hợp với hệ thống nào?
- [ ] Có Swagger/API doc không?
- [ ] Ai owns các services tích hợp?

### Constraints
- [ ] Technology stack bị ràng buộc không?
- [ ] Timeline? Budget level?
- [ ] Team size và skillset?

### Data
- [ ] Dữ liệu nhạy cảm nào? (PII, tài chính)
- [ ] Data retention requirements?
- [ ] Existing data sources?

---

## Level 2 — Semi-structured (8-12 câu)

- [ ] Scope rõ ràng chưa? Ranh giới nào?
- [ ] Actors đầy đủ chưa?
- [ ] Integration points có API doc không?
- [ ] NFR (performance, security) được đề cập chưa?
- [ ] Data retention / compliance?
- [ ] Deployment environment?
- [ ] Team topology?
- [ ] Timeline constraints?

---

## Level 3 — Structured BRD (3-5 câu)

- [ ] Integration points có đầy đủ không?
- [ ] NFR có rõ ràng không?
- [ ] Compliance requirements?
- [ ] Rollback strategy?

---

## Level 4 — Complete PRD+Swagger

Skip Q&A. Proceed directly to output.

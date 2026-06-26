# EA Review Lens — 5 Enterprise Architecture Alignment Checks

Use these 5 questions at Phase 6 (Review) to evaluate architecture alignment with the enterprise context.

## EA-1: Bounded Context Alignment

**Question:** Does the new service align with the defined Bounded Context?

**Pass criteria:**
- No cross-context data coupling (no shared DB with other services)
- Entity ownership is clear (each entity belongs to only 1 context)
- Communication between contexts via API or events (not via shared state)

**Red flags:**
- Shared database tables between services
- Direct DB access from another service
- Circular dependencies between contexts

---

## EA-2: Data Ownership

**Question:** Is data ownership clearly defined?

**Pass criteria:**
- Each entity has 1 service as the master owner
- Other services only hold cached/derived copies
- Source-of-truth is clearly documented

**Red flags:**
- 2+ services writing to the same entity
- Unclear who is the source-of-truth
- Shared database anti-pattern

---

## EA-3: Team Topology Fit

**Question:** Does the deployment strategy align with the team topology?

**Pass criteria:**
- Service boundaries align with team boundaries (Conway's Law)
- Team has sufficient autonomy to deploy independently
- Clear ownership → clear accountability

**Red flags:**
- 1 service requires 3+ teams to deploy
- No clear owner for the service
- Deployment blocked by external team

---

## EA-4: Observability Integration

**Question:** Is monitoring/alerting integrated into the shared enterprise platform?

**Pass criteria:**
- Logs/metrics/traces sent to central platform
- Alert routing follows the shared on-call schedule
- Dashboards accessible to relevant stakeholders
- Correlation IDs for distributed tracing

**Red flags:**
- Separate monitoring silo
- No alerting configured
- No distributed tracing

---

## EA-5: API Versioning & Deprecation

**Question:** Is the API versioning strategy documented?

**Pass criteria:**
- Version strategy defined (URL path, header, etc.)
- Breaking change policy documented
- Deprecation timeline for old versions
- Consumer notification process

**Red flags:**
- No versioning at all
- Breaking changes without notice
- No deprecation policy

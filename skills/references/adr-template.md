# ADR Template — MADR Format

Sử dụng template này cho mọi Architecture Decision Record.

## Template

```markdown
# ADR-[NUMBER]: [Title]

**Status:** [Proposed / Accepted / Deprecated / Superseded by ADR-XXX]  
**Date:** [YYYY-MM-DD]  
**Deciders:** [List of people/roles involved]

## Context

[Describe the situation, the forces at play, and why a decision is needed.
Include technical constraints, business requirements, and team context.]

## Decision

[State the decision clearly. Start with "We will..." or "Chúng tôi sẽ..."]

## Alternatives Considered

| Option | Pros | Cons |
|---|---|---|
| [Option A — Chosen] | [pros] | [cons] |
| [Option B] | [pros] | [cons] |
| [Option C] | [pros] | [cons] |

## Consequences

### Positive
- [Consequence 1]
- [Consequence 2]

### Negative
- [Trade-off 1]
- [Trade-off 2]

### Risks
- [Risk 1 and mitigation]
```

## Usage Notes

- ADR numbers are sequential (ADR-001, ADR-002...)
- Once Accepted, an ADR should NOT be modified — create a new ADR that supersedes it
- Keep Context section factual, not opinionated
- Decision section should be actionable ("We will...")
- Always document at least 2 alternatives considered

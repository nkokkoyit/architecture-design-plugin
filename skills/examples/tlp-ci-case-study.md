# Case Study: TLP-CI Agency D2C

> **Project:** Online insurance purchase (Term Life + Critical Illness)
> **Duration:** 10 working days, 5 sessions
> **Output:** 9 documents, 170KB, Review Score 95/100
> **Skills Used:** 8 unique skills, 4 subagents

## Pipeline Execution

| Phase | Duration | Key Activity | Output |
|:---:|---|---|---|
| 0 | 2h | Analyze PRD + HTML UI (L4 input) | Requirements breakdown |
| 0.5 | (implicit) | Learned landscape during Phase 6 | — |
| 1 | 3h | 3 parallel subagents (UI, Swagger×3) | Knowledge base |
| 2 | 1h | AS-IS vs TO-BE integration flows | Integration architecture |
| 3 | 3h | C4 L1-L4 + 17-state machine | Architecture models |
| 4 | 4h | 12 tables + 14 API endpoints + 4 review loops | DB + API design |
| 5 | 2h | Master architecture document (52KB) | Master doc v3.1 |
| 6 | 3h | 3 review rounds (85→92→95) + 20 ADRs | Review report |
| 6.5 | (not done) | — | — |
| 7 | 30min | Package 9 files + README | Dev package |
| 8 | (not done) | — | — |

## Statistics

| Metric | Value |
|---|---:|
| Total steps | 745 |
| User interactions | 56 |
| Skills activated | 8 unique |
| Subagent invocations | 4 |
| Review rounds | 3 (85→92→95) |
| Findings fixed | 14/14 (100%) |

## Skills Usage

| Skill | Times | Phases | Role |
|---|:---:|---|---|
| `/docs-architect` | 5 | P1,P3,P5,P6 | Writer |
| `/database-design` | 5 | P3,P4,P7 | Builder |
| `/architect-review` | 3 | P5,P6,P7 | Reviewer |
| `/backend-architect` | 2 | P2,P4 | Designer |
| `/c4-architecture` | 3 | P3,P5 | Modeler |
| `/database-architect` | 2 | P4 | Peer Reviewer |
| `/brainstorming` | 1 | P3 | Ideator |
| `/architecture-decision-records` | 1 | P5 | Formalizer |

## Lessons Learned

1. **Phase 0.5 missing** → Payment flow discovered late (Phase 6), caused ~100 steps rework
2. **Parallel subagents** → 60% faster Phase 1
3. **Self peer-review** (database-architect reviews database-design) → 6 issues caught early
4. **3 review rounds** → systematic quality improvement 85→92→95
5. **Q&A before design** → 5 rounds for payment flow prevented larger rework

## Recommendation
Always do Phase 0 + 0.5 thoroughly — it prevents expensive rework later.

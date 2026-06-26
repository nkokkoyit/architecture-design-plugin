---
name: phase-05-enterprise-context
description: "Analyze enterprise context: system landscape, business domain, dependencies, team topology, compliance requirements."
---

# Phase 0.5: Enterprise Context Analysis

## Role: Enterprise Context Analyst
## Effort: 30 min — 1h

## Purpose
Place the new application within the enterprise landscape. Understand "where it sits" before detailed design begins.

## ⚠️ CRITICAL RULE: MUST Proactively Ask for Enterprise Context

> **DO NOT skip the enterprise context inquiry step.**
> If the user has not provided landscape info, MUST ask before generating output.
> DO NOT infer/guess enterprise context from user input — MUST confirm with the user.

## Step 0.5.0 — Check Knowledge Base (NEW)

```
IF EA provides company knowledge from `knowledge-base/`:
  1. Read the provided company landscape, tech radar, team topology
  2. Confirm with user: "I have loaded your company knowledge base. Is it still current?"
  3. IF user confirms → skip full Q&A, only ask about PROJECT-SPECIFIC gaps:
     - Which team will own this new service?
     - Any new compliance requirements not in the KB?
     - Any new external integrations not in the landscape?
  4. Proceed to Step 0.5.2 (Map Dependencies) with KB + gap answers

IF NO knowledge base available:
  → Fall back to Step 0.5.1 (full Q&A below)
```

## Step 0.5.1 — Gather Enterprise Context (FULL Q&A — only if no KB)

Ask the user the following questions (if not already provided in the input):

```
To place the new system within the enterprise landscape, I need to know:

1. 🏢 System Landscape:
   - How many services/applications does the company currently have?
   - List the main services related to this project?
   - Is there a shared API Gateway / Service Mesh?

2. 🔧 Technology Stack:
   - What is the company's standard tech stack (language, framework, DB, cloud)?
   - Is there a technology radar? Which stacks are approved / deprecated?

3. 👥 Team Topology:
   - Which team will own the new service?
   - What are the related teams (upstream/downstream)?
   - Communication pattern? (collaboration / X-as-a-service)

4. 🔒 Compliance & Security:
   - Are there special compliance requirements? (PCI-DSS, HIPAA, Insurance regs)
   - Data classification policy?
   - Encryption requirements?

5. 📊 Monitoring & Operations:
   - What is the current monitoring platform? (Prometheus, DataDog, AppInsights...)
   - Logging platform? (ELK, CloudWatch...)
   - On-call process?
```

> **If the user has no landscape document:** Ask the user to describe it verbally. At minimum, you must know: how many services exist, the main stack, and who owns what.
>
> **If the user already provided context in the BRD:** Confirm it and ask about gaps (commonly missing: monitoring, team topology, compliance).

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
   - Which service owns each entity?
   - Where is the source-of-truth?

4. **Compliance mapping:**
   - PII fields → encryption requirement
   - Retention period per data category

## Step 0.5.3 — Impact Analysis

| Dimension | Question | MUST answer |
|---|---|:---:|
| Data | Does the new service create new data entities or use existing ones? | ✅ |
| API | Does it need to expose APIs for other services? | ✅ |
| Event | What events does it publish? What events does it subscribe to? | ✅ |
| Performance | How much additional load will it place on downstream services? | ✅ |
| Security | New PII? Access control changes? | ✅ |

## Step 0.5.4 — Generate Output

MUST use template: `templates/input/enterprise_context.tmpl.md`

Fill ALL sections:
1. System Landscape (Mermaid diagram MANDATORY)
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
| User (MANDATORY inquiry) | Enterprise context info |

## Output

| Artifact | Template | Description |
|---|---|---|
| `enterprise_context.md` | `templates/input/enterprise_context.tmpl.md` | Landscape diagram, dependency matrix, domain mapping, compliance |

## Sub-skills Used
- `docs-architect` (analyze existing documentation)

## Handoff to EA
Return `enterprise_context.md`. EA validates against own knowledge before approving.
**WAIT** for EA approval before Phase 1. Do NOT proceed automatically.

# Knowledge Base Configuration

## Purpose
This file tells the architecture-design plugin WHERE to find the company knowledge base.
The agent reads this file at startup and loads all referenced knowledge.

## Knowledge Base Paths

Configure one or more directories containing your company/project knowledge.
Paths can be absolute or relative to this file.

```yaml
# Primary: Company-wide knowledge (shared across all projects)
company_kb: "D:/Job/Source/company-knowledge"

# Secondary: Project-specific knowledge (optional, overrides company-level)
project_kb: ".agents/knowledge"
```

## Directory Structure

The knowledge base directory should follow this structure:

```
{kb_path}/
├── company-landscape.md       ← System landscape, service catalog (REQUIRED)
├── technology-radar.md        ← Approved/deprecated tech stacks (RECOMMENDED)
├── team-topology.md           ← Team ownership, interaction modes (RECOMMENDED)
├── compliance-security.md     ← Data classification, standards (RECOMMENDED)
├── architecture-constraints.md← Company-level architecture rules (RECOMMENDED)
├── architecture-adrs/         ← Company-level ADRs (OPTIONAL)
│   ├── ADR-001-kafka.md
│   ├── ADR-002-postgresql.md
│   └── ...
├── api-standards.md           ← API naming, versioning, error format (OPTIONAL)
├── infrastructure.md          ← Gateway, messaging, DB cluster details (OPTIONAL)
└── past-designs/              ← Previous architecture documents for reference (OPTIONAL)
    ├── order-service/
    ├── payment-service/
    └── ...
```

## How the Agent Uses This

### At Phase 0.5 (Enterprise Context):
1. Agent reads `kb_config.md` to find knowledge base path
2. Agent reads `company-landscape.md` (REQUIRED)
3. Agent reads all other available files
4. Agent SKIPS Q&A for sections already covered by knowledge base
5. Agent ONLY ASKS about gaps not covered

### At Phase 6 (Review):
1. Agent reads `architecture-constraints.md` for EA alignment checks
2. Agent reads `technology-radar.md` to verify tech stack compliance
3. Agent reads past designs in `past-designs/` for consistency checks

### Context Minimization:
- Phase 0: NO knowledge base loaded (only raw user input)
- Phase 0.5: FULL knowledge base loaded
- Phase 1-4: Only relevant sections passed to subagents
- Phase 6: constraints + radar loaded for review
- Phase 8: constraints loaded for governance rules

## Quick Start

1. Copy `company-landscape.example.md` to your knowledge base directory
2. Edit it with your company's actual data
3. Update the `company_kb` path in this file
4. The agent will auto-detect and load on next run

## File Size Guidelines

| File | Target Size | Notes |
|---|---|---|
| company-landscape.md | 5-15KB | Core reference, keep concise |
| technology-radar.md | 2-5KB | Update quarterly |
| team-topology.md | 2-5KB | Update when teams change |
| compliance-security.md | 3-8KB | Update when policies change |
| architecture-constraints.md | 1-3KB | Keep minimal, only hard rules |
| Past designs | 30-60KB each | Reference only, not loaded fully |

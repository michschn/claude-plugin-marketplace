# Documentation Expert Plugin for Claude Code

A Claude Code plugin that maintains Architecture Decision Records (ADRs), ensures code-documentation consistency, and proactively prompts for documentation when significant changes occur.

## Features

- **📝 ADR Management**: `/adr` command to create well-structured Architecture Decision Records
- **🔍 Consistency Review**: `/docs-review` verifies code aligns with documented decisions
- **📊 Documentation Audit**: `/docs-audit` comprehensive health check of all docs
- **🏗️ Structure Setup**: `/docs-init` initializes the docs directory structure
- **🤖 Proactive Prompting**: Skill auto-detects ADR-worthy changes and suggests documentation
- **👤 Expert Agent**: `@docs-expert` for complex documentation questions

## Installation

### From GitHub

```bash
# In Claude Code
/plugin marketplace add michschn/claude-plugins-marketplace
/plugin install docs-expert@michschn
```

### Local Development

```bash
git clone https://github.com/michschn/claude-plugins-marketplace.git

# In Claude Code
/plugin marketplace add ./claude-plugins-marketplace
/plugin install docs-expert@claude-plugins-marketplace
```

### Team Setup

Add to `.claude/settings.json`:

```json
{
  "marketplaces": ["michschn/claude-plugins-marketplace"],
  "plugins": {
    "docs-expert@michschn": "enabled"
  }
}
```

## Usage

### Initialize Documentation Structure

```
/docs-init
```

Creates:
```
docs/
├── adr/
│   ├── template.md
│   └── 0001-establish-documentation-structure.md
├── requirements/
├── design/
├── ideas/
│   └── backlog.md
└── README.md
```

### Create an ADR

```
/adr We decided to use Bazel for the build system because it handles our monorepo structure better than Make
```

Or interactively:
```
/adr
```

Claude will guide you through capturing context, decision, alternatives, and consequences.

### Review Code Against Documentation

```
/docs-review src/auth/
```

Checks:
- Does code follow decisions in ADRs?
- Are there pattern inconsistencies?
- Are there undocumented architectural decisions?

### Audit Documentation Health

```
/docs-audit
```

Reports:
- ADR inventory by status
- Stale or orphaned ADRs
- Documentation gaps
- Coverage recommendations

### Ask the Expert

```
@docs-expert Should this API change warrant an ADR?
```

## Auto-Detection

The plugin includes a **Skill** that automatically activates when Claude detects:

- New dependencies being added
- API design changes
- Data model modifications
- Pattern changes
- Technology choices

When detected, Claude will proactively suggest:

> "This change introduces [X]. Should I draft an ADR to capture the reasoning?"

## Documentation Structure

The plugin works with this documentation structure:

| Directory | Purpose |
|-----------|---------|
| `docs/adr/` | Architecture Decision Records |
| `docs/requirements/` | Product requirements & PRDs |
| `docs/design/` | Technical design documents |
| `docs/ideas/` | Exploration & future work |

## ADR Format

ADRs follow this structure:

```markdown
# NNNN: Title

## Status
Proposed | Accepted | Deprecated | Superseded

## Context
Why are we making this decision?

## Decision
What did we decide?

## Alternatives Considered
What else did we consider and why rejected?

## Consequences
What are the positive and negative implications?

## References
Related ADRs, code files, external docs
```

## When to Write an ADR

**DO write an ADR for:**
- Technology choices (frameworks, libraries, tools)
- Architectural patterns (API design, data models)
- Significant tradeoffs
- Hard-to-reverse decisions
- Changes affecting multiple system parts

**DON'T write an ADR for:**
- Bug fixes
- Small implementation details
- Style/formatting
- Easily reversible changes

## Commands Reference

| Command | Purpose |
|---------|---------|
| `/adr [topic]` | Create a new ADR |
| `/docs-review [files]` | Review code against docs |
| `/docs-audit` | Full documentation audit |
| `/docs-init` | Initialize docs structure |

## Templates

The plugin includes templates in `.skills/documentation/templates/`:

- `adr-template.md` - ADR format
- `requirement-template.md` - Requirements document format

## Contributing

Contributions welcome! Areas that would be helpful:

- Additional document templates
- More sophisticated consistency checking
- Integration with other documentation tools
- Support for different ADR formats (MADR, etc.)

## License

MIT

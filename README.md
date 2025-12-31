# Mike's Claude Code Plugins

A collection of Claude Code plugins for embedded development and documentation management.

## Plugins

| Plugin | Description |
|--------|-------------|
| [pigweed-expert](plugins/pigweed-expert/) | Ensures idiomatic Pigweed usage in embedded projects |
| [docs-expert](plugins/docs-expert/) | Documentation guardian for ADRs and code consistency |

## Installation

### Add the Marketplace

```bash
# In Claude Code
/plugin marketplace add michschn/claude-plugins-marketplace
```

### Install Plugins

```bash
# Install individually
/plugin install pigweed-expert@michschn
/plugin install docs-expert@michschn

# Or browse and install interactively
/plugin
```

## Plugin Details

### pigweed-expert

Ensures idiomatic [Pigweed](https://pigweed.dev) usage in your embedded projects.

**Commands:**
- `/pw-plan [feature]` - Plan implementation with proper Pigweed modules
- `/pw-review [files]` - Review code for idiomatic Pigweed usage
- `/pw-explain [module]` - Learn about a Pigweed module
- `/pw-migrate [files]` - Migrate from std:: to Pigweed patterns
- `/pw-update-docs` - Update local Pigweed documentation cache

**Agent:** `@pigweed-expert` - Ask complex Pigweed questions

**Auto-activation:** Skill triggers when working with `pw_*` modules

[Full documentation →](plugins/pigweed-expert/README.md)

### docs-expert

Maintains Architecture Decision Records (ADRs) and ensures code-documentation consistency.

**Commands:**
- `/adr [topic]` - Create a new Architecture Decision Record
- `/docs-review [files]` - Review code against documented decisions
- `/docs-audit` - Full documentation health audit
- `/docs-init` - Initialize docs/ structure

**Agent:** `@docs-expert` - Ask documentation questions

**Auto-activation:** Skill prompts for ADRs on significant changes

[Full documentation →](plugins/docs-expert/README.md)

## Team Setup

Add to your project's `.claude/settings.json`:

```json
{
  "marketplaces": ["michschn/claude-plugins-marketplace"],
  "plugins": {
    "pigweed-expert@michschn": "enabled",
    "docs-expert@michschn": "enabled"
  }
}
```

## Structure

```
claude-plugins-marketplace/
├── .claude-plugin/
│   └── marketplace.json      # Marketplace manifest
├── plugins/
│   ├── pigweed-expert/       # Pigweed plugin
│   │   ├── .claude-plugin/
│   │   ├── agents/
│   │   ├── commands/
│   │   └── skills/
│   └── docs-expert/          # Documentation plugin
│       ├── .claude-plugin/
│       ├── agents/
│       ├── commands/
│       └── skills/
└── README.md
```

## License

MIT

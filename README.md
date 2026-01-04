# Mike's Claude Code Plugins

A collection of Claude Code plugins for embedded development, NFC hardware, and documentation management.

## Plugins

| Plugin | Description |
|--------|-------------|
| [pigweed-expert](plugins/pigweed-expert/) | Ensures idiomatic Pigweed usage in embedded projects |
| [docs-expert](plugins/docs-expert/) | Documentation guardian for ADRs and code consistency |
| [pn532-expert](plugins/pn532-expert/) | Expert for PN532 NFC controller interfacing |
| [ntag424-expert](plugins/ntag424-expert/) | Expert for NTAG 424 DNA secure NFC tags |

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
/plugin install pn532-expert@michschn
/plugin install ntag424-expert@michschn

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

### pn532-expert

Expert assistant for [PN532](https://www.nxp.com/products/rfid-nfc/nfc-hf/nfc-readers/nfc-integrated-solution:PN5321A3HN) NFC controller development.

**Commands:**
- `/pn532-plan [task]` - Plan NFC feature implementation
- `/pn532-review [files]` - Review driver code for correctness
- `/pn532-explain [topic]` - Learn about commands, protocols, concepts
- `/pn532-init-docs [pdf]` - Initialize knowledge from user manual

**Agent:** `@pn532-expert` - Ask complex PN532 questions

**Auto-activation:** Skill triggers on NFC driver code, frame construction

**Documentation:** Point to your local PN532 User Manual PDF via path or config file, then run `/pn532-init-docs`

[Full documentation →](plugins/pn532-expert/README.md)

### ntag424-expert

Expert assistant for [NTAG 424 DNA](https://www.nxp.com/products/rfid-nfc/nfc-hf/ntag-for-tags-labels/ntag-424-dna-424-dna-tagtamper-advanced-security-and-privacy-for-trusted-iot-applications:NTAG424DNA) secure NFC tag development.

**Commands:**
- `/ntag424-plan [task]` - Plan secure NFC feature implementation
- `/ntag424-review [files]` - Security-focused code review
- `/ntag424-explain [topic]` - Learn about auth, SUN/SDM, security
- `/ntag424-init-docs [pdf]` - Initialize knowledge from datasheet

**Agent:** `@ntag424-expert` - Ask complex NTAG 424 questions

**Auto-activation:** Skill triggers on authentication, SUN/SDM code

**Documentation:** Point to your local NTAG 424 DNA datasheet and AN12196 via path or config file, then run `/ntag424-init-docs`

[Full documentation →](plugins/ntag424-expert/README.md)

## Team Setup

Add to your project's `.claude/settings.json`:

```json
{
  "marketplaces": ["michschn/claude-plugins-marketplace"],
  "plugins": {
    "pigweed-expert@michschn": "enabled",
    "docs-expert@michschn": "enabled",
    "pn532-expert@michschn": "enabled",
    "ntag424-expert@michschn": "enabled"
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
│   │   └── .skills/          # Generated (gitignored)
│   ├── docs-expert/          # Documentation plugin
│   │   ├── .claude-plugin/
│   │   ├── agents/
│   │   ├── commands/
│   │   └── .skills/          # Generated (gitignored)
│   ├── pn532-expert/         # PN532 NFC controller plugin
│   │   ├── .claude-plugin/
│   │   ├── agents/
│   │   ├── commands/
│   │   └── .skills/          # Generated (gitignored)
│   └── ntag424-expert/       # NTAG 424 DNA plugin
│       ├── .claude-plugin/
│       ├── agents/
│       ├── commands/
│       └── .skills/          # Generated (gitignored)
└── README.md
```

Note: `.skills/` directories contain generated knowledge and are gitignored. Run the appropriate init command (e.g., `/pn532-init-docs`) after cloning to populate them.

## License

MIT

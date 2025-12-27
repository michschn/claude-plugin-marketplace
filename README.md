# Claude Plugin Marketplace

A collection of Claude Code plugins for various development workflows.

## Available Plugins

### Pigweed Expert

Ensures idiomatic [Pigweed](https://pigweed.dev) usage in your embedded projects. Provides deep analysis during planning and code review, with automatic guidance when working with Pigweed code.

**Features:**
- **🎯 Smart Auto-Activation**: Skill automatically engages when working with `pw_*` modules
- **📋 Architecture Planning**: `/pw-plan` for designing features with proper module selection
- **🔍 Code Review**: `/pw-review` for thorough idiomatic analysis
- **📚 Module Explanations**: `/pw-explain` for learning any Pigweed module
- **🔄 Migration Assistance**: `/pw-migrate` for moving from std:: or manual implementations
- **📖 Documentation Cache**: `/pw-update-docs` keeps knowledge current from pigweed.dev
- **🤖 Expert Agent**: `@pigweed-expert` for complex questions requiring deep thinking

## Installation

### From GitHub

```bash
# In Claude Code, add the marketplace
/plugin marketplace add michschn/claude-plugin-marketplace

# Install a specific plugin
/plugin install pigweed-expert@michschn
```

### Local Development

```bash
# Clone the repo
git clone https://github.com/michschn/claude-plugin-marketplace.git

# In Claude Code, add as local marketplace
/plugin marketplace add ./claude-plugin-marketplace
/plugin install pigweed-expert@claude-plugin-marketplace
```

### Team Setup

Add to your project's `.claude/settings.json`:

```json
{
  "marketplaces": ["michschn/claude-plugin-marketplace"],
  "plugins": {
    "pigweed-expert@michschn": "enabled"
  }
}
```

---

## Pigweed Expert - Detailed Usage

### Planning New Features

```
/pw-plan implement a sensor polling system that reads from I2C every 100ms, handles failures gracefully, and logs anomalies
```

Claude will analyze requirements and recommend:
- Appropriate Pigweed modules (pw_async2, pw_result, pw_log, etc.)
- Architecture following Pigweed patterns
- BUILD.bazel structure
- Testing strategy

### Reviewing Code

```
/pw-review src/drivers/sensor.cc
```

or

```
/pw-review
```
(reviews current file or staged changes)

Claude will identify:
- Manual implementations that have Pigweed alternatives
- Incorrect Pigweed usage
- Style and architecture issues
- Specific before/after transformations

### Learning Modules

```
/pw-explain pw_async2
```

Get a comprehensive explanation with:
- Core concepts and design decisions
- Basic and advanced usage examples
- Integration patterns
- Common mistakes to avoid

### Migrating Code

```
/pw-migrate src/legacy/buffer_utils.cc
```

Claude will:
- Identify migration targets (std:: → pw::, manual → Pigweed)
- Provide step-by-step transformation guidance
- Show required BUILD.bazel changes
- Suggest testing approach

### Updating Documentation Cache

```
/pw-update-docs
```

Fetches latest documentation from pigweed.dev and generates a comprehensive summary for offline reference.

### Ask the Expert

For complex questions or architecture decisions:

```
@pigweed-expert How should I structure async communication between my sensor drivers and the main application loop?
```

### Auto-Activation

The plugin includes a **Skill** that automatically activates when Claude detects:

- Files with `#include "pw_*"` imports
- Bazel targets with `@pigweed//` dependencies
- Code patterns that could use Pigweed alternatives

When active, Claude will proactively:
- Flag anti-patterns (raw char buffers, std:: containers in embedded, etc.)
- Suggest Pigweed alternatives
- Enforce Pigweed style guidelines

### Module Coverage

The plugin knows about these Pigweed modules:

**Core:** `pw_result`, `pw_status`, `pw_span`, `pw_string`, `pw_containers`, `pw_log`, `pw_tokenizer`, `pw_assert`

**Async & Concurrency:** `pw_async2`, `pw_sync`, `pw_thread`

**Communication:** `pw_rpc`, `pw_hdlc`, `pw_stream`, `pw_protobuf`

**Storage & Utilities:** `pw_kvs`, `pw_blob_store`, `pw_checksum`, `pw_chrono`, `pw_bytes`, `pw_unit_test`

---

## Adding New Plugins

This marketplace is designed to host multiple plugins. Each plugin should follow this structure:

```
skills/<plugin-name>/
  SKILL.md           # Auto-activation skill definition
  knowledge/         # Cached documentation and resources

agents/
  <agent-name>.md    # Agent definitions (@mentions)

commands/
  <command>.md       # Slash command definitions (/commands)
```

To add a new plugin:
1. Create the necessary skill/agent/command files
2. Add the plugin entry to `.claude-plugin/marketplace.json`
3. Update this README with the plugin description

## Contributing

Contributions welcome! You can:
- Improve existing plugins
- Add new plugins for other frameworks/tools
- Fix bugs or improve documentation

## License

MIT

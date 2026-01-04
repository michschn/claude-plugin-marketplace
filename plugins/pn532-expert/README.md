# PN532 Expert Plugin

Expert assistant for PN532 NFC controller development - planning, code review, and hardware interfacing.

## Features

### Agent: `@pn532-expert`
Deep knowledge of PN532 commands, protocols, and interfacing. Ask complex questions about:
- Frame construction and checksums
- Protocol implementations (ISO14443A/B, FeliCa, P2P)
- Communication interfaces (SPI, I2C, HSU)
- Timing and performance optimization

### Commands

| Command | Description |
|---------|-------------|
| `/pn532-plan [task]` | Plan implementation of NFC features |
| `/pn532-review [files]` | Review code for correctness and best practices |
| `/pn532-explain [topic]` | Detailed explanation of commands, protocols, concepts |
| `/pn532-init-docs [pdf]` | Initialize knowledge base from user manual PDF |

### Auto-Activation
The skill automatically activates when working with:
- PN532 driver code
- NFC frame construction
- MIFARE/ISO14443 protocols

## Installation

```bash
/plugin marketplace add michschn/claude-plugins-marketplace
/plugin install pn532-expert@michschn
```

## Adding Documentation

The plugin works best with the PN532 User Manual. Since these are NXP proprietary documents, they're not included - you point to your local copies.

### Option 1: Pass Path Directly
```bash
/pn532-init-docs ~/Documents/datasheets/PN532_User_Manual.pdf
```

### Option 2: Config File (recommended)
Create `~/.config/claude-plugins/nfc-docs.json`:
```json
{
  "pn532": {
    "documents": ["~/Documents/datasheets/PN532_User_Manual.pdf"]
  }
}
```
Then just run:
```bash
/pn532-init-docs
```

The command extracts key information into `.skills/pn532/knowledge/overview.md`. The `.skills/` folder is gitignored (treated as build output), so each developer runs the init command after cloning.

## Example Usage

```bash
# Plan a feature
/pn532-plan read MIFARE Classic card with authentication

# Review existing code
/pn532-review src/drivers/nfc.cc

# Learn about a command
/pn532-explain InDataExchange

# Ask the expert
@pn532-expert Why is my ISO14443-4 communication failing after RATS?
```

## Coverage

- **Interfaces**: HSU, I2C, SPI
- **Protocols**: ISO14443-3A, ISO14443-3B, ISO14443-4, FeliCa, NFCIP-1, Jewel
- **Commands**: All host commands (0x00-0x86) and target commands
- **Card Types**: MIFARE Classic/Ultralight/DESFire, NTAG, ISO-DEP, and more

## License

MIT

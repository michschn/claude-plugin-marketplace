# NTAG 424 DNA Expert Plugin

Expert assistant for NTAG 424 DNA secure NFC tag development - authentication, SUN/SDM messaging, and secure file operations.

## Features

### Agent: `@ntag424-expert`
Deep knowledge of NTAG 424 DNA security features. Ask complex questions about:
- AES-128 EV2 authentication flow
- SUN/SDM (Secure Unique NFC / Secure Dynamic Messaging)
- Session key derivation and CMAC calculation
- File access conditions and key management
- Backend verification implementation

### Commands

| Command | Description |
|---------|-------------|
| `/ntag424-plan [task]` | Plan implementation of secure NFC features |
| `/ntag424-review [files]` | Review code for security and correctness |
| `/ntag424-explain [topic]` | Detailed explanation of commands, auth, SDM |
| `/ntag424-init-docs [pdf]` | Initialize knowledge base from datasheet |

### Auto-Activation
The skill automatically activates when working with:
- NTAG 424 driver code
- SUN/SDM implementations
- NFC authentication flows
- NDEF with encrypted content

## Installation

```bash
/plugin marketplace add michschn/claude-plugins-marketplace
/plugin install ntag424-expert@michschn
```

## Adding Documentation

The plugin works best with the NTAG 424 DNA datasheet and application notes. Since these are NXP proprietary documents, they're not included - you point to your local copies.

### Option 1: Pass Path Directly
```bash
/ntag424-init-docs ~/Documents/datasheets/nxp/
```

### Option 2: Config File (recommended)
Create `~/.config/claude-plugins/nfc-docs.json`:
```json
{
  "ntag424": {
    "documents": [
      "~/Documents/datasheets/NTAG424DNA.pdf",
      "~/Documents/datasheets/AN12196.pdf"
    ]
  }
}
```
Then just run:
```bash
/ntag424-init-docs
```

### Recommended Documents
- **NTAG 424 DNA datasheet** - Core reference
- **AN12196** - Features, hints, SUN/SDM examples  
- **AN12321** - Key diversification (optional)

The command extracts key information into `.skills/ntag424/knowledge/overview.md`. The `.skills/` folder is gitignored (treated as build output), so each developer runs the init command after cloning.

## Example Usage

```bash
# Plan a feature
/ntag424-plan implement SUN authentication with Firebase verification

# Review existing code
/ntag424-review src/auth/ntag_auth.cc

# Learn about a concept
/ntag424-explain session key derivation

# Ask the expert
@ntag424-expert How do I configure SDM to mirror UID and counter in the NDEF URL?
```

## Coverage

- **Authentication**: AES-128 EV2, LRP, session keys, CMAC
- **SUN/SDM**: URL templates, PICC data encryption, backend verification
- **Files**: CC, NDEF, Proprietary - access conditions and configuration
- **Keys**: Key 0-4 management, diversification, change procedures
- **TagTamper**: Loop status verification (NTAG 424 DNA TagTamper variant)

## Security Focus

This plugin emphasizes secure practices:
- Key management and storage
- Replay attack prevention
- Constant-time CMAC comparison
- Production deployment checklists
- Common vulnerability detection

## License

MIT

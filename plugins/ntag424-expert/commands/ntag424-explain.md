# /ntag424-explain

Explain NTAG 424 DNA commands, authentication, SDM/SUN, or security concepts with detailed examples.

## Usage
```
/ntag424-explain [topic]
```

## Examples
```
/ntag424-explain AuthenticateEV2First
/ntag424-explain SUN message format
/ntag424-explain session key derivation
/ntag424-explain SDM configuration bits
/ntag424-explain access conditions
/ntag424-explain CMAC calculation
/ntag424-explain file settings
```

## Behavior

Use extended thinking to provide comprehensive explanations:

### For Commands
```markdown
## [Command Name] (INS 0xXX)

### Purpose
[What this command does and when to use it]

### APDU Structure
| Field | Value | Description |
|-------|-------|-------------|
| CLA   | 0x00/0x90 | Class byte |
| INS   | 0xXX  | Instruction |
| P1    | 0xXX  | Parameter 1 |
| P2    | 0xXX  | Parameter 2 |
| Lc    | XX    | Data length |
| Data  | ...   | Command data |
| Le    | XX    | Expected response length |

### Parameters
- **[Param]**: [description and valid values]

### Response
| Field | Length | Description |
|-------|--------|-------------|
| ...   | ...    | ...         |
| SW1-SW2 | 2    | Status word |

### Status Codes
| SW | Meaning |
|----|---------|
| 9100 | Success |
| 91AE | Authentication error |
| ...  | ... |

### Example Transaction
[Complete APDU exchange with explanation]

### Security Notes
- [Authentication requirements]
- [Secure messaging considerations]
```

### For SUN/SDM
- URL format with all placeholders
- Configuration bit breakdown
- Backend verification algorithm
- Example URLs with real (anonymized) data

### For Cryptographic Operations
- Step-by-step calculation
- Key derivation formulas
- IV management
- Example with test vectors

### For Concepts
- Background and purpose
- How it works in NTAG 424 specifically
- Security implications
- Common pitfalls

## Knowledge Reference

Always check `.skills/ntag424/knowledge/overview.md` first for accurate details. Cite page numbers from the datasheet when available.

If documentation hasn't been initialized and specifics are needed, suggest running `/ntag424-init-docs` first.

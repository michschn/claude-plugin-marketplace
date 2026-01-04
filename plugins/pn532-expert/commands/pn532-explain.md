# /pn532-explain

Explain PN532 commands, registers, protocols, or concepts with detailed examples.

## Usage
```
/pn532-explain [topic]
```

## Examples
```
/pn532-explain InListPassiveTarget
/pn532-explain frame format
/pn532-explain MIFARE authentication
/pn532-explain I2C communication
/pn532-explain SAMConfiguration modes
/pn532-explain error codes
```

## Behavior

Use extended thinking to provide comprehensive explanations:

### For Commands
```markdown
## [Command Name] (0xXX)

### Purpose
[What this command does]

### Request Frame
| Byte | Value | Description |
|------|-------|-------------|
| TFI  | 0xD4  | Host to PN532 |
| CMD  | 0xXX  | Command code |
| ...  | ...   | Parameters |

### Parameters
- **Param1** (1 byte): [description and valid values]
- **Param2** (n bytes): [description]

### Response Frame
| Byte | Value | Description |
|------|-------|-------------|
| TFI  | 0xD5  | PN532 to Host |
| CMD+1| 0xXX  | Response code |
| ...  | ...   | Return data |

### Example
[Complete byte sequence with explanation]

### Common Errors
- Error 0x01: [meaning and cause]

### Usage Notes
- [Timing considerations]
- [Typical use cases]
- [Common mistakes]
```

### For Protocols
- State machine diagrams (ASCII)
- Timing requirements
- Required command sequences
- Example transactions

### For Concepts
- Background theory
- How it relates to PN532 specifically
- Practical implications
- Code examples

## Knowledge Reference

Always check `.skills/pn532/knowledge/overview.md` first for accurate details. Cite page numbers from the user manual when available.

If documentation hasn't been initialized and specifics are needed, suggest running `/pn532-init-docs` first.

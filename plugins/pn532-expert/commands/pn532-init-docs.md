# /pn532-init-docs

Initialize or update the PN532 knowledge base from PDF documentation.

## Usage
```
/pn532-init-docs [path-to-pdf-or-directory]
```

## Examples
```
/pn532-init-docs ~/Documents/datasheets/PN532_User_Manual.pdf
/pn532-init-docs ~/Documents/datasheets/nxp/
/pn532-init-docs  (uses config or looks in knowledge/ directory)
```

## Document Location Options

### Option 1: Pass Path Directly
```
/pn532-init-docs /path/to/your/PN532_User_Manual.pdf
```

### Option 2: Local Config File
Create `~/.config/claude-plugins/nfc-docs.json`:
```json
{
  "pn532": {
    "documents": [
      "~/Documents/datasheets/PN532_User_Manual.pdf",
      "~/Documents/datasheets/PN532_ApplicationNote.pdf"
    ]
  },
  "ntag424": {
    "documents": [
      "~/Documents/datasheets/NTAG424DNA.pdf",
      "~/Documents/datasheets/AN12196.pdf"
    ]
  }
}
```
Then just run:
```
/pn532-init-docs
```

### Option 3: Symlink (for team setups)
```bash
# Each developer links to their local docs
ln -s ~/Documents/datasheets/pn532 /path/to/plugin/.skills/pn532/knowledge/docs
```

## Behavior

Use extended thinking to process the documentation:

### 1. Locate Documentation
- Check command argument for path
- Check `~/.config/claude-plugins/nfc-docs.json` for configured paths
- Check `.skills/pn532/knowledge/` for existing PDFs
- Supported: PN532 User Manual, Application Notes

### 2. Extract and Summarize

Create `.skills/pn532/knowledge/overview.md` with:

```markdown
# PN532 Knowledge Base

Generated from: [source PDF(s)]
Generated on: [date]

## Quick Reference

### Communication Interfaces
[Summary of HSU/I2C/SPI configuration]

### Frame Format
[Byte-by-byte format with checksums]

### Command Reference

| Command | Code | Description | Page |
|---------|------|-------------|------|
| Diagnose | 0x00 | ... | p.XX |
| GetFirmwareVersion | 0x02 | ... | p.XX |
[... all commands ...]

### Register Map
[Key registers with addresses]

### Error Codes
| Code | Meaning |
|------|---------|
| 0x01 | Timeout |
[... all errors ...]

### Protocol Support

#### ISO14443A
- Supported card types
- Command sequences
- Authentication flow

#### ISO14443B
[...]

#### FeliCa
[...]

#### NFCIP-1 (P2P)
[...]

### Timing Specifications
[Critical timing values]

### Common Sequences

#### Card Detection and Read
[Step-by-step with frames]

#### MIFARE Classic Authentication
[Step-by-step with frames]

### Hardware Notes
- Antenna design considerations
- Power requirements
- Crystal/oscillator specs
```

### 3. Update Metadata

Create/update `.skills/pn532/knowledge/metadata.json`:
```json
{
  "sources": [
    {
      "filename": "PN532_User_Manual.pdf",
      "original_path": "/Users/mike/Documents/datasheets/PN532_User_Manual.pdf",
      "title": "PN532 User Manual",
      "version": "...",
      "pages": N
    }
  ],
  "generated": "ISO-date",
  "overview_file": "overview.md"
}
```

Note: Only the generated `overview.md` and `metadata.json` are stored in the plugin directory. The original PDFs remain in your local location and are NOT copied.

### 4. Verification
- Confirm key sections were extracted
- Note any missing information
- Suggest additional documents if needed

## Output

After completion:
```
✅ PN532 knowledge base initialized

Sources processed:
- ~/Documents/datasheets/PN532_User_Manual.pdf (XXX pages)

Generated:
- .skills/pn532/knowledge/overview.md
- .skills/pn532/knowledge/metadata.json

Original documents remain at their source locations (not copied to plugin).

The @pn532-expert agent and commands now have access to:
- XX commands documented
- XX registers mapped  
- XX error codes defined
- Protocol details for ISO14443A/B, FeliCa, P2P

Run /pn532-explain [topic] to query the knowledge base.
```

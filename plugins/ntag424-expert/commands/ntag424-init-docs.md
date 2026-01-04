# /ntag424-init-docs

Initialize or update the NTAG 424 DNA knowledge base from PDF documentation.

## Usage
```
/ntag424-init-docs [path-to-pdf-or-directory]
```

## Examples
```
/ntag424-init-docs ~/Documents/datasheets/NTAG424DNA.pdf
/ntag424-init-docs ~/Documents/datasheets/nxp/
/ntag424-init-docs  (uses config or looks in knowledge/ directory)
```

## Document Location Options

### Option 1: Pass Path Directly
```
/ntag424-init-docs /path/to/your/datasheets/
```
Will process all relevant PDFs in the directory.

### Option 2: Local Config File
Create `~/.config/claude-plugins/nfc-docs.json`:
```json
{
  "pn532": {
    "documents": [
      "~/Documents/datasheets/PN532_User_Manual.pdf"
    ]
  },
  "ntag424": {
    "documents": [
      "~/Documents/datasheets/NTAG424DNA.pdf",
      "~/Documents/datasheets/AN12196.pdf",
      "~/Documents/datasheets/AN12321.pdf"
    ]
  }
}
```
Then just run:
```
/ntag424-init-docs
```

### Option 3: Symlink (for team setups)
```bash
# Each developer links to their local docs
ln -s ~/Documents/datasheets/ntag424 /path/to/plugin/.skills/ntag424/knowledge/docs
```

## Recommended Documents

For complete coverage, include:
- **NTAG 424 DNA datasheet** - Core command reference
- **AN12196** - Features and hints, SUN/SDM examples
- **AN12321** - Key diversification (optional)

## Behavior

Use extended thinking to process the documentation:

### 1. Locate Documentation
- Check command argument for path
- Check `~/.config/claude-plugins/nfc-docs.json` for configured paths
- Check `.skills/ntag424/knowledge/` for existing PDFs

### 2. Extract and Summarize

Create `.skills/ntag424/knowledge/overview.md` with:

```markdown
# NTAG 424 DNA Knowledge Base

Generated from: [source PDF(s)]
Generated on: [date]

## Quick Reference

### Memory Organization
[File structure: CC, NDEF, Proprietary]
[Size limits per file]

### Key Allocation
| Key | Default Use | Access Rights |
|-----|-------------|---------------|
| 0   | Master key  | All operations |
| 1-4 | Application | Configurable |

### Command Reference

| Command | INS | Auth Required | Description | Page |
|---------|-----|---------------|-------------|------|
| ISOSelectFile | A4 | No | Select app/file | p.XX |
| AuthenticateEV2First | 71 | - | Start auth | p.XX |
[... all commands ...]

### Status Codes
| SW1-SW2 | Meaning |
|---------|---------|
| 9100 | Success |
| 91AE | Authentication error |
[... all status codes ...]

### SDM/SUN Configuration

#### SDMOptions Byte
| Bit | Name | Description |
|-----|------|-------------|
| 0 | UID | Mirror UID |
| 1 | SDMReadCtr | Mirror counter |
[...]

#### SDMAccessRights
[Key assignments for PICC data, file data, CMAC]

#### URL Template Format
[Placeholder positions and encoding]

### Authentication

#### EV2 Flow
[Step-by-step with byte formats]

#### Session Key Derivation
[SV1, SV2 formulas]

#### CMAC Calculation
[Algorithm and examples]

### File Access Conditions
[Access rights bit definitions]
[Read/Write/ReadWrite/Change permissions]

### Security Recommendations
- Key management best practices
- Production deployment checklist
```

### 3. Update Metadata

Create/update `.skills/ntag424/knowledge/metadata.json`:
```json
{
  "sources": [
    {
      "filename": "NTAG424DNA.pdf",
      "original_path": "/Users/mike/Documents/datasheets/NTAG424DNA.pdf",
      "title": "NTAG 424 DNA Datasheet",
      "revision": "...",
      "pages": N
    },
    {
      "filename": "AN12196.pdf",
      "original_path": "/Users/mike/Documents/datasheets/AN12196.pdf",
      "title": "NTAG 424 DNA Features and Hints",
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
✅ NTAG 424 DNA knowledge base initialized

Sources processed:
- ~/Documents/datasheets/NTAG424DNA.pdf (XXX pages)
- ~/Documents/datasheets/AN12196.pdf (XXX pages)

Generated:
- .skills/ntag424/knowledge/overview.md
- .skills/ntag424/knowledge/metadata.json

Original documents remain at their source locations (not copied to plugin).

The @ntag424-expert agent and commands now have access to:
- XX commands documented
- XX status codes defined
- SDM/SUN configuration details
- Authentication flow specifications
- Security guidelines

Run /ntag424-explain [topic] to query the knowledge base.
```

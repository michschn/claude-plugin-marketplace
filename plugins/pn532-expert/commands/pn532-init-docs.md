# /pn532-init-docs

Initialize or update the PN532 knowledge base from PDF documentation.

## Usage
```
/pn532-init-docs [path-to-pdf]
```

## Examples
```
/pn532-init-docs ~/Documents/datasheets/PN532_User_Manual.pdf
```

## Behavior

**IMPORTANT**: The PDF is too large to read directly. Process it chapter-by-chapter as images to preserve diagrams.

### 1. Setup - Convert PDF to Page Images

```bash
# Install poppler-utils if needed (Ubuntu/Debian)
sudo apt-get install poppler-utils

# Get the plugin directory (adjust path as needed)
PLUGIN_DIR=~/.claude/plugins/installed/pn532-expert
# Or if using local marketplace:
# PLUGIN_DIR=/path/to/claude-plugins-marketplace/plugins/pn532-expert

# Create pages directory inside .skills
mkdir -p "$PLUGIN_DIR/.skills/pn532/pages"

# Convert PDF to PNG images (one per page, 150 DPI for good quality)
pdftoppm -png -r 150 "/path/to/PN532_User_Manual.pdf" "$PLUGIN_DIR/.skills/pn532/pages/page"

# Verify
ls "$PLUGIN_DIR/.skills/pn532/pages/"*.png | wc -l
```

The page images are now stored at:
```
.skills/pn532/pages/
├── page-01.png
├── page-02.png
├── page-03.png
...
```

### 2. Get Document Structure

View the table of contents (usually first few pages):

```
View .skills/pn532/pages/page-01.png through page-05.png to find TOC
```

Typical PN532 User Manual structure:
| Chapter | Typical Pages | Content | Priority |
|---------|---------------|---------|----------|
| 1-2 | 1-10 | Overview, Features | Low |
| 3-5 | 11-25 | Pins, Interfaces (HSU/I2C/SPI) | Medium |
| 6 | 26-40 | Host interface, Frame format | ⭐ High |
| 7-9 | 41-90 | Commands | ⭐⭐ Critical |
| 10+ | 91+ | Card emulation, App notes | Medium |

### 3. Process Chapter by Chapter

For each chapter, view the page images and extract:

- **Text**: Commands, parameters, register values, descriptions
- **Diagrams**: Frame formats, timing diagrams, state machines → describe and redraw as ASCII
- **Tables**: Command codes, error codes, register maps

**Recommended processing order:**
1. Frame format chapter - essential for all communication
2. Commands chapter - the core reference
3. Protocol sections - ISO14443, MIFARE specifics
4. Overview - for context

### 4. Build Knowledge Summary

Create/update `.skills/pn532/knowledge/overview.md`:

```markdown
# PN532 Knowledge Base

Source: PN532 User Manual Rev X.X
Pages: XX total, stored in .skills/pn532/pages/
Generated: [date]

## Document Map
| Chapter | Pages | Content |
|---------|-------|---------|
| 6 | 26-40 | Frame format |
| 7 | 41-70 | Commands |
[...]

## Frame Format (p.26-30)

### Normal Information Frame
┌──────────┬───────┬─────┬─────┬──────────┬─────┬──────────┐
│ PREAMBLE │ START │ LEN │ LCS │ TFI+DATA │ DCS │ POSTAMBLE│
│   0x00   │00 FF  │     │     │          │     │   0x00   │
└──────────┴───────┴─────┴─────┴──────────┴─────┴──────────┘

- LCS: Lower byte of (LEN + LCS) = 0x00
- DCS: Lower byte of (TFI + DATA[0..n] + DCS) = 0x00
- TFI: 0xD4 (host→PN532), 0xD5 (PN532→host)

## Command Reference

### InListPassiveTarget (0x4A) - p.45
**Purpose**: Detect and activate passive targets
**Request**: D4 4A [MaxTg] [BrTy] [InitiatorData...]
**Response**: D5 4B [NbTg] [Tg] [TargetData...]

Parameters:
- MaxTg: Max targets (1 or 2)
- BrTy: Baud rate/type (0x00=106kbps ISO14443A)

See page-45.png for timing diagram.

### InDataExchange (0x40) - p.52
[...]

## Error Codes (p.XX)
| Code | Name | Meaning |
|------|------|---------|
| 0x00 | Success | No error |
| 0x01 | Timeout | RF timeout |
| 0x02 | CRC Error | CRC mismatch |
[...]

## Key Diagrams

### ISO14443A Activation (p.XX)
REQA → ATQA → Anticollision → SELECT → SAK
[Describe flow from diagram]

### Frame Timing (p.XX)  
[Describe timing requirements]
```

### 5. Update Metadata

Create `.skills/pn532/knowledge/metadata.json`:
```json
{
  "source": {
    "filename": "PN532_User_Manual.pdf",
    "original_path": "/path/to/original.pdf",
    "revision": "X.X",
    "total_pages": 120
  },
  "pages_dir": "../pages",
  "generated": "2025-01-04",
  "chapters_processed": ["6", "7"],
  "overview_file": "overview.md"
}
```

## Incremental Processing

Process across multiple sessions:

```
"Continue /pn532-init-docs - process pages 41-55 (commands chapter)"
```

The agent can now reference specific pages:
```
"Check .skills/pn532/pages/page-45.png for the InListPassiveTarget timing diagram"
```

## Later Reference

When answering questions, the agent can:
1. Check overview.md for quick reference
2. View specific page images for details not in the summary
3. Cite page numbers for user to verify

## Output

```
✅ PN532 knowledge base updated

Processed: Pages 41-55 (Commands)

Added to overview.md:
- 8 commands documented
- 3 diagrams described  
- Error code table

Page images stored: .skills/pn532/pages/ (120 pages)
Progress: 3/8 chapters processed

Next suggested: Pages 56-70 (remaining commands)
```

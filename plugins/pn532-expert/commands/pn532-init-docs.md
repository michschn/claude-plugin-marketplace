# /pn532-init-docs

Initialize or update the PN532 knowledge base from datasheet page images.

## Usage
```
/pn532-init-docs
/pn532-init-docs [chapter-or-page-range]
```

## Examples
```
/pn532-init-docs
/pn532-init-docs pages 41-55
/pn532-init-docs commands chapter
```

## CRITICAL: Do NOT Read PDFs Directly

**STOP. You MUST NOT attempt to read any PDF file directly.** PDFs are too large and will fail. This command works with pre-converted page images only.

## Step 1: Check for Existing Page Images (ALWAYS DO THIS FIRST)

Before anything else, check if page images exist:

```
Check if directory exists: .skills/pn532/pages/
If exists, list PNG files to see what's available
```

### If page images DO exist:
Skip to **Step 3: Process Page Images**

### If page images do NOT exist:
Tell the user they need to create them first, then STOP. Do not attempt to read any PDF.

**Tell the user:**
```
Page images not found at .skills/pn532/pages/

To initialize the knowledge base, first convert the PDF to images:

1. Install poppler-utils if needed:
   sudo apt-get install poppler-utils

2. Create the pages directory:
   mkdir -p /path/to/plugin/.skills/pn532/pages

3. Convert the PDF to PNG images:
   pdftoppm -png -r 150 "/path/to/PN532_User_Manual.pdf" ".skills/pn532/pages/page"

4. Run this command again once images are created.

The plugin directory is typically:
- Installed: ~/.claude/plugins/installed/pn532-expert
- Local dev: /path/to/claude-plugins-marketplace/plugins/pn532-expert
```

**Then STOP. Do not continue until the user creates the images.**

## Step 2: Verify Page Images

Once the user confirms images exist, verify them:

```
ls .skills/pn532/pages/*.png | wc -l
```

Expected structure:
```
.skills/pn532/pages/
├── page-01.png
├── page-02.png
├── page-03.png
...
```

## Step 3: Process Page Images

### Get Document Structure

View the table of contents (first few pages) to understand the document:

```
Read .skills/pn532/pages/page-01.png through page-05.png
```

Typical PN532 User Manual structure:
| Chapter | Typical Pages | Content | Priority |
|---------|---------------|---------|----------|
| 1-2 | 1-10 | Overview, Features | Low |
| 3-5 | 11-25 | Pins, Interfaces (HSU/I2C/SPI) | Medium |
| 6 | 26-40 | Host interface, Frame format | High |
| 7-9 | 41-90 | Commands | Critical |
| 10+ | 91+ | Card emulation, App notes | Medium |

### Process Chapter by Chapter

For each chapter, view the page images and extract:

- **Text**: Commands, parameters, register values, descriptions
- **Diagrams**: Frame formats, timing diagrams, state machines → describe and redraw as ASCII
- **Tables**: Command codes, error codes, register maps

**Recommended processing order:**
1. Frame format chapter - essential for all communication
2. Commands chapter - the core reference
3. Protocol sections - ISO14443, MIFARE specifics
4. Overview - for context

## Step 4: Build Knowledge Summary

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

## Step 5: Update Metadata

Create `.skills/pn532/knowledge/metadata.json`:
```json
{
  "source": {
    "filename": "PN532_User_Manual.pdf",
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
/pn532-init-docs pages 41-55
```

The agent references specific page images:
```
View .skills/pn532/pages/page-45.png for the InListPassiveTarget timing diagram
```

## Output

```
✅ PN532 knowledge base updated

Processed: Pages 41-55 (Commands)

Added to overview.md:
- 8 commands documented
- 3 diagrams described
- Error code table

Page images: .skills/pn532/pages/ (120 pages)
Progress: 3/8 chapters processed

Next suggested: Pages 56-70 (remaining commands)
```

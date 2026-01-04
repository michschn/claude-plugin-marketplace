# /ntag424-init-docs

Initialize or update the NTAG 424 DNA knowledge base from datasheet page images.

## Usage
```
/ntag424-init-docs
/ntag424-init-docs [document] [chapter-or-page-range]
```

## Examples
```
/ntag424-init-docs
/ntag424-init-docs datasheet pages 31-50
/ntag424-init-docs an12196 sdm section
```

## CRITICAL: Do NOT Read PDFs Directly

**STOP. You MUST NOT attempt to read any PDF file directly.** PDFs are too large and will fail. This command works with pre-converted page images only.

## Recommended Documents

| Document | Content | Priority |
|----------|---------|----------|
| NTAG 424 DNA datasheet | Commands, memory, specs | Critical |
| AN12196 | SUN/SDM examples, hints | Essential |
| AN12321 | Key diversification | Medium |

## Step 1: Check for Existing Page Images (ALWAYS DO THIS FIRST)

Before anything else, check if page images exist:

```
Check if directories exist:
- .skills/ntag424/pages/datasheet/
- .skills/ntag424/pages/an12196/

If any exist, list PNG files to see what's available
```

### If page images DO exist:
Skip to **Step 3: Process Page Images**

### If page images do NOT exist:
Tell the user they need to create them first, then STOP. Do not attempt to read any PDF.

**Tell the user:**
```
Page images not found at .skills/ntag424/pages/

To initialize the knowledge base, first convert the PDFs to images:

1. Install poppler-utils if needed:
   sudo apt-get install poppler-utils

2. Create the pages directories:
   mkdir -p /path/to/plugin/.skills/ntag424/pages/datasheet
   mkdir -p /path/to/plugin/.skills/ntag424/pages/an12196

3. Convert the PDFs to PNG images:
   pdftoppm -png -r 150 "/path/to/NTAG424DNA.pdf" ".skills/ntag424/pages/datasheet/page"
   pdftoppm -png -r 150 "/path/to/AN12196.pdf" ".skills/ntag424/pages/an12196/page"

4. Run this command again once images are created.

The plugin directory is typically:
- Installed: ~/.claude/plugins/installed/ntag424-expert
- Local dev: /path/to/claude-plugins-marketplace/plugins/ntag424-expert
```

**Then STOP. Do not continue until the user creates the images.**

## Step 2: Verify Page Images

Once the user confirms images exist, verify them:

```
ls .skills/ntag424/pages/datasheet/*.png | wc -l
ls .skills/ntag424/pages/an12196/*.png | wc -l
```

Expected structure:
```
.skills/ntag424/pages/
├── datasheet/
│   ├── page-01.png
│   ├── page-02.png
│   ...
└── an12196/
    ├── page-01.png
    ├── page-02.png
    ...
```

## Step 3: Process Page Images

### Get Document Structure

View table of contents from each document.

**NTAG 424 DNA Datasheet structure:**
| Chapter | Typical Pages | Content | Priority |
|---------|---------------|---------|----------|
| 1-3 | 1-15 | Features, Ordering, Block diagram | Low |
| 4-5 | 16-30 | Functional description, Memory | Medium |
| 6 | 31-50 | **Command set** | Critical |
| 7 | 51-65 | **Security (Auth, SDM)** | Critical |
| 8+ | 66+ | Electrical specs, Package | Low |

**AN12196 structure:**
| Section | Typical Pages | Content | Priority |
|---------|---------------|---------|----------|
| 1-2 | 1-10 | Overview, Getting started | Medium |
| 3 | 11-25 | **SUN/SDM configuration** | Essential |
| 4 | 26-35 | **Authentication examples** | Critical |
| 5 | 36-45 | Backend verification code | Critical |

### Process Chapter by Chapter

For each chapter, view page images and extract:

- **APDUs**: Full command/response format with all bytes
- **Diagrams**: Auth flows, SDM URL structure, memory maps → describe and redraw
- **Tables**: Commands, status codes, configuration bits
- **Code examples**: From AN12196

**Recommended processing order:**
1. AN12196 SDM section - most complex, most useful
2. Datasheet command set - APDU reference
3. AN12196 auth examples - practical flows
4. Datasheet security chapter - theory behind auth

## Step 4: Build Knowledge Summary

Create/update `.skills/ntag424/knowledge/overview.md`:

```markdown
# NTAG 424 DNA Knowledge Base

Sources:
- NTAG 424 DNA Datasheet Rev 3.3 (85 pages) → .skills/ntag424/pages/datasheet/
- AN12196 Rev 1.3 (48 pages) → .skills/ntag424/pages/an12196/

Generated: [date]

## Document Map

### Datasheet
| Chapter | Pages | Content |
|---------|-------|---------|
| 6 | 31-50 | Command set |
| 7 | 51-65 | Security features |

### AN12196
| Section | Pages | Content |
|---------|-------|---------|
| 3 | 11-25 | SDM/SUN configuration |
| 4 | 26-35 | Authentication |

## Memory Organization (Datasheet p.18)

| File | ID | Size | Purpose |
|------|-----|------|---------|
| CC | 01 | 32 B | Capability Container |
| NDEF | 02 | 256 B | NDEF message |
| Proprietary | 03 | 128 B | Custom data |

## Command Reference

### ISOSelectFile (INS 0xA4) - Datasheet p.32
Select NTAG 424 DNA application.

**APDU**: `00 A4 04 00 07 D276000085010100 00`
**Success**: `9000`

### AuthenticateEV2First (INS 0x71) - Datasheet p.38
Start AES-128 mutual authentication.

**APDU**: `90 71 00 00 02 [KeyNo] [LenCap] 00`
**Response**: `[RndB_enc (16)] 91AF`

See datasheet/page-38.png for full flow diagram.

## Authentication Flow (Datasheet p.40, AN12196 p.28)

```
Host                              Tag
  │                                │
  │──AuthEV2First(KeyNo)─────────>│
  │<─────────[RndB_enc, 91AF]─────│
  │                                │
  │  Host: Dec(RndB), Gen(RndA)   │
  │  Host: Enc(RndA || RndB')     │
  │                                │
  │──[RndA||RndB' enc]───────────>│
  │<──[TI||RndA'||caps, 9100]─────│
  │                                │
  [Both derive session keys]
```

Session key derivation: See AN12196 p.30 for SV1/SV2 formulas.

## SUN/SDM Configuration (AN12196 p.12-20)

### URL Template
```
https://example.com/v?uid=00000000000000&ctr=000000&cmac=0000000000000000
                         └─ENC_PICC_DATA─┘    └CTR─┘     └────CMAC────┘
```

### SDMOptions Byte (AN12196 p.14)
| Bit | Name | Description |
|-----|------|-------------|
| 0 | ASCII | ASCII encoding (vs binary) |
| 1 | UID | Include UID in PICC data |
| 2 | SDMReadCtr | Include counter |
| 6 | SDMEncFileData | Encrypt file data |

See an12196/page-14.png for full bit definitions.

### SDMAccessRights (AN12196 p.15)
| Nibble | Key Assignment |
|--------|----------------|
| Bits 0-3 | SDMCtrRet key |
| Bits 4-7 | SDMMetaRead key |
| Bits 8-11 | SDMFileRead key |
| Bits 12-15 | Reserved |

## Status Codes (Datasheet p.48)
| SW | Meaning |
|----|---------|
| 9100 | Success |
| 91AF | Additional frame expected |
| 91AE | Authentication error |
| 917E | Length error |
| 919D | Permission denied |
| 91BE | Boundary error |

## Backend Verification (AN12196 p.36)

1. Parse URL, extract ENC_PICC_DATA, ENC_CTR, CMAC
2. Decrypt PICC data with SDMMetaReadKey
3. Extract UID (bytes 0-6) and SDMReadCtr (bytes 7-9, LSB first)
4. Verify counter > last seen counter (replay protection)
5. Reconstruct CMAC input = URL up to CMAC placeholder
6. Calculate CMAC with SDMFileReadKey
7. Compare (constant-time!) with received CMAC

See an12196/page-38.png for pseudocode.
```

## Step 5: Update Metadata

Create `.skills/ntag424/knowledge/metadata.json`:
```json
{
  "sources": [
    {
      "name": "datasheet",
      "filename": "NTAG424DNA.pdf",
      "revision": "3.3",
      "pages": 85,
      "pages_dir": "../pages/datasheet"
    },
    {
      "name": "an12196",
      "filename": "AN12196.pdf",
      "revision": "1.3",
      "pages": 48,
      "pages_dir": "../pages/an12196"
    }
  ],
  "generated": "2025-01-04",
  "chapters_processed": {
    "datasheet": ["6", "7"],
    "an12196": ["3", "4", "5"]
  },
  "overview_file": "overview.md"
}
```

## Incremental Processing

Process across multiple sessions:

```
/ntag424-init-docs an12196 pages 11-20
```

The agent references specific page images:
```
View .skills/ntag424/pages/an12196/page-14.png for SDMOptions bit definitions
```

## Output

```
✅ NTAG 424 DNA knowledge base updated

Processed: AN12196 pages 11-20 (SDM Configuration)

Added to overview.md:
- SDM URL template documented
- SDMOptions/SDMAccessRights bits explained
- 4 diagrams described

Page images stored:
- .skills/ntag424/pages/datasheet/ (85 pages)
- .skills/ntag424/pages/an12196/ (48 pages)

Progress:
- Datasheet: 2/5 chapters
- AN12196: 2/4 sections

Next suggested: AN12196 pages 26-35 (Authentication examples)
```

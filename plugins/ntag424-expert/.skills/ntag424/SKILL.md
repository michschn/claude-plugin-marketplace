# NTAG 424 DNA Secure NFC Tag Skill

This skill provides expert knowledge for interfacing with NTAG 424 DNA and NTAG 424 DNA TagTamper secure NFC tags.

## Activation Triggers

Activate this skill when the context includes:
- Code with NTAG 424/NTAG424 references
- AES authentication for NFC tags
- SUN/SDM (Secure Unique NFC / Secure Dynamic Messaging) implementation
- ISO 7816-4 APDUs in NFC context
- CMAC calculation for NFC authentication
- NDEF with encrypted UID or counter mirroring
- Firebase/backend NFC verification

## Keyword Detection
- `ntag424`, `NTAG424`, `NTAG 424`
- `SUN`, `SDM`, `SecureDynamicMessaging`
- `AuthenticateEV2`, `EV2First`, `EV2NonFirst`
- `SDMReadCtr`, `PICC`, `PICCData`
- `0x71` (AuthenticateEV2First INS) in NFC context
- `D276000085010100` (NTAG 424 DNA AID)
- `CMac`, `CMAC` combined with NFC/tag context

## Knowledge Base

If `knowledge/overview.md` exists, it contains:
- Complete command reference with APDUs
- SDM/SUN configuration details
- Authentication flow specification
- Status codes and error handling
- Security best practices

Reference this for accurate details. If it doesn't exist but PDFs are in `knowledge/`, suggest `/ntag424-init-docs`.

## Proactive Behaviors

### Authentication Review
When seeing authentication code, verify:
- Proper 3-pass EV2 flow
- Cryptographically secure RNG used
- Session key derivation correct
- Command counter management
- Response MAC verification

### SUN/SDM Checks
When seeing SUN implementation:
- URL template matches configured offsets
- CMAC calculation includes correct data
- Counter replay protection implemented
- Backend uses constant-time comparison

### Security Anti-Patterns

| Pattern | Risk | Suggestion |
|---------|------|------------|
| Hardcoded AES keys | Key compromise | Use secure storage, HSM, or diversification |
| Default keys (all zeros) | Total compromise | Change keys during personalization |
| No CMAC verification | Message forgery | Always verify response MAC |
| SDMReadCtr not tracked | Replay attacks | Store and validate counter server-side |
| UID in plaintext logs | Privacy violation | Hash or encrypt before logging |
| Same key for all tags | Single key compromise affects all | Use key diversification |

### Common Mistakes

1. **Wrong CMAC scope**: CMAC covers URL up to (but not including) CMAC placeholder
2. **Counter byte order**: SDMReadCtr is LSB-first in encrypted PICC data
3. **Missing padding**: AES operations need PKCS#7 or zero padding
4. **IV reuse**: Each encryption must use correct IV from session

### File Access Guidance

| Access Bits | Meaning |
|-------------|---------|
| 0xE | Free access (no auth) |
| 0xF | No access |
| 0-4 | Key number required |

Remind about:
- Read vs Write vs ReadWrite vs ChangeAccess rights
- Master key (Key 0) importance
- Principle of least privilege

## Response Style

When activated:
1. Acknowledge the NTAG 424 context
2. Emphasize security implications
3. Show complete APDUs with all fields
4. Reference documentation page numbers when available
5. Suggest `/ntag424-*` commands for deeper analysis
6. For SUN/SDM, include backend verification logic

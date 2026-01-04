# /ntag424-review

Review code that interfaces with NTAG 424 DNA for correctness and security best practices.

## Usage
```
/ntag424-review [files or directories]
```

## Examples
```
/ntag424-review src/nfc/ntag424_auth.cc
/ntag424-review src/auth/
/ntag424-review  (reviews staged changes)
```

## Behavior

Use extended thinking to thoroughly analyze the code:

### 1. APDU Construction
- [ ] Correct CLA byte (0x00 for native, 0x90 for wrapped)
- [ ] Proper INS codes for each command
- [ ] P1/P2 parameters correct
- [ ] Lc matches data length
- [ ] Le appropriate for expected response

### 2. Authentication
- [ ] Full 3-pass EV2 authentication implemented
- [ ] Proper random number generation (cryptographically secure)
- [ ] Correct RndB rotation (left shift by 1 byte)
- [ ] Session key derivation follows spec (SV1, SV2)
- [ ] Transaction Identifier (TI) handled
- [ ] Command counter incremented correctly

### 3. Secure Messaging
- [ ] CMAC calculated correctly (on command header + data)
- [ ] Encryption uses session key with correct IV
- [ ] MAC verification on responses
- [ ] Padding handled per AES-CBC requirements

### 4. SUN/SDM Implementation
- [ ] URL template matches SDM configuration
- [ ] CMAC verification handles ASCII encoding
- [ ] Counter replay protection implemented
- [ ] PICC data decryption uses correct key
- [ ] Offset calculations match file settings

### 5. Key Management
- [ ] Keys not hardcoded in source
- [ ] Default keys changed before deployment
- [ ] Key diversification properly implemented (if used)
- [ ] Master key protection (Key 0 access)

### 6. Error Handling
- [ ] All status words checked (not just 0x9000/0x9100)
- [ ] Authentication retry limits
- [ ] Counter overflow handled
- [ ] Communication errors don't leak state

### 7. Security Anti-Patterns

| Pattern | Risk | Fix |
|---------|------|-----|
| Hardcoded keys | Key compromise | Use secure storage/HSM |
| No counter check | Replay attacks | Track SDMReadCtr |
| Default keys in production | Complete compromise | Change during personalization |
| CMAC not verified | Man-in-middle | Always verify response MAC |
| Plaintext UID in logs | Privacy leak | Encrypt or hash |

## Output Format

```markdown
## NTAG 424 Code Review: [filename]

### 🔐 Security Assessment
[Overall security posture]

### ✅ Correct
- [Good security practices found]

### ⚠️ Security Warnings
- [Potential vulnerabilities]

### ❌ Critical Issues
- [Security bugs or protocol violations]
  - Line X: [problem]
  - Risk: [impact]
  - Fix: [solution]

### 📋 Recommendations
- [Security improvements]

### APDU Analysis
[If APDU construction found, verify format and security]

### Cryptographic Review
[Verify key derivation, CMAC, encryption implementations]
```

Reference `.skills/ntag424/knowledge/overview.md` for correct command formats and security requirements.

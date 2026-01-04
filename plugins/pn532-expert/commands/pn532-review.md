# /pn532-review

Review code that interfaces with the PN532 NFC controller for correctness and best practices.

## Usage
```
/pn532-review [files or directories]
```

## Examples
```
/pn532-review src/nfc/pn532_driver.cc
/pn532-review src/nfc/
/pn532-review  (reviews staged changes)
```

## Behavior

Use extended thinking to thoroughly analyze the code:

### 1. Frame Construction
- [ ] Correct preamble/postamble sequences
- [ ] Proper LEN and LCS (length checksum) calculation
- [ ] Correct DCS (data checksum) calculation
- [ ] TFI (frame identifier) correct for direction (0xD4 host→PN532, 0xD5 PN532→host)

### 2. Communication Protocol
- [ ] ACK/NACK handling after commands
- [ ] Proper timeout handling for responses
- [ ] Interface-specific requirements:
  - **I2C**: Ready/IRQ polling, repeated start handling
  - **SPI**: SS timing, clock phase/polarity
  - **HSU**: Proper baud rate, wake-up sequence if needed

### 3. Command Usage
- [ ] SAMConfiguration called after power-up/reset
- [ ] Correct command codes and parameters
- [ ] Response parsing matches expected format
- [ ] Error codes checked (PN532 error byte in response)

### 4. Protocol Compliance
- [ ] ISO14443 state machine followed correctly
- [ ] Proper RATS/PPS for ISO14443-4
- [ ] Authentication before protected operations (MIFARE)
- [ ] Anticollision handled for multiple cards

### 5. Timing & Reliability
- [ ] Minimum inter-command delays respected
- [ ] RF field timeout configuration appropriate
- [ ] Retry logic for transient failures
- [ ] Card removal detection

### 6. Resource Management
- [ ] RF field turned off when not needed (power saving)
- [ ] Proper cleanup on errors
- [ ] No memory leaks in frame buffers

## Output Format

```markdown
## PN532 Code Review: [filename]

### ✅ Correct
- [Good patterns found]

### ⚠️ Warnings
- [Potential issues or suboptimal patterns]

### ❌ Issues
- [Bugs or protocol violations]
  - Line X: [problem]
  - Fix: [solution]

### 📋 Recommendations
- [Suggested improvements]

### Frame Analysis
[If frame construction found, verify byte-by-byte]
```

Reference `.skills/pn532/knowledge/overview.md` for correct command formats and parameters.

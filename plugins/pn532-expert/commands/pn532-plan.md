# /pn532-plan

Plan PN532 NFC controller integration or feature implementation.

## Usage
```
/pn532-plan [feature or task description]
```

## Examples
```
/pn532-plan read MIFARE Classic card UID and block 4
/pn532-plan implement card detection with interrupt-driven polling
/pn532-plan NDEF read/write support
/pn532-plan peer-to-peer communication with phone
```

## Behavior

Use extended thinking to create a comprehensive implementation plan:

1. **Understand Requirements**
   - What NFC protocol is needed (ISO14443A, FeliCa, P2P, etc.)?
   - Which communication interface (SPI/I2C/HSU)?
   - What are the timing/performance requirements?

2. **Check Knowledge Base**
   - Reference `.skills/pn532/knowledge/overview.md` for command details
   - Look up specific register values and frame formats
   - Note any errata or common issues

3. **Design the Solution**
   - Initialization sequence (SAMConfiguration, RFConfiguration)
   - Command sequence for the feature
   - Frame construction with checksums
   - Response parsing and validation

4. **Create Implementation Plan**
   ```
   ## Overview
   [Brief description of what we're implementing]
   
   ## Prerequisites
   - Hardware connections
   - Required PN532 configuration
   
   ## Implementation Steps
   1. [Step with code outline]
   2. [Step with code outline]
   ...
   
   ## PN532 Commands Used
   | Command | Code | Purpose |
   |---------|------|---------|
   | ... | ... | ... |
   
   ## Frame Examples
   [Show actual byte sequences]
   
   ## Error Handling
   - [Specific error conditions to handle]
   
   ## Testing Strategy
   - [How to verify each step]
   ```

5. **Highlight Pitfalls**
   - Timing requirements (e.g., minimum delay between commands)
   - Protocol-specific gotchas (MIFARE auth before read, etc.)
   - Power/RF considerations

Always provide concrete byte sequences and frame formats, not just abstract descriptions.

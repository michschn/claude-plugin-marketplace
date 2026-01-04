# /ntag424-plan

Plan NTAG 424 DNA integration or feature implementation.

## Usage
```
/ntag424-plan [feature or task description]
```

## Examples
```
/ntag424-plan implement SUN authentication with Firebase backend
/ntag424-plan secure file read/write with AES authentication
/ntag424-plan NDEF URL with encrypted UID and CMAC
/ntag424-plan key diversification strategy for production
/ntag424-plan TagTamper status verification
```

## Behavior

Use extended thinking to create a comprehensive implementation plan:

1. **Understand Requirements**
   - Authentication vs. SUN/SDM (online vs. offline verification)?
   - Which keys are involved and their roles?
   - What data needs to be secured?
   - Backend integration requirements?

2. **Check Knowledge Base**
   - Reference `.skills/ntag424/knowledge/overview.md` for command details
   - Look up specific APDU formats and status codes
   - Note configuration bit fields and access conditions

3. **Design the Solution**
   - Key allocation and access rights matrix
   - File configuration (SDM settings if needed)
   - Command sequence with full APDUs
   - Backend verification logic (for SUN)

4. **Create Implementation Plan**
   ```
   ## Overview
   [Brief description of what we're implementing]
   
   ## Security Model
   - Key allocation (Key 0-4 roles)
   - Access conditions per file
   - Authentication requirements
   
   ## Prerequisites
   - Tag personalization steps
   - Backend setup (if SUN)
   
   ## Implementation Steps
   1. [Step with APDU examples]
   2. [Step with code outline]
   ...
   
   ## APDU Sequences
   | Step | Command | APDU | Expected Response |
   |------|---------|------|-------------------|
   | 1 | Select | 00 A4 04 00 07 D276000085010100 | 9000 |
   | ... | ... | ... | ... |
   
   ## SUN/SDM Configuration (if applicable)
   - SDM settings breakdown
   - URL template with placeholders
   - Backend verification pseudocode
   
   ## Key Management
   - Initial key values
   - Key change procedure
   - Diversification (if used)
   
   ## Error Handling
   - [Authentication failures]
   - [Counter overflow]
   - [Access denied scenarios]
   
   ## Testing Strategy
   - [Verification steps]
   ```

5. **Security Considerations**
   - Key storage on host device
   - Replay attack prevention (counters)
   - Secure channel for key changes
   - Production key management

Always provide concrete APDU sequences and show the cryptographic operations involved.

---
description: Review code for consistency with documented architecture decisions and requirements. Identifies deviations from ADRs and suggests documentation updates.
allowed-tools:
  - read_file
  - list_directory
  - grep
  - glob
---

# Documentation Consistency Review

Review code to ensure it aligns with documented architecture decisions and requirements.

## Instructions

### Phase 1: Load Documentation Context

1. Read all ADRs in `docs/adr/`:
   - List accepted decisions
   - Note patterns and conventions they establish
   - Identify affected code areas

2. Read requirements in `docs/requirements/`:
   - Current product requirements
   - Feature specifications

3. Check design docs in `docs/design/`:
   - System architecture
   - Technical designs

### Phase 2: Analyze the Code

For the files/changes specified by the user:

1. **Identify patterns used**:
   - What architectural patterns does the code follow?
   - What technologies/libraries are used?
   - What conventions are applied?

2. **Compare against ADRs**:
   - Does the code follow documented decisions?
   - Are there contradictions?
   - Are there undocumented patterns?

3. **Check requirements alignment**:
   - Does implementation match requirements?
   - Are there gaps or deviations?

### Phase 3: Identify Issues

Look for these specific problems:

#### Critical Issues (Must Address)

| Issue Type | Description |
|------------|-------------|
| **ADR Violation** | Code directly contradicts an accepted ADR |
| **Pattern Inconsistency** | Code uses different pattern than documented for similar problems |
| **Missing ADR** | Significant architectural decision with no ADR |
| **Stale ADR** | ADR references code/patterns that no longer exist |

#### Recommendations (Should Address)

| Issue Type | Description |
|------------|-------------|
| **Undocumented Pattern** | New pattern that should be captured in ADR |
| **Requirement Drift** | Code has evolved beyond documented requirements |
| **Missing Links** | ADR doesn't reference relevant code files |
| **Proposed ADR Stale** | ADR in "Proposed" status for too long |

### Phase 4: Generate Report

## Output Format

### Summary
Brief overview: X ADRs reviewed, Y issues found, Z recommendations

### Code-ADR Alignment

| ADR | Status | Code Alignment |
|-----|--------|----------------|
| 0001-... | Accepted | ✅ Aligned / ⚠️ Partial / ❌ Violated |

### Critical Issues

For each critical issue:

```
**Issue**: [Brief description]
**Type**: ADR Violation / Pattern Inconsistency / etc.
**ADR**: [NNNN-title] (if applicable)
**Location**: [File:line or component]
**Problem**: What's wrong and why it matters
**Resolution**: How to fix it
  - Option A: Update code to match ADR
  - Option B: Supersede ADR with new decision
```

### Recommendations

For each recommendation:

```
**Recommendation**: [Brief description]
**Type**: Undocumented Pattern / Missing ADR / etc.
**Location**: [Files/components involved]
**Suggestion**: What to do
```

### Documentation Gaps

Significant decisions observed in code but not documented:

1. [Decision/pattern]: Consider ADR for [reason]
2. ...

### ADR Health

- Accepted ADRs still valid: N
- Deprecated ADRs: N  
- Proposed ADRs pending decision: N (list if stale)

---

## Code to Review

$ARGUMENTS

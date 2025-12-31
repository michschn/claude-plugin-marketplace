---
description: Comprehensive audit of project documentation. Checks ADR health, documentation completeness, and identifies gaps that need attention.
allowed-tools:
  - read_file
  - list_directory
  - grep
  - glob
---

# Documentation Audit

Perform a comprehensive audit of project documentation health.

## Instructions

### Phase 1: Inventory Documentation

1. **Scan docs/ directory**:
   ```
   docs/
   ├── adr/           # Count and list ADRs
   ├── requirements/  # Count and list requirements
   ├── design/        # Count and list designs
   ├── ideas/         # Count and list ideas
   └── README.md      # Check index completeness
   ```

2. **Categorize ADRs by status**:
   - Proposed (pending decision)
   - Accepted (active)
   - Deprecated (no longer applicable)
   - Superseded (replaced by newer ADR)

### Phase 2: Check Documentation Quality

For each ADR, verify:

- [ ] Has all required sections (Status, Context, Decision, Consequences)
- [ ] Context explains the "why" adequately
- [ ] Decision is clear and actionable
- [ ] Consequences include both positives and negatives
- [ ] References valid code/files (if any mentioned)

For requirements docs:

- [ ] Current and up-to-date
- [ ] Testable/verifiable criteria
- [ ] Linked to relevant ADRs

### Phase 3: Cross-Reference with Code

1. **Find code references in ADRs**:
   - Extract file paths mentioned
   - Verify files still exist
   - Check if code matches what ADR describes

2. **Identify undocumented areas**:
   - Scan for major code components
   - Check if significant areas have ADRs

3. **Check for orphaned ADRs**:
   - ADRs referencing deleted code
   - ADRs for features that were never implemented

### Phase 4: Identify Issues

#### Critical (Requires Immediate Action)

- ADR violations in current code
- Contradicting ADRs (both accepted)
- Missing ADRs for core architecture

#### Warning (Should Address Soon)

- Stale "Proposed" ADRs (>30 days old)
- ADRs with broken code references
- Outdated requirements docs

#### Info (Good to Know)

- Documentation coverage statistics
- Recently updated docs
- Areas with dense vs. sparse documentation

### Phase 5: Generate Recommendations

Based on findings, suggest:

1. ADRs that need updating
2. New ADRs that should be created
3. Docs that should be deprecated
4. Index updates needed

## Output Format

### Documentation Overview

| Category | Count | Health |
|----------|-------|--------|
| ADRs (Accepted) | N | 🟢/🟡/🔴 |
| ADRs (Proposed) | N | 🟢/🟡/🔴 |
| ADRs (Deprecated) | N | - |
| Requirements | N | 🟢/🟡/🔴 |
| Design Docs | N | 🟢/🟡/🔴 |

### ADR Status Summary

**Accepted** (N)
- 0001-title: Brief description
- ...

**Proposed** (N) 
- 0005-title: Created DATE - ⚠️ Pending >30 days
- ...

**Deprecated/Superseded** (N)
- ...

### Issues Found

#### Critical
1. [Issue description with location and suggested fix]

#### Warnings  
1. [Issue description]

### Coverage Gaps

Areas of the codebase without ADR coverage:

| Component | Significance | Suggested ADR |
|-----------|-------------|---------------|
| src/auth/ | High | Authentication approach |
| ... | ... | ... |

### Recommendations

1. **Create ADR**: [Topic] - [Why needed]
2. **Update ADR**: 000N - [What needs updating]
3. **Deprecate ADR**: 000N - [Why no longer relevant]
4. **Update Index**: Add entries for ADRs 000X-000Y

### Documentation Metrics

- Total documentation files: N
- Last updated: DATE
- Coverage estimate: X% of major components
- Average ADR age: N days

---

$ARGUMENTS

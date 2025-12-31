---
description: Create a new Architecture Decision Record (ADR). Guides you through capturing context, decision, and consequences for significant architectural choices.
allowed-tools:
  - read_file
  - write_file
  - list_directory
  - grep
---

# Create Architecture Decision Record

Help create a well-structured ADR for an architectural decision.

## Instructions

### Phase 1: Gather Context

1. Check existing ADRs in `docs/adr/` to understand:
   - Current numbering (find highest NNNN)
   - Existing decisions that might be related
   - Project conventions and style

2. If the user hasn't provided details, ask:
   - What decision are we documenting?
   - What problem or need prompted this?
   - What alternatives were considered?
   - Why was this option chosen over others?

### Phase 2: Analyze the Decision

Use extended thinking to consider:

- **Scope**: Is this actually ADR-worthy, or just an implementation detail?
- **Completeness**: Do we have enough context to understand the "why"?
- **Consequences**: What are the real implications (positive AND negative)?
- **Alternatives**: Were other options genuinely considered?
- **Reversibility**: How hard is this to change later?

### Phase 3: Draft the ADR

Create the ADR file following this format:

```markdown
# NNNN: [Descriptive Title]

## Status

Proposed

## Context

[2-4 paragraphs explaining:]
- What situation/problem prompted this decision
- Relevant constraints or requirements
- What we need to achieve

## Decision

[1-2 paragraphs clearly stating:]
- What we decided to do
- Key aspects of the approach

## Alternatives Considered

### [Alternative 1]
- Description
- Why rejected: [reason]

### [Alternative 2]
- Description  
- Why rejected: [reason]

## Consequences

### Positive
- [Benefit 1]
- [Benefit 2]

### Negative
- [Drawback 1]
- [Drawback 2]

### Neutral
- [Implication that's neither good nor bad]

## References

- [Related ADRs, docs, or external resources]
- [Files/modules affected: `path/to/file.cc`]
```

### Phase 4: File Naming

Name the file: `docs/adr/NNNN-kebab-case-title.md`

- NNNN = next sequential number (0001, 0002, etc.)
- Use kebab-case for the title portion
- Keep the title concise but descriptive

### Phase 5: Update Index

If `docs/README.md` has an ADR index section, add an entry for the new ADR.

## Output

1. Create the ADR file in `docs/adr/`
2. Show the user the created content
3. Ask if they want to:
   - Refine any sections
   - Add more alternatives
   - Adjust the status (mark as Accepted if appropriate)

---

## Decision to Document

$ARGUMENTS

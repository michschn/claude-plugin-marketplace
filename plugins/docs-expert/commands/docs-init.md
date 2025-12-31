---
description: Initialize or verify the documentation structure in a project. Creates docs/ directory with ADR, requirements, design, and ideas subdirectories plus templates.
allowed-tools:
  - read_file
  - write_file
  - list_directory
---

# Initialize Documentation Structure

Set up or verify the project's documentation structure.

## Instructions

### Phase 1: Check Existing Structure

1. Check if `docs/` directory exists
2. Check for existing subdirectories:
   - `docs/adr/`
   - `docs/requirements/`
   - `docs/design/`
   - `docs/ideas/`
3. Check for `docs/README.md`
4. Check for `docs/adr/template.md`

### Phase 2: Create Missing Structure

Create any missing directories and files:

#### docs/README.md (if missing)

```markdown
# Project Documentation

This directory contains structured documentation for the project.

## Structure

| Directory | Purpose |
|-----------|---------|
| `adr/` | Architecture Decision Records - significant technical decisions |
| `requirements/` | Product requirements and specifications |
| `design/` | Technical design documents |
| `ideas/` | Exploration, future work, and backlog |

## Architecture Decision Records (ADRs)

ADRs capture important architectural decisions along with their context and consequences.

### When to write an ADR

- Technology choices (frameworks, libraries, tools)
- Architectural patterns (API design, data models)
- Significant tradeoffs
- Decisions that are hard to reverse

### ADR Statuses

- **Proposed**: Under discussion
- **Accepted**: Decision made and active
- **Deprecated**: No longer applicable  
- **Superseded**: Replaced by a newer ADR

### Current ADRs

| # | Title | Status |
|---|-------|--------|
| [0001](adr/0001-establish-documentation-structure.md) | Establish documentation structure | Accepted |

## Quick Links

- [ADR Template](adr/template.md)
- [Ideas Backlog](ideas/backlog.md)
```

#### docs/adr/template.md (if missing)

```markdown
# NNNN: [Title]

## Status

Proposed

## Context

[What is the issue that we're seeing that is motivating this decision?]

## Decision

[What is the change that we're proposing and/or doing?]

## Alternatives Considered

### [Alternative 1]
- Description
- Why rejected: [reason]

## Consequences

### Positive
- [Benefit]

### Negative  
- [Drawback]

## References

- [Related docs, code files, external resources]
```

#### docs/adr/0001-establish-documentation-structure.md (if first ADR)

```markdown
# 0001: Establish Documentation Structure

## Status

Accepted

## Context

As the project grows, important decisions and their rationale get lost in chat logs, code comments, or tribal knowledge. We need a systematic way to capture:

- Architectural decisions and why we made them
- Product requirements
- Technical designs
- Ideas for future exploration

## Decision

We will use a structured documentation system in the `docs/` directory:

- `docs/adr/` - Architecture Decision Records for significant technical decisions
- `docs/requirements/` - Product requirements and specifications  
- `docs/design/` - Technical design documents
- `docs/ideas/` - Exploration and future work

ADRs follow a standard format with Status, Context, Decision, and Consequences sections.

## Consequences

### Positive
- Decisions are discoverable and searchable
- New team members can understand "why" not just "what"
- Reduces repeated discussions about settled decisions
- Creates accountability for architectural choices

### Negative
- Requires discipline to write ADRs for significant changes
- Some overhead in maintaining documentation

### Neutral
- Establishes a convention that must be followed consistently
```

#### docs/ideas/backlog.md (if missing)

```markdown
# Ideas Backlog

Ideas and future work to explore.

## Format

```
### [Idea Title]
**Status**: New | Exploring | Parked | Done
**Added**: YYYY-MM-DD

[Brief description of the idea]

**Next steps**: [What would move this forward]
```

## Ideas

<!-- Add ideas below -->
```

### Phase 3: Verify CLAUDE.md Integration

Check if `CLAUDE.md` or `.claude/` config mentions the docs structure. If not, suggest adding:

```markdown
## Documentation

Project documentation lives in `docs/`. See [docs/README.md](docs/README.md) for structure.

- Create ADRs for significant architectural decisions
- Keep requirements up to date as features evolve
- Document ideas before they get lost
```

## Output

1. Report what was created vs. what already existed
2. Show the structure that's now in place
3. Suggest any next steps (like adding to CLAUDE.md)

---

$ARGUMENTS

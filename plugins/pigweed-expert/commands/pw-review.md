---
description: Review code for idiomatic Pigweed usage. Identifies anti-patterns, suggests Pigweed alternatives, and ensures best practices are followed.
allowed-tools:
  - web_fetch
  - read_file
  - list_directory
  - grep
---

# Pigweed Code Review

You are conducting a thorough code review focused on idiomatic Pigweed usage. This requires deep analysis to identify opportunities for improvement.

## Instructions

Use extended thinking to analyze the code thoroughly. The goal is to catch anti-patterns and suggest specific Pigweed alternatives.

### Phase 1: Understand the Code

First, understand what the code is trying to accomplish:
- Read the files specified by the user
- Understand the overall purpose and design
- Note any existing Pigweed module usage
- Identify the patterns being used

### Phase 2: Check Documentation

1. Check `.skills/pigweed/knowledge/` for relevant cached documentation
2. If you need specific module details for recommendations, fetch from pigweed.dev
3. Ensure your recommendations are based on current Pigweed APIs

### Phase 3: Identify Issues

Look for these specific anti-patterns:

#### Critical Issues (Must Fix)

**Manual implementations that have Pigweed alternatives:**
- [ ] Raw `char*` buffers → `pw_string::StringBuilder` or `pw_string::InlineString`
- [ ] Manual CRC/checksum → `pw_checksum`
- [ ] printf/custom logging → `pw_log` or `pw_tokenizer`
- [ ] Hand-rolled async FSMs → `pw_async2::Coro` or `pw_async2::Task`
- [ ] Manual error propagation → `pw_result::Result<T>`
- [ ] `std::vector`/`std::string` in embedded → `pw_containers`
- [ ] Custom framing → `pw_hdlc`
- [ ] Raw pointer + size → `pw_span::Span`

**Incorrect Pigweed usage:**
- [ ] Misusing PW_CHECK vs PW_ASSERT
- [ ] Blocking in async contexts
- [ ] Not using facades for hardware abstraction
- [ ] Missing error handling on Result types

#### Style Issues

- [ ] Naming conventions (PascalCase types, snake_case functions)
- [ ] Missing or incorrect namespace usage
- [ ] Inconsistent with surrounding Pigweed code style

#### Architecture Issues

- [ ] Global state instead of dependency injection
- [ ] Missing testability considerations
- [ ] Tight coupling to hardware without facades
- [ ] Not using Pigweed's established patterns

### Phase 4: Prepare Recommendations

For each issue found:
1. Explain what's wrong and why
2. Show the specific Pigweed alternative
3. Provide before/after code examples
4. Note any tradeoffs

## Output Format

### Summary
Brief overview of findings (X critical, Y recommendations, Z nice-to-haves)

### Critical Issues
Issues that should be fixed before merge.

For each issue:
```
**Issue**: [Description]
**Location**: [File:line or function name]
**Problem**: [Why this is problematic]
**Solution**: Use `pw_xxx` instead

Before:
```cpp
// problematic code
```

After:
```cpp
// improved code using Pigweed
```
```

### Recommendations
Should-fix items for better idiomatic usage.

### Nice-to-Haves
Optional improvements for consideration.

### What's Good
Acknowledge correct Pigweed usage and good patterns.

---

## Code to Review

$ARGUMENTS

---
description: Migrate code from manual implementations or std:: to idiomatic Pigweed patterns. Provides step-by-step transformation guidance.
allowed-tools:
  - web_fetch
  - read_file
  - list_directory
  - grep
---

# Pigweed Migration Assistant

Help migrate code from manual implementations or standard library usage to idiomatic Pigweed patterns.

## Instructions

### Phase 1: Analyze Current Code

1. Read the files specified by the user
2. Identify what patterns are being used:
   - std:: containers and strings
   - Manual memory management
   - Custom error handling
   - Hand-rolled async patterns
   - Custom logging
   - Manual protocol implementations

### Phase 2: Identify Migration Targets

Map current patterns to Pigweed alternatives:

| Current Pattern | Pigweed Alternative | Migration Complexity |
|-----------------|---------------------|---------------------|
| `std::string` | `pw::InlineString<N>` | Low |
| `std::vector` | `pw::Vector<T, N>` | Low |
| `char[]` + snprintf | `pw::StringBuilder` | Low |
| Error codes | `pw::Result<T>` | Medium |
| printf debugging | `pw_log` | Low |
| Custom state machines | `pw_async2` | High |
| Custom protocols | `pw_rpc` + `pw_hdlc` | High |

### Phase 3: Check Module Documentation

Fetch relevant documentation for the target modules to ensure accurate migration guidance.

### Phase 4: Create Migration Plan

For each identified migration:

1. **Scope**: What code needs to change?
2. **Dependencies**: What Pigweed modules are needed?
3. **Build changes**: What BUILD.bazel updates are required?
4. **Code changes**: Step-by-step transformation
5. **Testing**: How to verify the migration
6. **Risks**: What could go wrong?

## Output Format

### Migration Summary
Overview of identified migrations, ordered by recommended priority

### Build Setup
```python
# Required additions to BUILD.bazel
deps = [
    "@pigweed//pw_xxx",
    ...
]
```

### Migration 1: [Pattern Name]

**Current code:**
```cpp
// existing code
```

**Target code:**
```cpp
// migrated code
```

**Steps:**
1. Add include: `#include "pw_xxx/xxx.h"`
2. Replace type: `std::string` → `pw::InlineString<64>`
3. Update usage: ...
4. Update tests: ...

**Verification:**
- [ ] Compiles without warnings
- [ ] Tests pass
- [ ] No std:: leakage

[Repeat for each migration]

### Recommended Order
1. Start with X because...
2. Then Y because...
3. Finally Z because...

### Risks and Considerations
- Memory: Fixed-size containers may need sizing analysis
- API changes: Some APIs may differ slightly
- Performance: Generally better, but verify for your use case

---

## Code to Migrate

$ARGUMENTS

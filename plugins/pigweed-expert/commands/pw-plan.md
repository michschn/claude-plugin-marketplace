---
description: Plan a feature implementation using idiomatic Pigweed patterns. Use before implementing new features to ensure proper module selection and architecture.
allowed-tools:
  - web_fetch
  - read_file
  - list_directory
  - grep
---

# Pigweed Architecture Planning

You are helping plan a feature implementation using Pigweed idiomatically. This requires deep analysis to recommend the right modules and patterns.

## Instructions

Use extended thinking to work through this planning task thoroughly. The user wants genuine understanding and specific recommendations, not surface-level rules.

### Phase 1: Understand Requirements

First, analyze what the user is trying to build:
- What are the functional requirements?
- What are the constraints (memory, latency, power, real-time)?
- What hardware abstractions are needed?
- What communication protocols are involved?
- What error handling is required?

### Phase 2: Check Documentation

1. Check `.skills/pigweed/knowledge/` for cached module documentation
2. If docs are missing or you need specific module details, fetch from pigweed.dev:
   - `https://pigweed.dev/pw_<module>/docs.html` for module docs
   - `https://pigweed.dev/docs/concepts/` for design patterns
3. Focus on modules relevant to the user's requirements

### Phase 3: Module Selection

For each aspect of the feature, determine:
- Which Pigweed module(s) apply?
- What are the alternatives and tradeoffs?
- Are there module combinations that work well together?

Consider these module categories:
- **Async**: pw_async2 for cooperative multitasking
- **Error handling**: pw_result, pw_status
- **Data**: pw_containers, pw_span, pw_string
- **Communication**: pw_rpc, pw_hdlc, pw_stream
- **Storage**: pw_kvs, pw_blob_store
- **Logging**: pw_log, pw_tokenizer
- **Time**: pw_chrono
- **Sync**: pw_sync, pw_thread

### Phase 4: Architecture Design

Design the system following Pigweed principles:
- **Facade pattern**: Where are hardware abstractions needed?
- **Dependency injection**: What dependencies should be injected?
- **Testability**: How will this be tested on host?
- **Modularity**: How do components connect?

### Phase 5: Implementation Strategy

Create a concrete plan:
- What order should things be built?
- What are the risk areas?
- What BUILD.bazel targets are needed?
- What tests should be written first?

## Output Format

Provide your analysis in this structure:

### Summary
Brief overview of the recommended approach

### Recommended Modules
| Module | Purpose in This Feature |
|--------|------------------------|
| pw_xxx | Why it's needed |

### Architecture
- High-level design with component interactions
- Key interfaces/facades needed
- Dependency graph

### Build Targets
```python
# BUILD.bazel structure
pw_source_set("feature_name") {
  ...
}
```

### Implementation Phases
1. Phase 1: ...
2. Phase 2: ...

### Testing Strategy
- Host-side tests with pw_unit_test
- What to mock/fake
- Integration test approach

### Open Questions
- Things to resolve before/during implementation

---

## Feature to Plan

$ARGUMENTS

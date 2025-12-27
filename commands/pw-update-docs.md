---
description: Fetch and analyze the latest Pigweed documentation from pigweed.dev. Updates the local knowledge cache with current module information.
allowed-tools:
  - web_fetch
  - write_file
  - read_file
  - bash
---

# Update Pigweed Documentation Cache

Fetch the latest Pigweed documentation and generate a comprehensive knowledge summary.

## Instructions

### Phase 1: Fetch Core Documentation

Fetch these key pages from pigweed.dev:

**Concepts & Style:**
- https://pigweed.dev/docs/concepts/index.html
- https://pigweed.dev/docs/style_guide.html
- https://pigweed.dev/docs/embedded_cpp_guide.html

**Core Modules:**
- https://pigweed.dev/pw_async2/docs.html
- https://pigweed.dev/pw_result/docs.html
- https://pigweed.dev/pw_status/docs.html
- https://pigweed.dev/pw_string/docs.html
- https://pigweed.dev/pw_containers/docs.html
- https://pigweed.dev/pw_span/docs.html
- https://pigweed.dev/pw_log/docs.html
- https://pigweed.dev/pw_tokenizer/docs.html

**Communication:**
- https://pigweed.dev/pw_rpc/docs.html
- https://pigweed.dev/pw_hdlc/docs.html
- https://pigweed.dev/pw_stream/docs.html
- https://pigweed.dev/pw_protobuf/docs.html

**Concurrency:**
- https://pigweed.dev/pw_sync/docs.html
- https://pigweed.dev/pw_thread/docs.html

**Storage:**
- https://pigweed.dev/pw_kvs/docs.html
- https://pigweed.dev/pw_blob_store/docs.html

**Utilities:**
- https://pigweed.dev/pw_checksum/docs.html
- https://pigweed.dev/pw_chrono/docs.html
- https://pigweed.dev/pw_bytes/docs.html
- https://pigweed.dev/pw_unit_test/docs.html
- https://pigweed.dev/pw_assert/docs.html

### Phase 2: Analyze and Summarize

Use extended thinking to deeply analyze the documentation:

1. **Design Philosophy**: What are Pigweed's core principles?
2. **Module Relationships**: How do modules work together?
3. **Common Patterns**: What patterns appear across modules?
4. **Anti-patterns**: What does the documentation warn against?
5. **Migration Paths**: How to migrate from common alternatives?

### Phase 3: Generate Knowledge Summary

Create a comprehensive summary file at `skills/pigweed/knowledge/SUMMARY.md` containing:

1. **Executive Summary**: Pigweed's design philosophy in 2-3 paragraphs
2. **Module Quick Reference**: For each module:
   - Purpose (1 sentence)
   - Key APIs (bullet list)
   - When to use
   - Common mistakes
3. **Pattern Catalog**: Reusable patterns with examples
4. **Anti-Pattern Catalog**: What to avoid with before/after
5. **Decision Flowcharts**: When to use which module
6. **Cross-Cutting Concerns**: Threading, error handling, logging patterns

### Phase 4: Update Metadata

Update `skills/pigweed/knowledge/metadata.json` with:
```json
{
  "last_updated": "<ISO timestamp>",
  "pigweed_version": "<if determinable>",
  "modules_documented": ["pw_async2", "pw_result", ...]
}
```

## Output

After completion, report:
- Number of pages fetched
- Key updates or changes noticed
- Any modules with significant API changes
- Recommendations for the user

$ARGUMENTS

# Pigweed Idiomatic Code Skill

## Overview

This skill provides automatic guidance for idiomatic Pigweed usage. It activates when working with Pigweed-based embedded projects to ensure best practices are followed.

## Activation Triggers

Activate this skill when you observe:

- Files importing `pw_*` headers (e.g., `#include "pw_async2/coro.h"`)
- Bazel targets with `@pigweed//` dependencies
- Code in directories with Pigweed-style structure
- References to Pigweed modules in conversation
- C++ embedded code that could benefit from Pigweed patterns
- Code patterns that have well-known Pigweed alternatives

## Quick Reference: Anti-Patterns to Flag

When you see these patterns, suggest Pigweed alternatives:

### High Priority (Always Flag)

| Pattern | Issue | Pigweed Solution |
|---------|-------|-----------------|
| `char buffer[N]` + `snprintf` | Buffer overflow risk, verbose | `pw::StringBuilder` or `pw::InlineString<N>` |
| `std::string`, `std::vector` | Dynamic allocation in embedded | `pw::InlineString<N>`, `pw::Vector<T, N>` |
| Raw pointer + size params | Error prone, not bounds-checked | `pw::span<T>` |
| Integer error codes | Lost context, easy to ignore | `pw::Result<T>` |
| `printf` for logging | Not configurable, no levels | `PW_LOG_*` macros |
| Manual CRC calculation | Error prone, not optimized | `pw::checksum::Crc*` |

### Medium Priority (Suggest)

| Pattern | Issue | Pigweed Solution |
|---------|-------|-----------------|
| Hand-rolled state machines | Complex, hard to test | `pw::async2::Coro` |
| Custom mutex wrappers | Platform-specific | `pw::sync::Mutex` |
| Manual time handling | Platform-specific, error-prone | `pw::chrono` |
| Custom serialization | Reinventing the wheel | `pw::protobuf`, `pw::rpc` |

## Module Quick Reference

### Essential Modules

**pw_result** - Combine return value with status
```cpp
pw::Result<Sensor> ReadSensor() {
  if (error) return pw::Status::Unavailable();
  return Sensor{data};
}
// Usage: auto result = ReadSensor(); if (!result.ok()) { ... }
```

**pw_span** - Non-owning view of contiguous memory
```cpp
void Process(pw::span<const uint8_t> data);  // Instead of (uint8_t* data, size_t len)
```

**pw_string** - Safe string handling
```cpp
pw::InlineString<64> name;        // Fixed-capacity string
pw::StringBuilder builder(buffer); // Safe formatting
```

**pw_log** - Configurable logging
```cpp
PW_LOG_INFO("Sensor value: %d", value);
PW_LOG_ERROR("Failed: %s", status.str());
```

**pw_async2** - Cooperative async without RTOS threads
```cpp
pw::async2::Coro<pw::Status> DoWork(pw::async2::Dispatcher& dispatcher) {
  co_await SomeAsyncOp();
  co_return pw::OkStatus();
}
```

### Communication Modules

**pw_rpc** - Remote procedure calls (prefer over custom protocols)
**pw_hdlc** - Framing for serial communication
**pw_stream** - Abstract I/O interface

### Concurrency Modules

**pw_sync** - Mutex, Semaphore, ThreadNotification
**pw_thread** - Thread abstraction

## Design Principles to Enforce

### 1. Dependency Injection
```cpp
// Good: Inject dependencies
class SensorManager {
 public:
  SensorManager(pw::i2c::Initiator& i2c, Logger& log) 
    : i2c_(i2c), log_(log) {}
};

// Bad: Global state
class SensorManager {
  void Read() { GlobalI2C::Instance().Write(...); }
};
```

### 2. Facade Pattern
Use Pigweed facades for hardware abstraction. This enables host-side testing.

### 3. Fixed-Size Containers
In embedded, prefer `pw::Vector<T, N>` over `std::vector<T>` to avoid heap allocation.

### 4. Error Handling with Result
Always handle `pw::Result` - don't ignore the status:
```cpp
// Bad: Ignoring error
auto value = GetValue().value();

// Good: Handle errors
auto result = GetValue();
if (!result.ok()) {
  PW_LOG_ERROR("GetValue failed: %s", result.status().str());
  return result.status();
}
auto& value = *result;
```

## When to Consult @pigweed-expert

Invoke the full agent (`@pigweed-expert`) for:
- Architecture decisions requiring deep analysis
- Complex migration planning
- When you need extended thinking on tradeoffs
- Reviewing significant code changes

## Knowledge Cache

Check `skills/pigweed/knowledge/` for cached documentation:
- `SUMMARY.md` - Comprehensive module summary
- `metadata.json` - Cache freshness info

If cache is stale (>7 days) or missing needed module info, recommend running `/pw-update-docs`.

## Build Integration

Standard Pigweed Bazel dependency pattern:
```python
cc_library(
    name = "my_module",
    hdrs = ["my_module.h"],
    srcs = ["my_module.cc"],
    deps = [
        "@pigweed//pw_result",
        "@pigweed//pw_span",
        "@pigweed//pw_log",
    ],
)
```

## Testing Guidance

Pigweed code should be testable on host:
```python
pw_cc_test(
    name = "my_module_test",
    srcs = ["my_module_test.cc"],
    deps = [
        ":my_module",
        "@pigweed//pw_unit_test",
    ],
)
```

Use fakes/mocks for hardware dependencies via facades.

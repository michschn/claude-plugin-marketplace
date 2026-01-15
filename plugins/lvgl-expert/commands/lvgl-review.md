---
description: Review code for idiomatic LVGL usage. Identifies anti-patterns, suggests LVGL alternatives, and ensures best practices are followed.
allowed-tools:
  - web_fetch
  - read_file
  - list_directory
  - grep
---

# LVGL Code Review

You are conducting a thorough code review focused on idiomatic LVGL usage. This requires deep analysis to identify opportunities for improvement.

## Instructions

Use extended thinking to analyze the code thoroughly. The goal is to catch anti-patterns and suggest specific LVGL alternatives.

### Phase 1: Understand the Code

First, understand what the UI code is trying to accomplish:
- Read the files specified by the user
- Understand the overall UI structure and design
- Note any existing LVGL widget usage
- Identify the patterns being used

### Phase 2: Check Documentation

1. Check `.skills/lvgl/knowledge/` for relevant cached documentation
2. If you need specific widget details for recommendations, fetch from docs.lvgl.io
3. Ensure your recommendations are based on current LVGL APIs

### Phase 3: Identify Issues

Look for these specific anti-patterns:

#### Critical Issues (Must Fix)

**Manual implementations that have LVGL alternatives:**
- [ ] Custom text rendering → `lv_label_create()`
- [ ] Manual rectangle buttons → `lv_btn_create()`
- [ ] Custom progress bars → `lv_bar_create()`
- [ ] Manual coordinate calculations → flex/grid layouts
- [ ] Custom scrolling → `LV_OBJ_FLAG_SCROLLABLE`
- [ ] Manual touch detection → LVGL event system
- [ ] Custom animation loops → `lv_anim_t`
- [ ] Hardcoded colors everywhere → style system

**Incorrect LVGL usage:**
- [ ] Creating widgets without proper parent
- [ ] Not using style system for consistency
- [ ] Memory leaks (creating without deletion plan)
- [ ] Blocking in event callbacks
- [ ] Ignoring return values
- [ ] Using deprecated APIs

#### Style Issues

- [ ] Hardcoded positions instead of layouts
- [ ] Repeated style properties instead of shared styles
- [ ] Inconsistent color values
- [ ] Missing widget flags (clickable, scrollable, etc.)

#### Architecture Issues

- [ ] Flat widget hierarchy instead of logical containers
- [ ] Global variables for widget references
- [ ] No separation between UI and business logic
- [ ] Monolithic screen setup functions

### Phase 4: Prepare Recommendations

For each issue found:
1. Explain what's wrong and why
2. Show the specific LVGL alternative
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
**Solution**: Use `lv_xxx` instead

Before:
```c
// problematic code
```

After:
```c
// improved code using LVGL properly
```
```

### Recommendations
Should-fix items for better idiomatic usage.

### Nice-to-Haves
Optional improvements for consideration.

### What's Good
Acknowledge correct LVGL usage and good patterns.

---

## Code to Review

$ARGUMENTS

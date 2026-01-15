---
description: Explain an LVGL widget or concept in depth with examples, common patterns, and integration guidance. Great for learning LVGL.
allowed-tools:
  - web_fetch
  - read_file
---

# Explain LVGL Widget/Concept

Provide an in-depth explanation of an LVGL widget or concept with practical examples and integration guidance.

## Instructions

### Phase 1: Fetch Documentation

1. Check `.skills/lvgl/knowledge/` for cached info on this topic
2. Fetch the latest documentation from docs.lvgl.io:
   - `https://docs.lvgl.io/master/widgets/<widget>.html` for widget docs
   - `https://docs.lvgl.io/master/overview/<concept>.html` for concepts
   - `https://docs.lvgl.io/master/layouts/<layout>.html` for layouts
   - Look for guide pages, API reference, and examples

### Phase 2: Analyze Topic

Use extended thinking to understand:
- What problem does this widget/concept solve?
- What are the key functions and properties?
- How does it integrate with other LVGL features?
- What are the design decisions and tradeoffs?
- What are common usage patterns?
- What mistakes do people commonly make?

### Phase 3: Explain

Provide a comprehensive explanation covering:

## Overview
What this widget/concept does and when to use it (2-3 paragraphs)

## Key Functions/Properties
Core API elements and their purposes

## Basic Usage
Simple example showing the most common use case

```c
// Basic example
lv_obj_t *widget = lv_xxx_create(parent);
lv_xxx_set_yyy(widget, value);
```

## Advanced Patterns
More sophisticated usage examples

## Styling
How to style this widget using the style system

```c
// Styling example
static lv_style_t style;
lv_style_init(&style);
lv_style_set_xxx(&style, value);
lv_obj_add_style(widget, &style, LV_PART_MAIN);
```

## Events
What events this widget emits and how to handle them

## Integration
How this widget works with layouts, screens, and other widgets

## Common Mistakes
What to avoid and why

## Memory Considerations
Object size, when to delete, parent relationships

---

## Topic to Explain

$ARGUMENTS

---
description: Fetch and analyze the latest LVGL documentation from docs.lvgl.io. Updates the local knowledge cache with current widget and API information.
allowed-tools:
  - web_fetch
  - write_file
  - read_file
  - bash
---

# Update LVGL Documentation Cache

Fetch the latest LVGL documentation and generate a comprehensive knowledge summary.

## Instructions

### Phase 1: Fetch Core Documentation

Fetch these key pages from docs.lvgl.io:

**Overview & Concepts:**
- https://docs.lvgl.io/master/overview/index.html
- https://docs.lvgl.io/master/overview/obj.html
- https://docs.lvgl.io/master/overview/style.html
- https://docs.lvgl.io/master/overview/event.html
- https://docs.lvgl.io/master/overview/indev.html
- https://docs.lvgl.io/master/overview/display.html

**Layouts:**
- https://docs.lvgl.io/master/layouts/flex.html
- https://docs.lvgl.io/master/layouts/grid.html

**Core Widgets:**
- https://docs.lvgl.io/master/widgets/obj.html
- https://docs.lvgl.io/master/widgets/label.html
- https://docs.lvgl.io/master/widgets/btn.html
- https://docs.lvgl.io/master/widgets/btnmatrix.html
- https://docs.lvgl.io/master/widgets/img.html
- https://docs.lvgl.io/master/widgets/arc.html
- https://docs.lvgl.io/master/widgets/bar.html
- https://docs.lvgl.io/master/widgets/slider.html
- https://docs.lvgl.io/master/widgets/switch.html

**Input Widgets:**
- https://docs.lvgl.io/master/widgets/textarea.html
- https://docs.lvgl.io/master/widgets/dropdown.html
- https://docs.lvgl.io/master/widgets/roller.html
- https://docs.lvgl.io/master/widgets/keyboard.html
- https://docs.lvgl.io/master/widgets/spinbox.html
- https://docs.lvgl.io/master/widgets/checkbox.html

**Container Widgets:**
- https://docs.lvgl.io/master/widgets/tabview.html
- https://docs.lvgl.io/master/widgets/tileview.html
- https://docs.lvgl.io/master/widgets/win.html
- https://docs.lvgl.io/master/widgets/menu.html

**Data Display:**
- https://docs.lvgl.io/master/widgets/chart.html
- https://docs.lvgl.io/master/widgets/table.html
- https://docs.lvgl.io/master/widgets/meter.html
- https://docs.lvgl.io/master/widgets/led.html
- https://docs.lvgl.io/master/widgets/msgbox.html

**Drawing & Animations:**
- https://docs.lvgl.io/master/overview/animation.html
- https://docs.lvgl.io/master/overview/drawing.html
- https://docs.lvgl.io/master/overview/layer.html

### Phase 2: Analyze and Summarize

Use extended thinking to deeply analyze the documentation:

1. **Design Philosophy**: What are LVGL's core principles?
2. **Widget Relationships**: How do widgets work together?
3. **Common Patterns**: What patterns appear across widgets?
4. **Anti-patterns**: What does the documentation warn against?
5. **Style System**: How should styles be organized?
6. **Event System**: How should events be handled?
7. **Layout System**: When to use flex vs grid vs manual?

### Phase 3: Generate Knowledge Summary

Create a comprehensive summary file at `.skills/lvgl/knowledge/SUMMARY.md` containing:

1. **Executive Summary**: LVGL's design philosophy in 2-3 paragraphs
2. **Widget Quick Reference**: For each widget:
   - Purpose (1 sentence)
   - Key APIs (bullet list)
   - When to use
   - Common mistakes
3. **Pattern Catalog**: Reusable patterns with examples
4. **Anti-Pattern Catalog**: What to avoid with before/after
5. **Style System Guide**: How to organize styles
6. **Layout Decision Guide**: When to use flex/grid/manual
7. **Event Handling Patterns**: Best practices for events

### Phase 4: Update Metadata

Update `.skills/lvgl/knowledge/metadata.json` with:
```json
{
  "last_updated": "<ISO timestamp>",
  "lvgl_version": "<if determinable from docs>",
  "widgets_documented": ["lv_obj", "lv_label", ...]
}
```

## Output

After completion, report:
- Number of pages fetched
- Key updates or changes noticed
- Any widgets with significant API changes
- Recommendations for the user

$ARGUMENTS

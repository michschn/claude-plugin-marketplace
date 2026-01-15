---
description: Plan a UI implementation using idiomatic LVGL patterns. Use before implementing new UI features to ensure proper widget selection and architecture.
allowed-tools:
  - web_fetch
  - read_file
  - list_directory
  - grep
---

# LVGL UI Architecture Planning

You are helping plan a UI implementation using LVGL idiomatically. This requires deep analysis to recommend the right widgets and patterns.

## Instructions

Use extended thinking to work through this planning task thoroughly. The user wants genuine understanding and specific recommendations, not surface-level rules.

### Phase 1: Understand Requirements

First, analyze what the user is trying to build:
- What are the UI requirements (screens, interactions)?
- What are the constraints (memory, display size, color depth)?
- What input methods are available (touch, encoder, keyboard)?
- What navigation flow is needed?
- What data needs to be displayed and updated?

### Phase 2: Check Documentation

1. Check `.skills/lvgl/knowledge/` for cached widget documentation
2. If docs are missing or you need specific widget details, fetch from docs.lvgl.io:
   - `https://docs.lvgl.io/master/widgets/` for widget docs
   - `https://docs.lvgl.io/master/layouts/` for layout docs
   - `https://docs.lvgl.io/master/overview/` for concepts
3. Focus on widgets relevant to the user's requirements

### Phase 3: Widget Selection

For each UI element, determine:
- Which LVGL widget(s) apply?
- What are the alternatives and tradeoffs?
- Are there widget combinations that work well together?

Consider these widget categories:
- **Containers**: lv_obj, lv_tabview, lv_tileview, lv_win
- **Inputs**: lv_btn, lv_textarea, lv_dropdown, lv_slider, lv_switch
- **Display**: lv_label, lv_img, lv_chart, lv_bar, lv_meter
- **Navigation**: lv_tabview, lv_menu, lv_msgbox
- **Layouts**: flex, grid

### Phase 4: Architecture Design

Design the UI following LVGL principles:
- **Screen hierarchy**: How are screens organized?
- **Style system**: What styles should be defined?
- **Event handling**: How will interactions be handled?
- **Memory**: Are there memory constraints to consider?
- **Responsive**: Does it need to work on multiple screen sizes?

### Phase 5: Implementation Strategy

Create a concrete plan:
- What order should screens/widgets be built?
- What are the risk areas?
- What styles should be defined first?
- How should the code be organized?

## Output Format

Provide your analysis in this structure:

### Summary
Brief overview of the recommended approach

### Recommended Widgets
| Widget | Purpose in This UI |
|--------|-------------------|
| lv_xxx | Why it's needed |

### Screen Architecture
- Screen hierarchy and navigation flow
- Key containers and their children
- Layout approach (flex/grid/manual)

### Style System
```c
// Styles to define
static lv_style_t style_xxx;
```

### Implementation Phases
1. Phase 1: ...
2. Phase 2: ...

### Event Handling Strategy
- How user interactions will be handled
- Event callbacks needed

### Memory Considerations
- Object count estimates
- Style sharing opportunities

### Open Questions
- Things to resolve before/during implementation

---

## Feature to Plan

$ARGUMENTS

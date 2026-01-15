# LVGL Knowledge Summary

## Executive Summary

LVGL (Light and Versatile Graphics Library) is a modular, widget-centric graphics library designed for embedded systems. Its architecture emphasizes portability across platforms (microcontrollers, RTOS, embedded Linux, PC), flexibility through configurable components, and performance via hardware acceleration support.

The library organizes UI elements hierarchically with all widgets inheriting from a base object class (`lv_obj`). This enables consistent behavior across widgets for styling, events, layouts, scrolling, and state management. LVGL provides powerful layout systems (Flex and Grid) inspired by CSS, eliminating the need for manual coordinate calculations.

Key design principles include: separation of display drivers from core logic, input device abstraction, a comprehensive style system with themes, and an event-driven architecture. The library prioritizes memory efficiency with options for static allocation and provides animation capabilities built into the core.

---

## Widget Quick Reference

### Base Widget (lv_obj)

**Purpose**: Foundation widget from which all others inherit. Can serve as a container.

**Key APIs**:
- `lv_obj_create(parent)` - Create base object
- `lv_obj_add_flag()` / `lv_obj_remove_flag()` - Toggle behavior flags
- `lv_obj_add_state()` / `lv_obj_remove_state()` - Manage states
- `lv_obj_add_event_cb()` - Add event callbacks
- `lv_obj_add_style()` - Apply styles
- `lv_obj_set_size()` / `lv_obj_set_pos()` - Dimensions and position
- `lv_obj_set_flex_flow()` / `lv_obj_set_grid_dsc_array()` - Layout

**When to use**: As containers, custom widgets, or when no specific widget fits.

**Common mistakes**:
- Creating widgets without proper parent hierarchy
- Not using layouts (manual positioning instead)
- Forgetting to delete or manage object lifecycle

---

### Label (lv_label)

**Purpose**: Display text with various overflow handling modes.

**Key APIs**:
- `lv_label_create(parent)` - Create label
- `lv_label_set_text(label, "text")` - Set text (copies string)
- `lv_label_set_text_static(label, "text")` - Set text (no copy, use for static strings)
- `lv_label_set_text_fmt(label, "fmt", ...)` - Printf-style formatting
- `lv_label_set_long_mode(label, mode)` - Set overflow behavior
- `lv_label_set_recolor(label, true)` - Enable inline color syntax

**Long modes**:
- `LV_LABEL_LONG_WRAP` - Wrap and expand height
- `LV_LABEL_LONG_DOTS` - Truncate with "..."
- `LV_LABEL_LONG_SCROLL` - Scroll back and forth
- `LV_LABEL_LONG_SCROLL_CIRCULAR` - Continuous scroll
- `LV_LABEL_LONG_CLIP` - Just clip

**When to use**: Any text display - prefer over custom text rendering.

**Common mistakes**:
- Using `lv_label_set_text()` for rapidly updating text (>30/sec) - use `lv_label_set_text_static()` instead
- Not setting long mode for potentially long text
- Custom text rendering instead of using labels

---

### Button (lv_button)

**Purpose**: Interactive button with built-in states and events.

**Key APIs**:
- `lv_button_create(parent)` - Create button
- `lv_obj_add_flag(btn, LV_OBJ_FLAG_CHECKABLE)` - Make toggle button
- `lv_obj_add_event_cb(btn, cb, LV_EVENT_CLICKED, user_data)` - Add click handler

**Events**:
- `LV_EVENT_CLICKED` - Button was clicked
- `LV_EVENT_VALUE_CHANGED` - Toggle state changed (if checkable)
- `LV_EVENT_PRESSED` / `LV_EVENT_RELEASED` - Press states

**When to use**: Any clickable UI element - prefer over custom touch detection.

**Common mistakes**:
- Manual rectangle drawing for buttons
- Custom touch region detection
- Not adding a label child for button text

---

### Bar (lv_bar)

**Purpose**: Progress indicator with background and fill.

**Key APIs**:
- `lv_bar_create(parent)` - Create bar
- `lv_bar_set_value(bar, value, LV_ANIM_ON/OFF)` - Set progress
- `lv_bar_set_range(bar, min, max)` - Set value range
- `lv_bar_set_mode(bar, mode)` - Set operating mode
- `lv_bar_set_start_value(bar, value, anim)` - For range mode

**Modes**:
- `LV_BAR_MODE_NORMAL` - Left-to-right fill
- `LV_BAR_MODE_SYMMETRICAL` - From center (needs -/+ range)
- `LV_BAR_MODE_RANGE` - Show range between start and end values

**When to use**: Progress indicators, loading bars, level displays.

**Common mistakes**:
- Custom rectangle drawing for progress bars
- Not using animation for smooth transitions

---

### Slider (lv_slider)

**Purpose**: User-adjustable value selector with draggable knob.

**Key APIs**:
- `lv_slider_create(parent)` - Create slider
- `lv_slider_set_value(slider, value, anim)` - Set value
- `lv_slider_set_range(slider, min, max)` - Set range
- `lv_slider_get_value(slider)` - Get current value
- `lv_slider_set_mode(slider, mode)` - Set mode

**Events**:
- `LV_EVENT_VALUE_CHANGED` - Continuous updates while dragging
- `LV_EVENT_RELEASED` - When dragging ends

**When to use**: Volume controls, brightness, any adjustable numeric value.

**Common mistakes**:
- Getting value only on RELEASED (miss continuous feedback)
- Custom slider implementations

---

### Switch (lv_switch)

**Purpose**: Toggle switch with ON/OFF states.

**Key APIs**:
- `lv_switch_create(parent)` - Create switch
- `lv_obj_add_state(sw, LV_STATE_CHECKED)` - Turn ON
- `lv_obj_remove_state(sw, LV_STATE_CHECKED)` - Turn OFF
- `lv_obj_has_state(sw, LV_STATE_CHECKED)` - Check state

**Events**:
- `LV_EVENT_VALUE_CHANGED` - State toggled

**When to use**: Boolean settings, on/off toggles.

**Common mistakes**:
- Using checkbox for toggle settings (switch is more appropriate for settings)
- Custom toggle implementations

---

### Dropdown (lv_dropdown)

**Purpose**: Selection from a dropdown list of options.

**Key APIs**:
- `lv_dropdown_create(parent)` - Create dropdown
- `lv_dropdown_set_options(dd, "Opt1\nOpt2\nOpt3")` - Set options
- `lv_dropdown_set_selected(dd, index)` - Set selection
- `lv_dropdown_get_selected(dd)` - Get selected index
- `lv_dropdown_get_selected_str(dd, buf, buf_size)` - Get selected text
- `lv_dropdown_set_dir(dd, LV_DIR_xxx)` - Set list direction
- `lv_dropdown_open(dd)` / `lv_dropdown_close(dd)` - Control list

**Events**:
- `LV_EVENT_VALUE_CHANGED` - Selection changed
- `LV_EVENT_READY` - List opened
- `LV_EVENT_CANCEL` - List closed

**When to use**: Selection from multiple options, menus.

**Common mistakes**:
- Custom dropdown implementations
- Not using `lv_dropdown_set_options_static()` for constant strings

---

### Text Area (lv_textarea)

**Purpose**: Multi-line text input with cursor.

**Key APIs**:
- `lv_textarea_create(parent)` - Create textarea
- `lv_textarea_add_char(ta, 'c')` - Insert character
- `lv_textarea_add_text(ta, "text")` - Insert text
- `lv_textarea_set_text(ta, "text")` - Replace all text
- `lv_textarea_get_text(ta)` - Get text
- `lv_textarea_set_placeholder_text(ta, "hint")` - Set placeholder
- `lv_textarea_set_one_line(ta, true)` - Single line mode
- `lv_textarea_set_password_mode(ta, true)` - Password masking
- `lv_textarea_set_accepted_chars(ta, "0123456789")` - Input filter
- `lv_textarea_set_max_length(ta, num)` - Character limit

**Events**:
- `LV_EVENT_VALUE_CHANGED` - Text changed
- `LV_EVENT_INSERT` - Before insertion (can modify)
- `LV_EVENT_READY` - Enter pressed (one-line mode)

**When to use**: Any text input - names, passwords, search, numbers.

**Common mistakes**:
- Custom text input implementations
- Not setting accepted_chars for numeric input

---

### Tab View (lv_tabview)

**Purpose**: Tabbed interface with multiple pages.

**Key APIs**:
- `lv_tabview_create(parent)` - Create tabview
- `lv_tabview_add_tab(tv, "Tab Name")` - Add tab (returns container)
- `lv_tabview_set_active(tv, index, anim)` - Switch tab
- `lv_tabview_get_tab_active(tv)` - Get active tab index
- `lv_tabview_set_tab_bar_position(tv, LV_DIR_xxx)` - Tab bar position
- `lv_tabview_set_tab_bar_size(tv, size)` - Tab bar size

**Events**:
- `LV_EVENT_VALUE_CHANGED` - Tab changed

**When to use**: Multi-page interfaces, settings screens.

**Common mistakes**:
- Custom tab switching logic
- Not using tab containers for content

---

### Chart (lv_chart)

**Purpose**: Line, bar, and scatter charts for data visualization.

**Key APIs**:
- `lv_chart_create(parent)` - Create chart
- `lv_chart_set_type(chart, type)` - Set chart type
- `lv_chart_add_series(chart, color, axis)` - Add data series
- `lv_chart_set_next_value(chart, series, value)` - Add data point
- `lv_chart_set_point_count(chart, count)` - Set point count
- `lv_chart_set_range(chart, axis, min, max)` - Set axis range
- `lv_chart_refresh(chart)` - Redraw after direct array changes

**Chart types**: `LV_CHART_TYPE_LINE`, `LV_CHART_TYPE_BAR`, `LV_CHART_TYPE_SCATTER`

**Update modes**: `LV_CHART_UPDATE_MODE_SHIFT`, `LV_CHART_UPDATE_MODE_CIRCULAR`

**When to use**: Data visualization, sensor readings, statistics.

**Common mistakes**:
- Custom graph drawing
- Not calling refresh after direct array modification

---

### Arc (lv_arc)

**Purpose**: Circular progress/control with adjustable indicator.

**Key APIs**:
- `lv_arc_create(parent)` - Create arc
- `lv_arc_set_value(arc, value)` - Set value
- `lv_arc_set_range(arc, min, max)` - Set range
- `lv_arc_set_bg_angles(arc, start, end)` - Background arc angles
- `lv_arc_set_angles(arc, start, end)` - Indicator angles
- `lv_arc_set_rotation(arc, degrees)` - Rotate entire arc
- `lv_arc_set_mode(arc, mode)` - Set mode

**Modes**: `LV_ARC_MODE_NORMAL`, `LV_ARC_MODE_REVERSE`, `LV_ARC_MODE_SYMMETRICAL`

**When to use**: Circular progress, gauges, rotary controls.

**Common mistakes**:
- Custom circular drawing
- Not using rotation for different orientations

---

### List (lv_list)

**Purpose**: Vertically scrolling list of buttons and text.

**Key APIs**:
- `lv_list_create(parent)` - Create list
- `lv_list_add_button(list, icon, text)` - Add button item
- `lv_list_add_text(list, text)` - Add text/header

**When to use**: Scrollable menus, settings lists, file browsers.

**Common mistakes**:
- Manual list implementations
- Not using icons from LV_SYMBOL_xxx

---

### Message Box (lv_msgbox)

**Purpose**: Modal dialog with title, text, and buttons.

**Key APIs**:
- `lv_msgbox_create(parent)` - Create (NULL parent = modal)
- `lv_msgbox_add_title(mb, title)` - Add title
- `lv_msgbox_add_text(mb, text)` - Add body text
- `lv_msgbox_add_close_button(mb)` - Add close button
- `lv_msgbox_add_footer_button(mb, text)` - Add action button
- `lv_msgbox_close(mb)` - Close and delete

**When to use**: Alerts, confirmations, dialogs.

**Common mistakes**:
- Custom dialog implementations
- Forgetting to close/delete message boxes

---

## Layout System Guide

### When to Use Flex Layout

Use Flex for **one-dimensional** arrangements:
- Horizontal button rows
- Vertical lists
- Navigation bars
- Toolbars
- Dynamic content that grows/shrinks

```c
lv_obj_set_flex_flow(container, LV_FLEX_FLOW_ROW);
lv_obj_set_flex_align(container, LV_FLEX_ALIGN_SPACE_EVENLY,
                      LV_FLEX_ALIGN_CENTER, LV_FLEX_ALIGN_CENTER);

// Make a child fill available space
lv_obj_set_flex_grow(child, 1);
```

**Flex Flow Options**:
- `LV_FLEX_FLOW_ROW` / `LV_FLEX_FLOW_COLUMN` - Basic layouts
- `LV_FLEX_FLOW_ROW_WRAP` / `LV_FLEX_FLOW_COLUMN_WRAP` - With wrapping
- `LV_FLEX_FLOW_ROW_REVERSE` / `LV_FLEX_FLOW_COLUMN_REVERSE` - Reversed

**Alignment Options**:
- `LV_FLEX_ALIGN_START` / `LV_FLEX_ALIGN_END` / `LV_FLEX_ALIGN_CENTER`
- `LV_FLEX_ALIGN_SPACE_EVENLY` / `LV_FLEX_ALIGN_SPACE_AROUND` / `LV_FLEX_ALIGN_SPACE_BETWEEN`

### When to Use Grid Layout

Use Grid for **two-dimensional** arrangements:
- Form layouts
- Calculator keypads
- Dashboard cards
- Any table-like structure

```c
static int32_t col_dsc[] = {100, LV_GRID_FR(1), 100, LV_GRID_TEMPLATE_LAST};
static int32_t row_dsc[] = {50, 50, LV_GRID_TEMPLATE_LAST};

lv_obj_set_grid_dsc_array(container, col_dsc, row_dsc);
lv_obj_set_layout(container, LV_LAYOUT_GRID);

// Place child at column 1, row 0
lv_obj_set_grid_cell(child, LV_GRID_ALIGN_STRETCH, 1, 1,
                     LV_GRID_ALIGN_CENTER, 0, 1);
```

**Size Options**:
- Pixel values: `100`
- Content-sized: `LV_GRID_CONTENT`
- Proportional: `LV_GRID_FR(1)`, `LV_GRID_FR(2)` (like CSS fr units)

**Alignment Options**:
- `LV_GRID_ALIGN_START` / `LV_GRID_ALIGN_CENTER` / `LV_GRID_ALIGN_END`
- `LV_GRID_ALIGN_STRETCH` - Fill cell

### When to Use Manual Positioning

Rarely! Only for:
- Overlapping elements
- Pixel-perfect designs for specific screens
- Complex custom widgets

---

## Style System Guide

### Style Architecture

LVGL styles are composed of:
- **Properties**: Visual attributes (colors, sizes, etc.)
- **Parts**: Widget sub-elements (`LV_PART_MAIN`, `LV_PART_INDICATOR`, etc.)
- **States**: Widget states (`LV_STATE_DEFAULT`, `LV_STATE_PRESSED`, etc.)

### Defining Reusable Styles

```c
// Define once (static = persists)
static lv_style_t style_btn;
lv_style_init(&style_btn);
lv_style_set_bg_color(&style_btn, lv_palette_main(LV_PALETTE_BLUE));
lv_style_set_radius(&style_btn, 10);
lv_style_set_shadow_width(&style_btn, 10);
lv_style_set_shadow_ofs_y(&style_btn, 5);

// Apply to multiple widgets
lv_obj_add_style(btn1, &style_btn, 0);
lv_obj_add_style(btn2, &style_btn, 0);
```

### Applying Styles to Parts and States

```c
// Style for pressed state
static lv_style_t style_pressed;
lv_style_init(&style_pressed);
lv_style_set_bg_color(&style_pressed, lv_palette_darken(LV_PALETTE_BLUE, 2));

lv_obj_add_style(btn, &style_btn, LV_PART_MAIN);
lv_obj_add_style(btn, &style_pressed, LV_PART_MAIN | LV_STATE_PRESSED);
```

### Common Style Properties

**Background**:
- `bg_color`, `bg_opa`, `bg_grad_color`, `bg_grad_dir`

**Border**:
- `border_color`, `border_width`, `border_opa`, `border_side`

**Outline** (outside border):
- `outline_color`, `outline_width`, `outline_opa`

**Shadow**:
- `shadow_color`, `shadow_width`, `shadow_ofs_x`, `shadow_ofs_y`

**Padding/Margin**:
- `pad_top`, `pad_bottom`, `pad_left`, `pad_right`

**Size**:
- `width`, `height`, `min_width`, `max_width`, `radius`

**Text**:
- `text_color`, `text_font`, `text_align`, `text_letter_space`

### Style Parts Reference

| Part | Description |
|------|-------------|
| `LV_PART_MAIN` | Background/main area |
| `LV_PART_INDICATOR` | Value indicator (bar, slider) |
| `LV_PART_KNOB` | Draggable handle |
| `LV_PART_ITEMS` | List/dropdown items |
| `LV_PART_CURSOR` | Textarea cursor |
| `LV_PART_SCROLLBAR` | Scrollbar |
| `LV_PART_SELECTED` | Selected text |

### Style States Reference

| State | Description |
|-------|-------------|
| `LV_STATE_DEFAULT` | Normal state |
| `LV_STATE_PRESSED` | Being pressed |
| `LV_STATE_FOCUSED` | Has keyboard/encoder focus |
| `LV_STATE_CHECKED` | Toggle is on |
| `LV_STATE_DISABLED` | Widget is disabled |

---

## Event Handling Patterns

### Basic Event Callback

```c
static void event_cb(lv_event_t *e) {
    lv_event_code_t code = lv_event_get_code(e);
    lv_obj_t *target = lv_event_get_target(e);
    void *user_data = lv_event_get_user_data(e);

    if(code == LV_EVENT_CLICKED) {
        // Handle click
    }
}

lv_obj_add_event_cb(btn, event_cb, LV_EVENT_CLICKED, &my_data);
```

### Common Event Types

| Event | When Triggered |
|-------|---------------|
| `LV_EVENT_CLICKED` | Object clicked |
| `LV_EVENT_VALUE_CHANGED` | Value changed (slider, dropdown, etc.) |
| `LV_EVENT_PRESSED` | Press started |
| `LV_EVENT_RELEASED` | Press ended |
| `LV_EVENT_FOCUSED` | Gained focus |
| `LV_EVENT_DEFOCUSED` | Lost focus |
| `LV_EVENT_DELETE` | Object being deleted |
| `LV_EVENT_SCROLL` | Scrolling |

### Multiple Widgets, One Handler

```c
static void btn_handler(lv_event_t *e) {
    int *id = lv_event_get_user_data(e);
    printf("Button %d clicked\n", *id);
}

static int btn_ids[] = {1, 2, 3};
for(int i = 0; i < 3; i++) {
    lv_obj_t *btn = lv_button_create(parent);
    lv_obj_add_event_cb(btn, btn_handler, LV_EVENT_CLICKED, &btn_ids[i]);
}
```

---

## Animation Guide

### Basic Animation

```c
lv_anim_t anim;
lv_anim_init(&anim);
lv_anim_set_var(&anim, obj);
lv_anim_set_exec_cb(&anim, (lv_anim_exec_xcb_t)lv_obj_set_x);
lv_anim_set_values(&anim, 0, 100);
lv_anim_set_duration(&anim, 500);
lv_anim_set_path_cb(&anim, lv_anim_path_ease_out);
lv_anim_start(&anim);
```

### Animation Paths

- `lv_anim_path_linear` - Constant speed
- `lv_anim_path_ease_in` - Slow start
- `lv_anim_path_ease_out` - Slow end
- `lv_anim_path_ease_in_out` - Slow start and end
- `lv_anim_path_bounce` - Bounce effect
- `lv_anim_path_overshoot` - Overshoot end value

### Animation Timeline

```c
lv_anim_timeline_t *timeline = lv_anim_timeline_create();
lv_anim_timeline_add(timeline, 0, &anim1);      // Start immediately
lv_anim_timeline_add(timeline, 200, &anim2);    // Start at 200ms
lv_anim_timeline_add(timeline, 400, &anim3);    // Start at 400ms
lv_anim_timeline_start(timeline);
```

---

## Anti-Pattern Catalog

### Manual Text Rendering → lv_label

**Before (wrong)**:
```c
// Don't do this
void draw_text(int x, int y, const char *text) {
    // Custom text rendering...
}
```

**After (correct)**:
```c
lv_obj_t *label = lv_label_create(parent);
lv_label_set_text(label, text);
lv_obj_set_pos(label, x, y);
```

### Manual Buttons → lv_button

**Before (wrong)**:
```c
// Don't do this
void draw_button(int x, int y, int w, int h) {
    draw_rect(x, y, w, h, color);
    if(touch_in_rect(x, y, w, h)) { ... }
}
```

**After (correct)**:
```c
lv_obj_t *btn = lv_button_create(parent);
lv_obj_set_size(btn, w, h);
lv_obj_add_event_cb(btn, click_cb, LV_EVENT_CLICKED, NULL);
```

### Hardcoded Positions → Layouts

**Before (wrong)**:
```c
// Don't do this - breaks on different screen sizes
lv_obj_set_pos(btn1, 10, 10);
lv_obj_set_pos(btn2, 120, 10);
lv_obj_set_pos(btn3, 230, 10);
```

**After (correct)**:
```c
lv_obj_set_flex_flow(container, LV_FLEX_FLOW_ROW);
lv_obj_set_flex_align(container, LV_FLEX_ALIGN_SPACE_EVENLY,
                      LV_FLEX_ALIGN_CENTER, LV_FLEX_ALIGN_CENTER);
// Children automatically positioned
```

### Repeated Style Properties → Shared Styles

**Before (wrong)**:
```c
// Don't do this - repetitive and hard to maintain
lv_obj_set_style_bg_color(btn1, lv_color_hex(0x2196F3), 0);
lv_obj_set_style_radius(btn1, 10, 0);
lv_obj_set_style_bg_color(btn2, lv_color_hex(0x2196F3), 0);
lv_obj_set_style_radius(btn2, 10, 0);
```

**After (correct)**:
```c
static lv_style_t style_btn;
lv_style_init(&style_btn);
lv_style_set_bg_color(&style_btn, lv_color_hex(0x2196F3));
lv_style_set_radius(&style_btn, 10);
lv_obj_add_style(btn1, &style_btn, 0);
lv_obj_add_style(btn2, &style_btn, 0);
```

### Global Widget References → User Data

**Before (wrong)**:
```c
// Don't do this - global state is error-prone
static lv_obj_t *g_label;

void btn_cb(lv_event_t *e) {
    lv_label_set_text(g_label, "Clicked");
}
```

**After (correct)**:
```c
void btn_cb(lv_event_t *e) {
    lv_obj_t *label = lv_event_get_user_data(e);
    lv_label_set_text(label, "Clicked");
}

lv_obj_add_event_cb(btn, btn_cb, LV_EVENT_CLICKED, label);
```

### Custom Animation Loop → lv_anim

**Before (wrong)**:
```c
// Don't do this
static int anim_x = 0;
void timer_cb(lv_timer_t *t) {
    anim_x += 5;
    lv_obj_set_x(obj, anim_x);
}
```

**After (correct)**:
```c
lv_anim_t anim;
lv_anim_init(&anim);
lv_anim_set_var(&anim, obj);
lv_anim_set_exec_cb(&anim, (lv_anim_exec_xcb_t)lv_obj_set_x);
lv_anim_set_values(&anim, 0, 100);
lv_anim_set_duration(&anim, 500);
lv_anim_start(&anim);
```

---

## Memory Considerations

### Object Lifecycle

- Objects are automatically deleted when parent is deleted
- Use `lv_obj_del()` for explicit deletion
- Use `LV_EVENT_DELETE` callback for cleanup

### Static Allocation

- Define styles as `static` - they must persist
- Use `lv_label_set_text_static()` for constant strings
- Grid/flex descriptors must be static arrays

### Object Count Awareness

- Each widget consumes RAM
- Complex widgets (chart, table) use more
- Consider object pooling for dynamic lists

---

## Quick Start Template

```c
#include "lvgl.h"

// Define styles once
static lv_style_t style_btn;

void ui_init(void) {
    // Initialize styles
    lv_style_init(&style_btn);
    lv_style_set_bg_color(&style_btn, lv_palette_main(LV_PALETTE_BLUE));
    lv_style_set_radius(&style_btn, 8);

    // Create main screen container with flex layout
    lv_obj_t *screen = lv_scr_act();
    lv_obj_t *container = lv_obj_create(screen);
    lv_obj_set_size(container, LV_PCT(100), LV_PCT(100));
    lv_obj_set_flex_flow(container, LV_FLEX_FLOW_COLUMN);
    lv_obj_set_flex_align(container, LV_FLEX_ALIGN_CENTER,
                          LV_FLEX_ALIGN_CENTER, LV_FLEX_ALIGN_CENTER);

    // Add title
    lv_obj_t *title = lv_label_create(container);
    lv_label_set_text(title, "My App");

    // Add button
    lv_obj_t *btn = lv_button_create(container);
    lv_obj_add_style(btn, &style_btn, 0);
    lv_obj_add_event_cb(btn, btn_clicked, LV_EVENT_CLICKED, NULL);

    lv_obj_t *btn_label = lv_label_create(btn);
    lv_label_set_text(btn_label, "Click Me");
}

static void btn_clicked(lv_event_t *e) {
    LV_LOG_USER("Button clicked!");
}
```

---

*Last updated: See metadata.json for timestamp*

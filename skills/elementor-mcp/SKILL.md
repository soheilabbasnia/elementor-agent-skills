---
name: elementor-mcp
description: >-
  Use this skill whenever working with an Elementor or Elementor Pro WordPress site via EMCP Tools
  (formerly "Elementor MCP") — recognizable by `mcp__*__emcp-tools-*` tools (plugin v3+) or legacy
  `mcp__*__elementor-mcp-*` tools. Teaches fluent patterns for building pages with native Elementor
  widgets via the catalog flow (`list-widgets` → `get-widget-schema` → `add-free-widget`), schema-first
  styling, Global Kit token strategies (`__globals__`), container layout traps (`flex_gap` vs `gap`,
  `padding` vs `_padding`, flex boxed collapse), RTL & Persian alignments, and the agent visual-review loop.
---

# Elementor MCP — Fluent Live Page Building

Most pain when building Elementor pages via MCP comes from two mistakes: dropping into HTML widgets the moment something doesn't style as expected, and guessing setting keys — Elementor silently stores any key you write and silently ignores the wrong ones. The patterns below let you build pages that are visually faithful, responsive, and remain fully editable from the Elementor visual panel UI.

---

## Plugin Versions & Tool Gates (EMCP Tools v3+)

Tool names below use the v3 prefix (`emcp-tools-<tool>`):
- **`mcp__*__emcp-tools-*`** → plugin v3.0+ (standard). Confirm Elementor is active with `wp plugin list`.
- **`mcp__*__elementor-mcp-*`** → legacy plugin (pre-3.0). Recommend updating to current.

### The v3 Tool Gate
v3.1 disables 39 abilities out of the box (stored in `emcp_tools_disabled_tools` wp option) — including six commonly needed:
- `add-custom-css`
- `set-dynamic-tag`
- `list-dynamic-tags`
- `add-pro-widget`
- `create-theme-template`
- `set-template-conditions`

To enable them via WP-CLI:
```bash
wp eval '
  $d = get_option("emcp_tools_disabled_tools", []);
  $enable = ["emcp-tools/add-custom-css","emcp-tools/set-dynamic-tag","emcp-tools/list-dynamic-tags",
             "emcp-tools/add-pro-widget","emcp-tools/create-theme-template","emcp-tools/set-template-conditions"];
  update_option("emcp_tools_disabled_tools", array_values(array_diff($d, $enable)));
'
```

---

## Core Workflow

1. **Read Source Artifact / Brief:** PDF, Figma, sketch, brief, or reference URL. Extract exact content, hierarchy, and design tokens.
2. **Get Global Kit:** Call `emcp-tools-get-global-settings` before placing elements. Use kit colors and typography via `__globals__` references so design tokens remain centralized.
3. **Draft First:** Create the page as draft via `emcp-tools-create-page` with `status: "draft"`.
4. **Build Incrementally:** Top-level container → children → next section. Always set `_title` on top-level section containers (e.g. `"_title": "Hero Section"`).
5. **Native Widget Catalog Flow:**
   - `list-widgets`: discover available widgets.
   - `get-widget-schema`: inspect valid parameters and style controls.
   - `add-free-widget` / `add-pro-widget`: insert widget with content and styling in one call.
6. **Iterate & Polish:** Use `update-element` for partial merges.
7. **Visual Review & Verify:** Check responsiveness and rendering on desktop, tablet, and mobile.

---

## Critical Layout & Setting Traps

### 1. Container Gap: Use `flex_gap`, NOT `gap`
When setting gap between flex children, Elementor's CSS generator reads `flex_gap`:
```json
// CORRECT:
{
  "flex_gap": { "column": "0", "row": "24", "unit": "px", "sizes": [] }
}
```
*`gap` is silently ignored by the CSS generator!*

### 2. Container Padding: Use `padding`, NOT `_padding`
Container padding generates `--padding-top` etc. CSS vars from `padding`:
```json
// CORRECT:
{
  "padding": { "top": "80", "right": "24", "bottom": "80", "left": "24", "unit": "px", "isLinked": false }
}
```
*`_padding` is silently ignored on containers!*

### 3. Flex Children Width: The Boxed Collapse Trap
If a 3-column flex row collapses to a vertical stack:
- **Cause:** Child containers default to `content_width: "boxed"`, which Elementor forces to 100% width.
- **Fix:** Flex children that have explicit widths must set `content_width: "full"`.
```json
{
  "content_width": "full",
  "width": { "unit": "%", "size": 33.33, "sizes": [] }
}
```

### 4. Grid Containers: Always Set Both Axes
When creating a `container_type: "grid"` container, always pass both `grid_columns_grid` AND `grid_rows_grid` in the same call.
- `grid_columns_grid`: string (e.g. `"repeat(3, 1fr)"`).
- `grid_rows_grid`: object (`{"unit": "fr", "size": 1, "sizes": []}`).

---

## Persian & RTL Rules for Live Sites

When building or styling an Elementor page for Persian or Arabic sites:
1. **Typography Alignment:**
   - Pass `"align": "right"` to all headings, text blocks, and buttons.
   - Set `"typography_font_family": "Vazirmatn"` (or the site's registered Persian font) when overriding global kit defaults.
   - Set line-height to at least `1.7em` to prevent clipping Persian diacritics.
2. **Global Kit Sync:**
   - If the site is fresh, read `references/site-settings.md` to configure the Persian font stack in Site Settings first, rather than redefining fonts on every individual widget.
3. **Directional Flow:**
   - In `flex_direction: "row"`, the first child naturally renders on the right in RTL mode. Position your primary visual elements accordingly.

---

## References Directory
Detailed documentation on advanced capabilities:
- [`references/site-settings.md`](references/site-settings.md) — Configuring Global Kit colors and typography.
- [`references/kit-css.md`](references/kit-css.md) — Baseline site CSS.
- [`references/layout-patterns.md`](references/layout-patterns.md) — Standard responsive container layouts.
- [`references/dynamic-tags.md`](references/dynamic-tags.md) — Dynamic content integration.
- [`references/diagnostics.md`](references/diagnostics.md) — Troubleshooting broken layouts.
- [`references/onboarding.md`](references/onboarding.md) — Wiring up MCP on a new WordPress site.

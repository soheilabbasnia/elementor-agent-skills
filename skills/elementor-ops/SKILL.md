---
name: elementor-ops
description: >-
  Manage, edit, and troubleshoot Elementor pages and templates on existing WordPress sites via WP-CLI
  and browser automation (Playwright/Chrome MCP). Use for safe database text/image replacements in
  _elementor_data, flushing Elementor CSS cache, duplicating pages/templates via CLI, and visual editing
  or screenshot QA when no MCP plugin is installed on the WordPress site.
---

# Elementor Operations (WP-CLI & Browser Automation)

This skill provides operational procedures for editing and managing Elementor websites without requiring an active MCP plugin on WordPress. It uses **WP-CLI** for fast, scriptable, and safe text/URL replacements in the database, and **Browser Automation (Playwright / Chrome MCP)** for structural changes and visual verification.

---

## Decision Matrix: Choosing the Right Tool

| Task | Recommended Method | Risk Level |
| :--- | :--- | :--- |
| **Text content & copy updates** | WP-CLI search-replace | Low (with backup) |
| **Image URL swaps** | WP-CLI meta update / search-replace | Low (with backup) |
| **Duplicating existing pages/templates** | WP-CLI meta copy | Very Low |
| **Flushing CSS & regeneration** | WP-CLI `wp elementor flush-css` | None |
| **Widget styling & layout tweaks** | Browser Automation (Playwright / Chrome) | None |
| **Adding/removing sections & widgets** | Browser Automation (Playwright / Chrome) | None |
| **Visual QA & Screenshot** | Playwright — cropped WebP, element-only, final step only | None |
| **Functional checks (menu, click, hover)** | Accessibility Tree first — screenshot only if tree is insufficient | None |

> [!TIP]
> **Rule of Thumb:** If changing text, phone numbers, or image URLs within existing widgets, WP-CLI is dramatically faster. For structural or visual styling modifications, use browser automation.

> [!IMPORTANT]
> **Bandwidth Rule:** Never take a full-page screenshot. Always crop to the specific element under test and use WebP format. Screenshots are a last resort — prefer Accessibility Tree for functional checks like hamburger menus, dropdowns, and hover states.

---

## 1. Fast & Safe Text Replacements via WP-CLI

### Step 1: Always Back Up First
Before touching Elementor serialized postmeta, save an exact copy of the current page data:
```bash
wp post meta get <POST_ID> _elementor_data > /tmp/elementor-backup-<POST_ID>.json
```

### Step 2: Dry Run First
Always perform a dry run to verify the number of occurrences:
```bash
wp search-replace "متن قبلی" "متن جدید" wp_postmeta \
  --include-columns=meta_value --dry-run --precise
```

### Step 3: Execute the Replacement
```bash
wp search-replace "متن قبلی" "متن جدید" wp_postmeta \
  --include-columns=meta_value --precise
```

### Step 4: Flush Elementor CSS Cache
Elementor stores pre-generated CSS files based on widget settings. You **must** flush the CSS cache after CLI updates or changes won't be visible:
```bash
wp elementor flush-css
```
*Fallback if Elementor CLI extension is missing:*
```bash
wp option delete _elementor_global_css
wp post meta delete-all _elementor_css
```

---

## 2. Managing & Duplicating Pages via WP-CLI

### Duplicate an Elementor Page
```bash
# 1. Read source data
SOURCE_DATA=$(wp post meta get <SOURCE_ID> _elementor_data)
SOURCE_SETTINGS=$(wp post meta get <SOURCE_ID> _elementor_page_settings)

# 2. Create target page as draft
NEW_ID=$(wp post create --post_type=page --post_title="صفحه کپی‌شده جدید" --post_status=draft --porcelain)

# 3. Apply Elementor meta
wp post meta update $NEW_ID _elementor_data "$SOURCE_DATA"
wp post meta update $NEW_ID _elementor_edit_mode "builder"
wp post meta update $NEW_ID _elementor_page_settings "$SOURCE_SETTINGS"

# 4. Regenerate CSS
wp elementor flush-css
```

---

## 3. Visual QA via Browser Automation

### Screenshot Rules — Bandwidth Optimization

**Always follow these rules. No exceptions.**

- **NEVER** take full-page screenshots
- **ALWAYS** crop to the specific element being tested
- **Format:** WebP only (`--type=webp`) — never PNG or JPEG
- **Max viewport width:** 1280px
- **Take a screenshot ONLY at these moments:**
  1. After the **final** change is applied and verified — not during intermediate steps
  2. When a specific visual bug cannot be confirmed any other way

**Before cropping:** get the element's exact bounding box via JavaScript:
```js
// Run this in the browser console or via playwright evaluate
document.querySelector('header').getBoundingClientRect()
// → { x: 0, y: 0, width: 1280, height: 80 }
// Use those exact values for --clip below
```

### Playwright Screenshot Flow

```bash
# 1. Navigate to the FRONTEND URL — never the Elementor editor — for visual QA
playwright-cli open "https://example.com"

# 2. Screenshot ONLY the target element using its bounding box
#    Example: header area
playwright-cli screenshot \
  --clip="0,0,1280,80" \
  --type=webp \
  --filename="qa-header.webp"

# 3. For hamburger menu — click the button first, then screenshot only the open menu area
#    Step A: click the hamburger button
playwright-cli click "button.menu-toggle"
#    Step B: screenshot only the expanded menu panel
playwright-cli screenshot \
  --clip="0,0,400,600" \
  --type=webp \
  --filename="qa-menu-open.webp"

# 4. For mobile responsive check — resize viewport, then crop
playwright-cli set-viewport-size "375,812"
playwright-cli screenshot \
  --clip="0,0,375,80" \
  --type=webp \
  --filename="qa-header-mobile.webp"
```

### Functional Checks — Prefer Accessibility Tree Over Screenshots

For interactive elements (menus, dropdowns, buttons, modals), **always try the Accessibility Tree first**. It is text-based and costs near-zero bandwidth:

```bash
# Get the accessibility tree of the page — no image transfer needed
playwright-cli accessibility

# Check if a menu item is visible and focusable after clicking hamburger
# Look for role="menuitem" or role="navigation" in the output
# Only take a screenshot if the tree output is ambiguous or incomplete
```

**When to use Accessibility Tree vs Screenshot:**

| Check | Use Accessibility Tree | Use Screenshot |
| :--- | :---: | :---: |
| Menu item exists and is reachable | ✅ | ❌ |
| Button is enabled / disabled | ✅ | ❌ |
| Modal opens after click | ✅ | ❌ |
| Font, color, spacing looks correct | ❌ | ✅ (cropped) |
| RTL alignment is visually correct | ❌ | ✅ (cropped) |
| Hamburger menu visually renders correctly | ❌ | ✅ (cropped, menu area only) |

### RTL & Persian Visual QA Checklist
When reviewing pages in browser automation:
- Verify headings and paragraphs are aligned right (`text-align: right;`).
- Check that icons and directional indicators point appropriately (e.g. back/forward).
- Confirm no text wrapping collisions or diacritics clipping across mobile viewports (375px) and desktop (1280px).
- For RTL checks, always crop to the specific widget — never full page.

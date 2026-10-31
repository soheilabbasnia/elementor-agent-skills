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
| **Visual QA & Responsive screenshot** | Browser Automation | None |

> [!TIP]
> **Rule of Thumb:** If changing text, phone numbers, or image URLs within existing widgets, WP-CLI is dramatically faster. For structural or visual styling modifications, use browser automation.

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

## 3. Visual Editing & QA via Browser Automation

When structural changes or widget insertions are required on sites without MCP:

### Playwright / Headless Browser Flow
```bash
# 1. Navigate to Elementor Editor
playwright-cli open "https://example.com/wp-admin/post.php?post=<POST_ID>&action=elementor"

# 2. Wait for visual editor canvas and widgets sidebar to finish loading
# 3. Perform edits or verify layouts
# 4. Take full-page visual verification screenshot
playwright-cli screenshot --filename="qa-elementor-verify.png" --full-page
```

### RTL & Persian Visual QA Checklist
When reviewing pages in browser automation:
- Verify headings and paragraphs are aligned right (`text-align: right;`).
- Check that icons and directional indicators point appropriately (e.g. back/forward).
- Confirm no text wrapping collisions or diacritics clipping across mobile viewports (375px) and desktop (1280px).

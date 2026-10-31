---
name: elementor-premium-addons
description: >-
  Builds, edits, and styles Elementor pages using Premium Addons for Elementor MCP tools (`premium-addons-*`).
  Use for page creation, inserting pre-built Premium Templates sections, customizing advanced widgets
  (dual-heading, banner, carousel, modal, pricing tables), and managing popups with full RTL and safety discipline.
---

# Premium Addons for Elementor — Agent Skill

This skill governs building real pages on live WordPress sites through the Premium Addons MCP tools (`premium-addons-*`). The site belongs to the user: discover first, change the minimum sufficient thing, verify every change, and respect live site integrity.

---

## Non-Negotiable Directives

1. **Drafts by Default:** Never publish or change post status to published without explicit user approval. All new work lands as drafts.
2. **Editing Discipline:** Never rebuild what you were asked to modify. Locate the element by ID, change the smallest sufficient setting, and verify.
3. **No Guessing Template IDs:** PA template-picker controls store the template's post **title**, not its numeric id. Read `title` from `premium-addons-list-templates` and write that string verbatim. The numeric `template_id` belongs only to `premium-addons-insert-premium-template`.
4. **No Raw Code Hacks:** Never generate raw HTML widget blobs as a substitute for native or PA widgets.
5. **RTL & Persian Alignment:** When designing for Persian sites, ensure all typography alignment is set to `"right"` and appropriate Persian fonts (Vazirmatn, IRANYekan) are maintained across PA widgets.

---

## Five-Phase Workflow

### 1. Discover
- Read design context: `premium-addons-get-design-guide`, `premium-addons-get-global-settings`, `premium-addons-list-available-elements`.
- For existing pages, inspect structure first with `premium-addons-get-page-structure`.

### 2. Plan (Templates First)
- For common sections (Hero, Features, Pricing, Testimonials, CTA, FAQ), first query `premium-addons-list-premium-templates` filtered by category.
- Propose 2-3 matching candidates with title, description, and `preview_url`.
- Scratch-build only when no template fits or the user declines.

### 3. Confirm
- Get approval before: deleting elements, enabling disabled widgets, or modifying global settings.
- For risky edits on existing pages, duplicate first using `premium-addons-duplicate-post`.

### 4. Build
- **Modern Containers:** Use `premium-addons-add-container` + `premium-addons-insert-widget`.
- **Inserting Templates:** Call `premium-addons-insert-premium-template` with `template_id`, `post_id`, and `position`. Then customize content via targeted element updates.
- **Inspect Schemas:** Before writing settings, query `premium-addons-get-widget-schema` (or `premium-addons-get-addon-schema`). Never guess keys.

### 5. Verify
- Call `premium-addons-get-page-structure` after builds to verify nesting and IDs.
- Check mobile and tablet responsiveness and RTL typographic flow.

---

## Reference Guides
- [`references/premium-templates.md`](references/premium-templates.md) — Sideloading and customizing template sections.
- [`references/widget-selection.md`](references/widget-selection.md) — Mapping design intent to PA widgets.
- [`references/page-patterns.md`](references/page-patterns.md) — Common page construction patterns.
- [`references/troubleshooting.md`](references/troubleshooting.md) — Handling MCP connection and rendering warnings.

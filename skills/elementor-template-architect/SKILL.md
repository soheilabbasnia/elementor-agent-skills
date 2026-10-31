---
name: elementor-template-architect
description: >-
  Generates production-ready, 100% valid Elementor v4+ JSON templates for any WordPress page design brief.
  Use when designing WordPress landing pages, creating responsive Elementor templates, converting wireframes
  or briefs into editable layouts, generating importable container-based JSON schemas, or creating Persian/RTL
  WordPress pages with modern typography (Vazirmatn, IRANYekan) and flexbox containers.
---

# Elementor v4+ Template Architect (with Native RTL & Persian Support)

This skill commands the generation of fully editable, responsive, and structurally valid Elementor JSON templates compatible with Elementor version 4.0 and higher (as well as modern 3.x sites with Flexbox Containers enabled).

## Core Operational Directives

1. **Native Widget Governance:**
   - Assemble all layouts using core native Elementor widgets (`heading`, `text-editor`, `image`, `button`, `icon-box`, `counter`, `divider`, `icon-list`, `spacer`).
   - The creation of pseudo-layouts using monolithic raw code inside `html` widgets is **strictly forbidden**. Every piece of copy, asset, and hyperlink must remain fully editable within the Elementor visual panel controls.

2. **Container Engine Exclusivity:**
   - Build layouts strictly with `elType: "container"` operating on modern CSS Flexbox and Grid paradigms.
   - The generation of deprecated `section` and `column` nodes is strictly prohibited.

3. **Deterministic ID Topology:**
   - Every container and widget must carry a globally unique 8-character hexadecimal string identifier (e.g., `"7f3a9b1c"`).
   - Duplicate identifiers anywhere within the payload compromise the editor state and will cause the Elementor canvas editor to fail or freeze.

4. **Persian & RTL Native Alignment:**
   - When generating templates for Persian or Arabic content, enforce right-aligned typography (`"align": "right"`) on all heading, text-editor, and button widgets.
   - Set standard Persian typography families when custom typography is specified (e.g. `'Vazirmatn', sans-serif`, `'IRANYekanX', 'IRANYekan', sans-serif`, `'Shabnam', sans-serif`, or `'Dana', sans-serif`).
   - Set appropriate Persian line heights: minimum `1.7` to `1.9` for body text (`text-editor`) to prevent diacritics clipping.
   - In horizontal flex layouts (`flex_direction: "row"`), remember that in RTL environments the first element starts from the right side. Order items logically for an RTL optical reading path.

5. **Responsive Breakdown Strategy:**
   - Define baseline desktop metrics directly in the `settings` object.
   - Inject tablet overrides using the `_tablet` suffix (e.g., `"flex_direction_tablet": "column"`).
   - Inject mobile overrides using the `_mobile` suffix (e.g., `"flex_direction_mobile": "column"`, `"typography_font_size_mobile": {"unit": "px", "size": 24}}`).

6. **Ecosystem Independence:**
   - Restrict widget selection to the core free Elementor engine unless the prompt explicitly mandates Elementor Pro features. Avoid introducing undeclared third-party plugin dependencies.

---

## Universal Design Synthesis Workflow

When prompted to construct any web page or section, follow this sequential execution path:

### Step 1: Information Architecture Breakdown
- Deconstruct the design request into functional sections:
  1. **Hero Section:** Value proposition, bold headline, supporting copy, primary CTA, secondary action, visual hero image/mockup.
  2. **Social Proof / Trust Badges:** Client logos, ratings, partner institutions.
  3. **Feature Matrix / Services:** 3 or 4-column feature grid using `icon-box` or nested cards.
  4. **Key Metrics / Counter:** Numerical stats using `counter` widgets.
  5. **Pricing / Packages or Lead Capture:** Clean cards with clear feature lists and CTA buttons.
  6. **FAQ / Accordion or Testimonials:** Content blocks answering common user concerns.
  7. **Footer / Bottom CTA:** Final call to action.
- Select a cohesive color palette with high contrast (minimum 4.5:1 ratio for text readability).

### Step 2: Container Nesting & Spatial Logic
- **Outer Root Containers (`isInner: false`):**
  - Set `content_width: "boxed"` with standard width (e.g. 1140px or 1200px) or `content_width: "full"`.
  - Apply generous desktop vertical padding (e.g., 80px - 100px top and bottom).
  - Use `flex_direction: "column"` for multi-row content or `flex_direction: "row"` for two-column split heroes.
- **Child / Inner Containers (`isInner: true` or nested `container`):**
  - Group related widgets together to control flex alignments (`flex_justify_content`, `flex_align_items`, `flex_gap`).
  - Set `flex_direction_mobile: "column"` and `flex_direction_tablet: "column"` on any horizontal row container to ensure clean mobile responsiveness.

### Step 3: Native Content Population
- Write realistic, high-quality, professional copy matching the requested business domain (avoid generic `lorem ipsum` where possible).
- For images, inject reliable image URLs (e.g. Unsplash placeholders with appropriate dimensions: `https://images.unsplash.com/...`).

### Step 4: Schema Assembly & Syntax Verification
- Wrap the entire structure inside the root Elementor exportable schema (`type: "page"`, `version: "0.4"`).
- Run the Pre-Flight Checklist before delivering the JSON.

---

## Elementor v4+ JSON Schema Model

### Root Document Model
```json
{
  "title": "Universal Page Template",
  "type": "page",
  "version": "0.4",
  "page_settings": {},
  "content": []
}
```

### 1. Flexbox Container Schema (`elType: "container"`)
```json
{
  "id": "a1b2c3d4",
  "elType": "container",
  "isInner": false,
  "settings": {
    "_title": "Hero Section",
    "content_width": "boxed",
    "flex_direction": "row",
    "flex_direction_tablet": "column",
    "flex_direction_mobile": "column",
    "flex_justify_content": "space-between",
    "flex_align_items": "center",
    "flex_gap": {
      "unit": "px",
      "size": 32,
      "sizes": []
    },
    "padding": {
      "unit": "px",
      "top": "80",
      "right": "24",
      "bottom": "80",
      "left": "24",
      "isLinked": false
    },
    "padding_mobile": {
      "unit": "px",
      "top": "48",
      "right": "16",
      "bottom": "48",
      "left": "16",
      "isLinked": false
    },
    "background_background": "classic",
    "background_color": "#0F172A"
  },
  "elements": []
}
```

### 2. Heading Widget Schema (`widgetType: "heading"`)
```json
{
  "id": "e5f6a7b8",
  "elType": "widget",
  "widgetType": "heading",
  "isInner": false,
  "settings": {
    "title": "راهکاری نوین برای ارتقای کسب‌وکار دیجیتال شما",
    "header_size": "h1",
    "align": "right",
    "title_color": "#FFFFFF",
    "typography_typography": "custom",
    "typography_font_family": "Vazirmatn",
    "typography_font_size": {
      "unit": "px",
      "size": 42,
      "sizes": []
    },
    "typography_font_size_mobile": {
      "unit": "px",
      "size": 28,
      "sizes": []
    },
    "typography_font_weight": "800",
    "typography_line_height": {
      "unit": "em",
      "size": 1.4,
      "sizes": []
    }
  },
  "elements": []
}
```

### 3. Text Editor Widget Schema (`widgetType: "text-editor"`)
```json
{
  "id": "c9d0e1f2",
  "elType": "widget",
  "widgetType": "text-editor",
  "isInner": false,
  "settings": {
    "editor": "<p>با بهره‌گیری از زیرساخت‌های مقیاس‌پذیر و فناوری‌های مدرن، سرعت و بازدهی کسب‌وکار آنلاین خود را چند برابر کنید. مدیریت آسان و طراحی بی‌نقص در اختیار شماست.</p>",
    "align": "right",
    "text_color": "#94A3B8",
    "typography_typography": "custom",
    "typography_font_family": "Vazirmatn",
    "typography_font_size": {
      "unit": "px",
      "size": 16,
      "sizes": []
    },
    "typography_line_height": {
      "unit": "em",
      "size": 1.8,
      "sizes": []
    }
  },
  "elements": []
}
```

### 4. Button Widget Schema (`widgetType: "button"`)
```json
{
  "id": "b3c4d5e6",
  "elType": "widget",
  "widgetType": "button",
  "isInner": false,
  "settings": {
    "text": "شروع رایگان و مشاوره",
    "link": {
      "url": "#cta",
      "is_external": false,
      "nofollow": false
    },
    "align": "right",
    "size": "md",
    "button_text_color": "#0F172A",
    "background_color": "#38BDF8",
    "border_radius": {
      "unit": "px",
      "top": "8",
      "right": "8",
      "bottom": "8",
      "left": "8",
      "isLinked": true
    },
    "typography_typography": "custom",
    "typography_font_family": "Vazirmatn",
    "typography_font_weight": "700"
  },
  "elements": []
}
```

### 5. Image Widget Schema (`widgetType: "image"`)
```json
{
  "id": "d7e8f9a0",
  "elType": "widget",
  "widgetType": "image",
  "isInner": false,
  "settings": {
    "image": {
      "url": "https://images.unsplash.com/photo-1551288049-bebda4e38f71?w=900&auto=format&fit=crop&q=80",
      "id": ""
    },
    "image_size": "full",
    "align": "center"
  },
  "elements": []
}
```

### 6. Icon Box Widget Schema (`widgetType: "icon-box"`)
```json
{
  "id": "f1e2d3c4",
  "elType": "widget",
  "widgetType": "icon-box",
  "isInner": false,
  "settings": {
    "selected_icon": {
      "value": "fas fa-shield-alt",
      "library": "fa-solid"
    },
    "title_text": "امنیت تضمین‌شده داده‌ها",
    "description_text": "تمام ارتباطات با پروتکل‌های پیشرفته رمزنگاری شده و بالاترین سطح پایداری را تجربه خواهید کرد.",
    "position": "top",
    "text_align": "right",
    "title_color": "#FFFFFF",
    "description_color": "#94A3B8"
  },
  "elements": []
}
```

---

## Pre-Flight Quality Verification Checklist

Before emitting or saving any Elementor JSON template, verify:
- [ ] Root object contains `"version": "0.4"` and `"type": "page"`.
- [ ] Structural layout strictly uses `elType: "container"` (no `section` or `column`).
- [ ] Every element across all depths possesses a unique 8-character hexadecimal string ID (`[a-f0-9]{8}`).
- [ ] Responsive properties for tablet (`_tablet`) and mobile (`_mobile`) are present on multi-column rows (`flex_direction_mobile: "column"`).
- [ ] Native widgets are employed for all content items; no raw `html` widget placeholders used for structural elements.
- [ ] RTL typographic alignments (`"align": "right"` and `"text_align": "right"`) are verified for Persian/Arabic content.
- [ ] Valid JSON syntax: no trailing commas, properly escaped quotes, and correctly closed brackets.

---

## WordPress Import Procedures

### Method 1: Web Interface (Saved Templates)
1. In the WordPress Admin, navigate to **Templates > Saved Templates** (قالب‌ها > قالب‌های ذخیره‌شده).
2. Click **Import Templates** (درون‌ریزی قالب‌ها) at the top of the page.
3. Choose the generated `.json` file and click **Import Now**.
4. The template will appear in the library and can be inserted into any page via Elementor's folder icon ("My Templates").

### Method 2: Direct Database Insertion (CLI / PHP)
When injecting directly into `wp_postmeta` for a specific post:
- Key: `_elementor_data`
- Value: JSON string of the `content` array (must be slash-escaped using `wp_slash()` in PHP or passed safely via WP-CLI).
- Set `_elementor_edit_mode` to `"builder"`.
- Flush the CSS cache: `wp elementor flush-css`.

# Persian & RTL Design Guide for Elementor v4+

Designing websites in Persian or Arabic requires strict adherence to Right-to-Left (RTL) visual balance, typography metrics, and logical layout behavior.

## 1. Typography & Web Fonts

Persian typography differs significantly from Latin typography in vertical ascenders/descenders, baseline alignment, and letter connection.

### Recommended Font Families
When generating custom typography configurations in Elementor settings (`typography_font_family`), use clean system-stack fallbacks:
- **Vazirmatn (وزیرمتن - استاندارد وب و مدرن):**
  `'Vazirmatn', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif`
- **IRANYekanX / IRANYekan (ایران‌یکان - شرکتی و استارتاپی):**
  `'IRANYekanX', 'IRANYekan', 'Vazirmatn', sans-serif`
- **Dana (دانا - مدرن و مینیمال):**
  `'Dana', 'Vazirmatn', sans-serif`
- **Shabnam (شبنم - ساده و رسمی):**
  `'Shabnam', 'Vazirmatn', sans-serif`

### Line Heights (ارتفاع خط)
Persian diacritics and dots (اعراب و نقطه‌ها) require more breathing room than English:
- **Headings (عناوین):** `1.3em` to `1.5em` (never below 1.2em).
- **Body Text (پاراگراف‌ها):** `1.7em` to `1.9em` (e.g., `"typography_line_height": {"unit": "em", "size": 1.8}}`). Avoid line-heights below 1.6 as lines will collide.

### Text Alignment
- All text widgets (`heading`, `text-editor`) must set `"align": "right"`.
- For `icon-box`, explicitly set `"text_align": "right"`.
- For centered banners or call-to-action blocks, `"align": "center"` is acceptable if deliberate.

---

## 2. Flexbox Container Flow in RTL

### Horizontal Containers (`flex_direction: "row"`)
In an RTL page environment in WordPress:
- The `row` direction automatically starts from the **Right** and flows towards the **Left**.
- **Child Element #1** appears on the right (ideal for the textual value proposition and headlines).
- **Child Element #2** appears on the left (ideal for the hero image, graphic, or form).
- In mobile breakpoints (`flex_direction_mobile: "column"`), Child Element #1 will stay on top, preserving the correct optical hierarchy.

### Directional Icons & Arrows
- Forward arrows (e.g. `fa-arrow-left` vs `fa-arrow-right`):
  - In LTR, "Next" or "Proceed" points to the right.
  - In Persian RTL, "Next" or "Proceed" points to the **left** (`fas fa-arrow-left`).

---

## 3. Spacing & Padding Metrics
- Always keep lateral padding balanced (`left` and `right` equal) unless creating asymmetrical designs.
- When creating asymmetric spacing, remember:
  - Starting margin/padding = `right`
  - Trailing margin/padding = `left`

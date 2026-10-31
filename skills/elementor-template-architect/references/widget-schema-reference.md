# Native Elementor Widgets Schema Reference

When generating Elementor JSON templates, only use native widgets. Elementor silently ignores invalid setting keys. Below is the curated reference of valid setting keys for core widgets.

## Common Widget Keys
Every widget object must have:
```json
{
  "id": "8-char-hex",
  "elType": "widget",
  "widgetType": "<type>",
  "isInner": false,
  "settings": {},
  "elements": []
}
```

---

## 1. Heading (`heading`)
- `title` (string): Text content.
- `header_size` (string): `"h1"`, `"h2"`, `"h3"`, `"h4"`, `"h5"`, `"h6"`, `"div"`, `"span"`, `"p"`.
- `align` (string): `"left"`, `"center"`, `"right"`, `"justify"`.
- `title_color` (hex string): e.g. `"#0F172A"`.
- `typography_typography` (string): `"custom"`.
- `typography_font_family` (string): Font family name.
- `typography_font_size` (object): `{"unit": "px", "size": 36, "sizes": []}`.
- `typography_font_size_tablet`: Overrides for tablet.
- `typography_font_size_mobile`: Overrides for mobile.
- `typography_font_weight` (string): `"400"`, `"500"`, `"600"`, `"700"`, `"800"`.
- `typography_line_height` (object): `{"unit": "em", "size": 1.4, "sizes": []}`.

---

## 2. Text Editor (`text-editor`)
- `editor` (string): HTML string (e.g. `"<p>Content goes here</p>"`).
- `align` (string): `"left"`, `"center"`, `"right"`, `"justify"`.
- `text_color` (hex string): e.g. `"#64748B"`.
- `typography_typography` (string): `"custom"`.
- `typography_font_size` (object): `{"unit": "px", "size": 16, "sizes": []}`.
- `typography_line_height` (object): `{"unit": "em", "size": 1.8, "sizes": []}`.

---

## 3. Button (`button`)
- `text` (string): Label.
- `link` (object): `{"url": "https://example.com", "is_external": false, "nofollow": false}`.
- `align` (string): `"left"`, `"center"`, `"right"`, `"justify"`.
- `size` (string): `"xs"`, `"sm"`, `"md"`, `"lg"`, `"xl"`.
- `button_text_color` (hex string): Text color.
- `background_color` (hex string): Background fill color.
- `border_radius` (object): `{"unit": "px", "top": "8", "right": "8", "bottom": "8", "left": "8", "isLinked": true}`.
- `selected_icon` (object, optional): `{"value": "fas fa-arrow-left", "library": "fa-solid"}`.
- `icon_align` (string): `"left"` or `"right"`.

---

## 4. Image (`image`)
- `image` (object): `{"url": "https://...", "id": ""}`.
- `image_size` (string): `"thumbnail"`, `"medium"`, `"medium_large"`, `"large"`, `"full"`.
- `align` (string): `"left"`, `"center"`, `"right"`.
- `border_radius` (object): Border radius values.
- `box_shadow_box_shadow_type` (string): `"yes"`.
- `box_shadow_box_shadow` (object): `{"horizontal": 0, "vertical": 10, "blur": 25, "spread": 0, "color": "rgba(0,0,0,0.1)"}`.

---

## 5. Icon Box (`icon-box`)
- `selected_icon` (object): `{"value": "fas fa-bolt", "library": "fa-solid"}`.
- `title_text` (string): Heading.
- `description_text` (string): Subtext.
- `position` (string): `"top"`, `"left"`, `"right"`.
- `text_align` (string): `"left"`, `"center"`, `"right"`.
- `title_color` (hex string).
- `description_color` (hex string).
- `primary_color` (hex string): Icon color.

---

## 6. Counter (`counter`)
- `starting_number` (number): Starting count (usually 0).
- `ending_number` (number): Target number (e.g. 500).
- `prefix` (string, optional): e.g. `"+"` or `"$"`.
- `suffix` (string, optional): e.g. `"k"`, `"%"` or `" کاربر"`.
- `title` (string): Counter label.
- `number_color` (hex string).
- `title_color` (hex string).

---

## 7. Divider (`divider`)
- `style` (string): `"solid"`, `"double"`, `"dotted"`, `"dashed"`.
- `weight` (object): `{"unit": "px", "size": 1, "sizes": []}`.
- `color` (hex string).
- `width` (object): `{"unit": "%", "size": 100, "sizes": []}`.
- `align` (string): `"center"`, `"left"`, `"right"`.

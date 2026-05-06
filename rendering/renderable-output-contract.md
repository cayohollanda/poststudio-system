# Renderable Output Contract

> The contract Claude must conform to when producing renderable output. The worker validates against this contract before rendering.

The full schema lives at [`schemas/renderable-carousel.schema.json`](schemas/renderable-carousel.schema.json) (mirror of [`/schemas/renderable-carousel.schema.json`](../schemas/renderable-carousel.schema.json)).

---

## Top-level required fields

| Field | Purpose |
|---|---|
| `version` | "1.0" |
| `prompt_version` | The PostStudio version (`v2.0.0`) |
| `request_id` | Echo from the original request |
| `brand` | Brand slug |
| `topic` | Carousel topic (one sentence) |
| `carousel.canvas` | `{ width, height, format }` — default 1080×1350 PNG |
| `carousel.render_mode` | `html-css` \| `svg` \| `template` |
| `carousel.slide_count` | matches `slides.length` |
| `carousel.accent_color` | `#RRGGBB` |
| `slides[]` | per-slide payloads |
| `caption` | post caption |
| `cta` | one-line CTA |
| `first_comment` | value-stacked first comment |
| `alternative_hooks[]` | exactly 5 |
| `renderability_validated` | boolean |

---

## Per-slide required fields

| Field | Purpose |
|---|---|
| `slide_number` | integer ≥ 1 |
| `role` | role from the role vocabulary |
| `headline` | the line that goes on the slide |
| `payload` | sub-mode-specific (see below) |
| `metadata.brand_slug` | for downstream tooling |

---

## Payload by sub-mode

### `render_mode: "html-css"`

```json
"payload": {
  "html": "<!doctype html>...complete self-contained HTML...</html>",
  "document_shape": "complete"
}
```

### `render_mode: "svg"`

```json
"payload": {
  "svg": "<svg xmlns=\"http://www.w3.org/2000/svg\" width=\"1080\" height=\"1350\" viewBox=\"0 0 1080 1350\">...</svg>"
}
```

### `render_mode: "template"`

```json
"payload": {
  "template_id": "lumen.cinematic-dark.hook.v1",
  "slots": { "headline": "...", "accent_word": "...", "support_text": "...", ... }
}
```

---

## Universal renderable rules

The output must respect:

1. **No JavaScript** anywhere.
2. **No remote URLs** (only `data:` URIs and `{{asset:<id>}}` placeholders).
3. **No copyrighted logos / fake brand assets.**
4. **Fallback fonts** in every text element's `font-family` chain.
5. **One accent color** per slide, applied selectively.
6. **Editable text** — no rasterized text in `<img>` or inline image.
7. **Canvas dimensions** match `carousel.canvas`.
8. **Safe margins** respected (default 48px, brand override allowed).

---

## Required arrays

- `slides[].length === carousel.slide_count`.
- `alternative_hooks.length === 5`.
- `fonts_required[]` lists every font referenced.
- `assets_required[]` lists every asset ID referenced.

---

## Validation flags

```json
"renderability_validated": true,
"renderability_checks": {
  "schema_valid": "pass",
  "canvas_correct": "pass",
  "no_remote_assets": "pass",
  "no_javascript": "pass",
  "no_invented_logos": "pass",
  "fallback_fonts_present": "pass",
  "single_accent": "pass",
  "headline_overflow_estimate": "pass",
  "no_banned_css_or_svg_features": "pass",
  "metadata_complete": "pass"
}
```

If `renderability_validated: false`, the worker must NOT proceed to render. Return errors immediately.

---

## Errors and warnings

```json
"warnings": [
  "Font 'GT Sectra Display' not in available_fonts; falling back to Tiempos Headline."
],
"missing_inputs": [
  "Slide 4 image asset 'lock-and-key' not in available_assets; using flat brand background."
],
"errors": []
```

If `errors[]` is non-empty → don't render.

---

## Worker-side validation order

1. Parse JSON. If parse fails → `SCHEMA_VIOLATION`.
2. Validate against renderable-carousel schema. If fails → `SCHEMA_VIOLATION`.
3. Check `renderability_validated === true`. If false → `UNSAFE_RENDERABLE`.
4. Sanitize HTML/CSS/SVG. If anything stripped → `UNSAFE_HTML_FEATURE` warning.
5. Resolve asset placeholders. If unresolved → `ASSET_NOT_FOUND` per slide.
6. Resolve font references. If unavailable → `FONT_NOT_AVAILABLE` warning per slide.
7. (Template mode) resolve template_id. If unavailable → `TEMPLATE_NOT_FOUND` per slide.
8. Render.

---

## What the worker should NOT do

- Modify the `payload` after sanitization (audit trail).
- Generate alternative content if a slide fails.
- Cache renders across jobs.
- Persist `renderable_carousel` JSON beyond the job's life.

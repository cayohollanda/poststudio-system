# Renderable Output Template

> The standard JSON shape returned by this module. Conforms to `schemas/renderable-carousel.schema.json`.

---

## Top-level structure

```json
{
  "version": "1.0",
  "prompt_version": "v2.0.0",
  "request_id": "<echoed>",
  "generated_at": "2026-05-06T00:00:00Z",
  "mode": "renderable-creative-worker",
  "brand": "<brand-slug>",
  "topic": "<one-sentence topic>",
  "platform": "linkedin",
  "language": "en",
  "goal": "save",
  "angle": "<from system/content-system.md>",
  "structure": "default",
  "carousel": {
    "canvas": { "width": 1080, "height": 1350, "format": "png" },
    "render_mode": "html-css",
    "accent_color": "#E2A852",
    "slide_count": 8,
    "fonts_required": [
      { "family": "GT Sectra Display", "fallback_chain": ["Tiempos Headline", "Georgia", "serif"] },
      { "family": "Söhne", "fallback_chain": ["Inter", "system-ui", "sans-serif"] },
      { "family": "IBM Plex Mono", "fallback_chain": ["Menlo", "monospace"] }
    ],
    "assets_required": [
      { "asset_id": "fractured-glass-dome-01", "purpose": "slide-1 background", "required": true },
      { "asset_id": "calendar-pages-fragmenting-02", "purpose": "slide-2 background", "required": true }
    ]
  },
  "slides": [
    {
      "slide_number": 1,
      "role": "hook",
      "headline": "Most AI tools quietly destroy your deep work.",
      "support_text": "And the chat is why.",
      "payload": {
        "html": "<!doctype html>...complete self-contained HTML...</html>"
      },
      "metadata": {
        "accent_color": "#E2A852",
        "accent_word": "destroy",
        "background_asset_id": "fractured-glass-dome-01",
        "brand_slug": "lumen",
        "data_attributes": { "data-slide": "1", "data-role": "hook" }
      }
    }
  ],
  "caption": "<full caption text with \\n for line breaks>",
  "cta": "<single CTA line>",
  "first_comment": "<first comment text>",
  "alternative_hooks": ["...", "...", "...", "...", "..."],
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
    "data_attributes_complete": "pass"
  },
  "warnings": [],
  "missing_inputs": [],
  "errors": []
}
```

---

## Sub-mode payload shapes

### `render_mode: "html-css"`

```json
"payload": {
  "html": "<!doctype html>...self-contained HTML document...</html>",
  "document_shape": "complete"
}
```

If `document_shape: "fragment"`:

```json
"payload": {
  "html": "<div class=\"slide\" style=\"width:1080px;height:1350px;...\">...</div>",
  "document_shape": "fragment"
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
  "slots": {
    "headline": "Most AI tools quietly destroy your deep work.",
    "accent_word": "destroy",
    "support_text": "And the chat is why.",
    "image_asset_id": "fractured-glass-dome-01",
    "slide_number_label": "01 / 08"
  }
}
```

---

## Slide metadata block

Always included, regardless of sub-mode. Even when the renderable code already contains the data, the metadata block is the canonical source for analytics, archive, and downstream tooling.

```json
"metadata": {
  "accent_color": "#E2A852",
  "accent_word": "destroy",
  "background_asset_id": "fractured-glass-dome-01",
  "brand_slug": "lumen",
  "visual_mode": "cinematic-dark",
  "data_attributes": { "data-slide": "1", "data-role": "hook" }
}
```

---

## Required vs optional fields

**Required at envelope level:**
- `version`, `prompt_version`, `request_id`, `generated_at`, `mode`
- `brand`, `topic`, `platform`, `language`, `goal`
- `carousel.canvas`, `carousel.render_mode`, `carousel.slide_count`
- `slides[]` with at least 1 slide
- `caption`, `cta`, `first_comment`, `alternative_hooks` (5)
- `renderability_validated`

**Required per slide:**
- `slide_number`, `role`, `headline`
- `payload` (sub-mode-specific)
- `metadata.brand_slug`

**Optional but recommended:**
- `support_text` per slide
- `assets_required[]`, `fonts_required[]`
- `warnings[]`, `missing_inputs[]`

---

## What goes in `warnings[]`

Soft issues that didn't block generation but the consumer should know:

- "Font 'GT Sectra Display' not in available_fonts; falling back to Tiempos Headline."
- "Slide 4 image asset 'lock-and-key' not in available_assets; falling back to flat brand background."
- "Headline on slide 6 is at 88% of estimated max width; consider shortening if rendered output overflows."
- "Brand Pack visual-style.md specifies a logo asset, but it wasn't passed in available_assets; brand mark renders as wordmark text."

---

## What goes in `missing_inputs[]`

Things the writer needs to provide:

- "Slide 7 (proof) needs a real before/after time number."
- "Caption references a customer story; no story is in proof-assets.md."
- "CTA mentions 'lumen.example/three-questions'; URL is illustrative — confirm before publishing."

---

## What goes in `errors[]`

Things that fundamentally couldn't be done. If `errors[]` is non-empty, `renderability_validated` is `false` and the consumer should not attempt to render.

```json
"errors": [
  {
    "code": "UNSAFE_RENDERABLE",
    "message": "Brand Pack visual-style.md requires custom font 'GT Sectra Display' which is not in available_fonts and no acceptable fallback exists.",
    "remediation": "Register the font in the worker's font library, or update brand visual-style to use an available font."
  }
]
```

# Playbook — Template-Driven Rendering Workflow

> The recommended production path. Templates live in the worker; Claude only fills slots.

---

## Why template-driven for production

- Deterministic — same slot values produce identical output.
- Brand consistent — templates enforce visual rules.
- Easy to validate — only slots change.
- Fast to render — no HTML parsing surprises.
- Safer — Claude doesn't generate HTML/CSS.

---

## Template library structure

```
templates/
  lumen/
    cinematic-dark.hook.v1/
      template.html
      schema.json
      preview.png
    cinematic-dark.problem.v1/
      template.html
      schema.json
      preview.png
    cinematic-dark.cta.v1/
      template.html
      schema.json
      preview.png
    ...
```

Each template:

- `template.html` — the renderable HTML/CSS with `{{slot:<name>}}` placeholders.
- `schema.json` — slot definitions (type, length, required, enum).
- `preview.png` — a sample render for documentation.

---

## Per-template schema

```json
{
  "template_id": "lumen.cinematic-dark.hook.v1",
  "role": "hook",
  "brand_slug": "lumen",
  "version": 1,
  "canvas": { "width": 1080, "height": 1350 },
  "slots": {
    "headline": { "type": "string", "max_length": 80, "required": true },
    "accent_word": { "type": "string", "max_length": 20, "required": false },
    "support_text": { "type": "string", "max_length": 50, "required": false },
    "image_asset_id": { "type": "string", "required": false },
    "slide_number_label": { "type": "string", "max_length": 8, "required": true }
  },
  "fonts_required": [
    { "family": "GT Sectra Display", "fallback_chain": ["Tiempos Headline", "Georgia", "serif"] },
    { "family": "Söhne", "fallback_chain": ["Inter", "system-ui", "sans-serif"] }
  ]
}
```

---

## Template HTML

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <style>
    /* ... full style block ... */
  </style>
</head>
<body data-slide="{{slot:slide_number_label}}" data-role="hook" data-brand="lumen">
  <div class="bg" style="background-image: url('{{slot:image_asset_id}}');"></div>
  <div class="num">{{slot:slide_number_label}}</div>
  <div class="frame">
    <h1 class="headline">
      {{#if slot:accent_word}}
        <!-- pseudocode: split headline at accent_word and wrap with .accent -->
        {{slot:headline.before_accent}}<span class="accent">{{slot:accent_word}}</span>{{slot:headline.after_accent}}
      {{else}}
        {{slot:headline}}
      {{/if}}
    </h1>
    {{#if slot:support_text}}
      <p class="support">{{slot:support_text}}</p>
    {{/if}}
  </div>
  <div class="brand">lumen</div>
</body>
</html>
```

(Use the templating engine of your choice: Handlebars, Mustache, EJS, etc. Keep it logic-light.)

---

## Worker pipeline

### 1. Receive render job

The renderable_carousel's slides are template-driven:

```json
"payload": {
  "template_id": "lumen.cinematic-dark.hook.v1",
  "slots": { "headline": "...", ... }
}
```

### 2. Validate template_id

Look up `template_id` in the brand's template library. If not found → `TEMPLATE_NOT_FOUND`.

### 3. Validate slots

Validate slot values against the template's `schema.json`. If any slot violates → `SLOT_VIOLATION`.

### 4. Render

Compile the template with the slot data. Resolve image_asset_id via the asset registry. Render via the underlying engine (Playwright for HTML; resvg for SVG templates).

### 5. Optimize, upload, callback

Same as the freeform pipeline.

---

## Adding a new template

1. Author `template.html` per the brand's `visual-style.md`.
2. Define `schema.json` with explicit slots.
3. Test with sample slot data; produce `preview.png`.
4. Register in the worker's template library.
5. Update the brand's available_templates manifest (so Claude can use it).
6. Deploy.

---

## Versioning templates

Templates are versioned (`v1`, `v2`, ...). Updates:

- Backward-compatible slot additions → minor bump (slots become optional).
- Breaking slot changes → new major version.
- Both versions can coexist; old jobs reference old templates.

The `template_id` includes the version: `lumen.cinematic-dark.hook.v1`.

---

## Brand onboarding to template-driven

1. Run brand for 2-4 weeks in freeform mode.
2. Identify the top 5-7 layouts that worked.
3. Codify them as templates.
4. Register in the worker.
5. Update Claude's `available_templates[]` for that brand.
6. Switch the brand's default `render_mode` to `template`.
7. Keep `html-css` available for one-offs.

See [`integration/playbooks/onboard-new-brand-production.md`](../../integration/playbooks/onboard-new-brand-production.md) Day 3 for the asset/template registration step.

---

## When to fall back to freeform

The CRUD platform may set `render_mode: "html-css"` for:

- One-off creative explorations.
- Special launches that need bespoke layout.
- Posts that don't fit any existing template.

Document the exception in the job metadata so future template extraction can capture the pattern.

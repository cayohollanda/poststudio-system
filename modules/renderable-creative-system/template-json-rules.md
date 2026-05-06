# Template-Driven JSON Sub-mode Rules

> Rules when `render_mode: "template"`. Claude does **not** generate HTML/CSS/SVG. Claude fills slots in templates the worker already maintains.

This sub-mode is the recommended production path. Deterministic, brand-consistent, easy to validate.

---

## How it works

```
┌──────────────────────────────────────────────────────────┐
│ Worker maintains a library of templates per brand:       │
│   - lumen.cinematic-dark.hook.v1                         │
│   - lumen.cinematic-dark.problem.v1                      │
│   - lumen.cinematic-dark.cta.v1                          │
│   - ...                                                  │
│ Each template defines slots: headline, accent_word,      │
│ support_text, image_asset_id, slide_number_label.        │
└────────────────────────┬─────────────────────────────────┘
                         │
                         ▼
┌──────────────────────────────────────────────────────────┐
│ Claude picks a template_id from the request's            │
│ available_templates[] and fills only its slots.          │
└────────────────────────┬─────────────────────────────────┘
                         │
                         ▼
┌──────────────────────────────────────────────────────────┐
│ Worker renders the template with the slot data.          │
│ HTML/CSS lives in the worker, not the prompt.            │
└──────────────────────────────────────────────────────────┘
```

---

## Required input from the request

The request must include `available_templates[]` — a list of templates the worker supports for this brand:

```json
{
  "available_templates": [
    {
      "template_id": "lumen.cinematic-dark.hook.v1",
      "role": "hook",
      "slots": {
        "headline": { "type": "string", "max_length": 80 },
        "accent_word": { "type": "string", "max_length": 20, "required": false },
        "support_text": { "type": "string", "max_length": 50, "required": false },
        "image_asset_id": { "type": "string", "required": false },
        "slide_number_label": { "type": "string", "max_length": 8 }
      }
    },
    {
      "template_id": "lumen.cinematic-dark.problem.v1",
      "role": "problem",
      "slots": { ... }
    }
  ]
}
```

If a request arrives without `available_templates[]`, return:

```json
{
  "errors": [
    {
      "code": "MISSING_INPUT",
      "message": "render_mode 'template' requires available_templates[] in the request.",
      "field": "available_templates"
    }
  ]
}
```

---

## Hard rules

1. **Never invent a template_id.** Use only what's in `available_templates[]`.
2. **Never invent a slot.** Use only the slots defined for the picked template.
3. **Respect every slot's constraints:** `type`, `max_length`, `required`, `enum` if present.
4. **Pick a template that matches the slide's role.** Don't pick `cta` template for slide 1.
5. **One template per slide.** Mixing is not allowed.
6. **No HTML/CSS/SVG in the slot values.** Slot values are plain strings, numbers, or asset IDs.
7. **If no template fits a slide,** surface in `warnings[]` and either pick the closest match (with note) or return a `MODULE_NOT_SUPPORTED` error.

---

## Output shape per slide

```json
{
  "slide_number": 1,
  "role": "hook",
  "payload": {
    "template_id": "lumen.cinematic-dark.hook.v1",
    "slots": {
      "headline": "Most AI tools quietly destroy your deep work.",
      "accent_word": "destroy",
      "support_text": "And the chat is why.",
      "image_asset_id": "fractured-glass-dome-01",
      "slide_number_label": "01 / 08"
    }
  },
  "metadata": {
    "headline": "Most AI tools quietly destroy your deep work.",
    "accent_color": "#E2A852",
    "brand_slug": "lumen"
  }
}
```

The `metadata` block duplicates key fields for downstream tooling (analytics, search, archive). Slot values stay clean.

---

## Slot type discipline

| Type | Rule |
|---|---|
| `string` | escape internal `"`, normalize whitespace |
| `integer` | actual integer, not string |
| `boolean` | `true` / `false`, not string |
| `enum` | exactly one of the listed values |
| `asset_id` | string matching the worker's asset registry; never a URL |
| `color` | hex string `#RRGGBB`; only if the template allows per-slide override |

If you can't satisfy a constraint:

- For optional slots: omit the slot.
- For required slots: surface in `warnings[]` and optionally fill with a safe default if one is documented.

---

## Multi-template strategy across the carousel

A typical 8-slide carousel uses 4-6 templates:

```
Slide 1 → hook template
Slide 2 → problem template
Slide 3 → mistake template (or alt problem template)
Slide 4 → reframe template
Slide 5 → mechanism template
Slide 6 → application template
Slide 7 → example template (or proof template)
Slide 8 → cta template
```

If the available templates don't cover every role, fall back to the closest available template and document the substitution in `warnings[]`.

---

## Why template-driven beats freeform

- **Deterministic.** Same slot values = same render. No CSS surprises.
- **Brand-consistent.** Every carousel for a brand uses the same fonts, margins, accent placement.
- **Cheap to validate.** Worker just checks slots against the template schema.
- **Safe.** Worker controls all HTML/CSS — Claude can't ship unsafe markup.
- **Easy to evolve.** Update the template once; every future carousel inherits the change.

---

## When the request is template-mode but the brand has no templates yet

- Surface in `warnings[]`: "No templates registered for brand X."
- Fall back to `render_mode: "html-css"` if the request allows fallback.
- Otherwise return `MODULE_NOT_SUPPORTED` with remediation: "Register templates for this brand in the worker's template library, or use html-css/svg sub-mode."

---

## Self-check before returning each slide

- [ ] `template_id` is in the request's `available_templates[]`.
- [ ] Every required slot is filled.
- [ ] No invented slots.
- [ ] All slot values respect type and length constraints.
- [ ] Asset IDs match the worker's registry (or surface in `assets_required[]`).
- [ ] Slide role matches the template's role.
- [ ] `metadata` block populated.

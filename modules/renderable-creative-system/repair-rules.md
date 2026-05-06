# Repair Rules

> How Claude fixes broken renderable output, given a worker error report. Loop max 2 iterations before flagging for human review.

---

## When repair is invoked

Repair runs when:

- The worker fails to render one or more slides and returns a structured error.
- The renderability checklist flagged an issue Claude can fix without re-generating content.
- The CRUD platform calls `prompts/16-repair-renderable-output.md` with the broken renderable JSON + the error report.

---

## Inputs to repair

```json
{
  "renderable_carousel": { /* the original output */ },
  "errors": [
    {
      "slide_number": 4,
      "code": "TEXT_OVERFLOW",
      "message": "Headline exceeds canvas at 76px font-size; rendered output cropped 3 chars on the right.",
      "details": { "rendered_width_px": 1108, "canvas_width_px": 1080 }
    },
    {
      "slide_number": 6,
      "code": "FONT_NOT_AVAILABLE",
      "message": "Font 'GT Sectra Display' is not registered in the worker's font library.",
      "details": { "fallback_used": "Times" }
    }
  ]
}
```

---

## Repair strategy by error code

### `TEXT_OVERFLOW`

**Cause:** headline / support text too long for the chosen `font-size` and `max-width`.

**Repair, in order of preference:**

1. **Shorten the copy.** Cut filler words ("the," "actually," "really"). Re-read for rhythm.
2. **Change line break point.** Move `<br>` or `<tspan>` boundary so the longest word doesn't anchor a line.
3. **Reduce `max-width` slightly** if the slide allows (e.g. 880 → 920).
4. **Reduce `font-size` by 4-8px** as a last resort. Don't go below the Brand Pack's minimum.

Never solve overflow by removing safe margins.

### `FONT_NOT_AVAILABLE`

**Cause:** font referenced in CSS / SVG isn't in the worker's font library.

**Repair:**

1. Replace the `font-family` chain with the worker's confirmed available font (or system fallback).
2. Add a `warnings[]` entry explaining the substitution.
3. If the substitution materially changes the brand visual, surface in `warnings[]` and recommend registering the font.

### `ASSET_NOT_FOUND`

**Cause:** `{{asset:<id>}}` placeholder doesn't resolve to a registered asset.

**Repair:**

1. If the asset is optional (background image, decorative element): remove it; fall back to a flat brand-color background.
2. If the asset is required (logo, brand mark): mark `[MISSING_INPUT]` and add to `missing_inputs[]`. Do not invent a substitute.

### `UNSAFE_HTML_FEATURE`

**Cause:** the worker's sanitizer stripped a CSS feature, JS, or HTML element.

**Repair:**

1. Identify the offending feature (the worker's error message names it).
2. Replace with a worker-supported equivalent (e.g. `position: sticky` → `position: absolute` with calculated coords).
3. If no equivalent exists, simplify the layout.

### `INVALID_SVG`

**Cause:** SVG didn't parse, or used unsupported features (`<foreignObject>`, complex filters).

**Repair:**

1. Identify the failing element from the error.
2. Replace `<foreignObject>` content with native SVG equivalents (`<text>`, `<rect>`, etc.).
3. Simplify complex filters to common ones (`feGaussianBlur`, `feDropShadow`).
4. Validate `viewBox`, `width`, `height` are present.

### `TEMPLATE_NOT_FOUND`

**Cause:** the picked `template_id` doesn't exist in the worker's library.

**Repair:**

1. Look at the request's `available_templates[]`. Pick the closest match by role.
2. If no acceptable match, return `MODULE_NOT_SUPPORTED` and recommend html-css fallback.

### `SLOT_VIOLATION`

**Cause:** a slot value violates type / length / enum constraints.

**Repair:**

1. Truncate or rephrase the slot value to fit the constraint.
2. For required slots that can't be filled, surface in `warnings[]` and use a documented safe default if one exists.

### `IMAGE_LOAD_FAILED`

**Cause:** the worker tried to load an asset by ID and got a 404 / network error.

**Repair:**

1. Mark the asset in `assets_required[]` as `status: "failed"`.
2. Fall back to a flat brand-color background for that slide.
3. Surface to consumer in `warnings[]`.

---

## What you cannot repair (and what to do)

Some failures need human or upstream intervention. Don't try to repair them:

- **`SCHEMA_VIOLATION` not caused by your output** — return original error, don't loop.
- **`UNSAFE_CLAIM` flagged by Quality Gate** — the brief itself is broken; surface to consumer.
- **`MISSING_BRAND_PACK`** — same.
- **`AUTHENTICATION_FAILED` to worker** — infrastructure, not your problem.

Return these as-is in `errors[]`.

---

## Repair output discipline

Repair returns the **same shape** as the original generation: a full `renderable-carousel` JSON.

- Preserve the original `request_id`, `brand`, `topic`, `prompt_version`.
- Update `generated_at` to repair time.
- Include a `repair_metadata` block:

```json
"repair_metadata": {
  "iteration": 1,
  "errors_addressed": ["TEXT_OVERFLOW@slide-4", "FONT_NOT_AVAILABLE@slide-6"],
  "changes_summary": [
    "Slide 4: headline shortened from 78 to 64 chars to fit canvas.",
    "Slide 6: font-family changed from 'GT Sectra Display' to 'Tiempos Headline' (worker fallback)."
  ]
}
```

---

## Repair loop limits

- **Iteration 1:** Claude attempts to repair the listed errors.
- **Iteration 2:** if iteration 1's output still fails, attempt one more repair targeting only the residual errors.
- **Iteration 3+:** stop. Return the latest version with `renderability_validated: false` and an `errors[]` block explaining what couldn't be fixed. Flag for human review.

The CRUD platform is responsible for tracking iteration count and not invoking Claude indefinitely.

---

## Self-check after repair

Run the renderability checklist again on the repaired output. If a previously-passing item now fails, you broke something; revert that change.

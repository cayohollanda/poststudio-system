# Renderability Checks

> The 10-point pre-render audit. Runs whenever the output is in renderable form (HTML/CSS / SVG / template-driven JSON). Critical fails block the render.

This is the renderability companion to the content critique. Both passes can run on the same output.

---

## The 10 checks

### Critical (R1-R7) — any fail blocks render

#### R1. Schema valid

The output parses as JSON and conforms to `schemas/renderable-carousel.schema.json`.

**Fail signal:** `JSON.parse` fails, or schema validator reports missing/invalid fields.

#### R2. Canvas correct

`carousel.canvas.width` and `.height` match the request (default 1080×1350). Each slide's renderable body matches the canvas.

**Fail signal:** body dimensions don't match canvas; SVG `viewBox` doesn't match canvas; HTML root has different `width`/`height`.

#### R3. No remote assets

No `http://` or `https://` URLs in `href`, `src`, `url()`, `xlink:href`. Only `data:` URIs and `{{asset:<id>}}` placeholders.

**Fail signal:** any external URL detected. Sanitizer would strip it; render would fail or produce blank elements.

#### R4. No JavaScript

No `<script>` tags. No `on*=` handlers. No `javascript:` URIs. No `eval`, `Function()`, or DOM-manipulation code in any string field.

**Fail signal:** any of the above present.

#### R5. No invented logos

No real-brand wordmarks, icons, or marks that aren't in `available_assets[]`.

**Fail signal:** brand mark text or image references a real company that wasn't provided as an asset.

#### R6. Fallback fonts present

Every text element's `font-family` chain ends in a generic family (`serif`, `sans-serif`, `monospace`).

**Fail signal:** `font-family: "GT Sectra Display"` with no fallback chain.

#### R7. Single accent applied selectively

One accent color per slide, applied to one element max.

**Fail signal:** accent color used on background AND headline AND slide number.

---

### Quality (R8-R10) — fail = flag, dispatch with warnings

#### R8. Headline overflow estimate

The headline character count fits the canvas at the chosen `font-size`.

**Estimate formula:** `chars_per_line ≈ canvas_width / (font_size × 0.5)`. Compare to actual char count after manual line breaks.

**Fail signal:** estimated overflow > 5% of canvas width.

#### R9. No banned CSS / SVG features

For HTML/CSS:
- No `position: sticky`.
- No `backdrop-filter`.
- No viewport units (`vw`, `vh`).
- No `mix-blend-mode` over rasterized backgrounds.
- No `@container`, `@scope`, `@layer`.

For SVG:
- No `<foreignObject>`.
- No CSS animations.
- No filters beyond common ones (`feGaussianBlur`, `feDropShadow`).

**Fail signal:** any of the above present.

#### R10. Metadata complete

Each slide has populated `metadata`:
- `brand_slug`
- `accent_color`
- `role`
- `headline`

**Fail signal:** missing or empty metadata fields.

---

## Running the checks

For each slide:
1. Parse the payload (HTML / SVG / JSON).
2. Search for forbidden tokens (`http://`, `https://`, `<script`, `javascript:`, `on*=`).
3. Count accent-color usages.
4. Estimate headline width.
5. Check fallback font chains.
6. Verify metadata block.

For the carousel envelope:
1. Schema-validate.
2. `slide_count === slides.length`.
3. `assets_required[]` references resolve to known IDs (or surface warnings).
4. `fonts_required[]` references match `available_fonts[]` (or surface warnings).

---

## Output

Set each item to `pass / fail / risk`:

```json
"renderability_checks": {
  "schema_valid": "pass",
  "canvas_correct": "pass",
  "no_remote_assets": "pass",
  "no_javascript": "pass",
  "no_invented_logos": "pass",
  "fallback_fonts_present": "pass",
  "single_accent": "pass",
  "headline_overflow_estimate": "risk",
  "no_banned_css_or_svg_features": "pass",
  "metadata_complete": "pass"
}
```

If any of R1-R7 is `fail`:
- Set `renderability_validated: false`.
- Add `errors[]` entries.
- Block render.
- Return to `prompts/16-repair-renderable-output.md`.

If only R8-R10 fail:
- Set `renderability_validated: true` (or `partial`).
- Add `warnings[]` entries.
- Dispatch render with warnings.

---

## Common failure modes the checks catch

- Claude forgot to escape internal `"` in HTML strings → SCHEMA_VIOLATION.
- Claude referenced a Google Fonts URL → no_remote_assets fail.
- Claude added a `<script>console.log("rendered")</script>` for "debugging" → no_javascript fail.
- Claude generated an Apple/Google logo as wordmark → no_invented_logos fail.
- Claude used `position: sticky` to anchor the slide number → banned_css fail.
- Headline at 76px fills the canvas at 88% width → overflow risk.
- Metadata block missing `accent_color` because the request didn't specify one → metadata fail.

The checks fail loud. Don't silently downgrade the verdict.

# Render Quality Checklist

> Self-checks Claude runs before returning renderable output. Fails block the response (`renderability_validated: false`).

---

## Critical (any fail → don't ship)

| # | Check | What it means |
|---|---|---|
| 1 | Schema valid | The JSON conforms to `schemas/renderable-carousel.schema.json`. |
| 2 | Canvas correct | `carousel.canvas` matches the request (default 1080×1350). |
| 3 | No remote assets | No `http://`, `https://` URLs in `href`, `src`, `url()`, `xlink:href`. |
| 4 | No JavaScript | No `<script>` tags, no `on*=` handlers, no `javascript:` URIs. |
| 5 | No invented logos | No real-brand wordmarks or logos that weren't in `available_assets[]`. |
| 6 | Fallback fonts | Every text element's font-family chain ends in a generic family. |
| 7 | Single accent | One accent color used per slide; never two. |
| 8 | Slide count matches request | `slides.length === carousel.slide_count`. |
| 9 | Mode-appropriate payload | HTML in html-css; SVG in svg; template_id+slots in template. No mixing. |
| 10 | Metadata complete | Each slide has `brand_slug`, `accent_color`, `role`, `headline` in metadata. |

---

## Quality (each fail → flag, not block)

| # | Check |
|---|---|
| 11 | Headline overflow estimate — character-count fits canvas at chosen font-size |
| 12 | Safe margins respected (48px default unless brand overrides) |
| 13 | Box-sizing reset present (HTML/CSS only) |
| 14 | Editable text (no rasterized text in `<img>` / inline base64 image) |
| 15 | Asset IDs match `available_assets[]` (or surfaced in warnings) |
| 16 | Fonts match `available_fonts[]` (or surfaced in warnings) |
| 17 | Z-index hygiene (text always above background image) |
| 18 | No banned CSS features (sticky, backdrop-filter, viewport units, etc.) |
| 19 | Determinism — no random IDs, timestamps, or non-input-derived values |
| 20 | Slide role consistent with template_id (template mode) |

---

## How to run the checklist

For each slide payload:

1. Mentally parse it. Does it look like valid HTML / SVG / template JSON?
2. Search for forbidden tokens: `http://`, `https://` (outside escaped strings), `<script`, `on*="`.
3. Count accent-color usages. Should be ≤ 1 element per slide.
4. Estimate headline width: characters × ~0.5 × font-size. Compare to `max-width`.
5. Confirm metadata block exists and is populated.

For the carousel envelope:

1. Schema fields present?
2. `slide_count` matches `slides.length`?
3. `fonts_required[]` lists every font referenced across all slides?
4. `assets_required[]` lists every asset ID referenced?
5. `caption`, `cta`, `first_comment`, `alternative_hooks` (exactly 5) all present?

---

## Checklist output in the JSON

Set `renderability_checks` field for each item:

```json
"renderability_checks": {
  "schema_valid": "pass",
  "canvas_correct": "pass",
  "no_remote_assets": "pass",
  "no_javascript": "pass",
  "no_invented_logos": "pass",
  "fallback_fonts_present": "pass",
  "single_accent": "pass",
  "slide_count_matches": "pass",
  "mode_appropriate_payload": "pass",
  "metadata_complete": "pass",
  "headline_overflow_estimate": "risk",
  "safe_margins": "pass",
  "box_sizing_reset": "pass",
  "editable_text": "pass",
  "asset_ids_match": "pass",
  "fonts_match": "pass",
  "z_index_hygiene": "pass",
  "no_banned_css": "pass",
  "determinism": "pass",
  "role_consistent_with_template": "pass"
}
```

Set `renderability_validated`:

- `true` if all critical pass and at least 7 of 10 quality items pass.
- `false` otherwise.

---

## When `renderability_validated` is `false`

- Populate `warnings[]` with the items that flagged.
- Populate `errors[]` with the critical fails.
- The consumer (CRUD platform / worker) decides whether to attempt repair via `prompts/16-repair-renderable-output.md`.
- Never ship `renderability_validated: true` when any critical item fails.

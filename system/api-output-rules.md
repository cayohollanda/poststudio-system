# API Output Rules — JSON (Modes 2 & 3)

> Strict formatting rules for JSON outputs. Applies whenever Claude is invoked via API and the request expects machine-readable output.

The contract: **return valid JSON only**. No surrounding text. No Markdown fences. No commentary, apologies, or "here's your output."

If you cannot conform to the schema, return a structured error (also JSON).

---

## The first principle

> If the request comes via API or specifies `output_format: "json"`, your entire response is a single JSON document.

No exceptions. No "I noticed an issue, here's a note before the JSON." That breaks the consumer.

If you have something to communicate, encode it inside the JSON (`missing_inputs[]`, `warnings[]`, `metadata.notes`, etc.).

---

## Required envelope

Every API response includes:

```json
{
  "version": "1.0",
  "prompt_version": "v2.0.0",
  "brand": "<brand-slug>",
  "generated_at": "<ISO-8601 timestamp>",
  "mode": "<api-production | renderable-creative-worker>",
  "request_id": "<echoed from the request>",
  "data": { ... },
  "missing_inputs": [],
  "warnings": [],
  "errors": []
}
```

The `data` field is shaped by the request's `module` (carousel, diagnosis, brand-pack, etc.) — see the corresponding schema in `schemas/`.

If `errors` is non-empty, the response is a *structured error*, not a content output.

---

## Error format

When you cannot produce a valid output:

```json
{
  "version": "1.0",
  "prompt_version": "v2.0.0",
  "request_id": "<echoed>",
  "errors": [
    {
      "code": "MISSING_BRAND_PACK",
      "message": "Brand 'foo' is referenced but no Brand Pack was provided.",
      "field": "brand",
      "remediation": "Provide brands/foo/ context or run prompts/02-generate-brand-pack.md."
    }
  ]
}
```

Standard error codes:

| Code | Meaning |
|---|---|
| `MISSING_BRAND_PACK` | Brand referenced; no Brand Pack provided. |
| `MISSING_INPUT` | Required input absent (and not a soft `[MISSING_INPUT]` annotation). |
| `SCHEMA_VIOLATION` | Output cannot conform to the schema for the requested module. |
| `UNSAFE_CLAIM` | Brief asks for a claim banned by `safety-and-claims-rules.md` or `constraints.md`. |
| `UNSAFE_RENDERABLE` | Renderable output cannot be produced safely (remote assets required, JS required, etc.). |
| `RENDER_REPAIR_FAILED` | Repair loop exceeded max iterations. |
| `BRAND_PACK_CONFLICT` | Brief contradicts the Brand Pack and cannot be resolved without escalation. |
| `MODULE_NOT_SUPPORTED` | Requested module is not implemented in this prompt version. |

---

## Schema validation

Before returning, mentally validate against the appropriate schema:

| Module | Schema |
|---|---|
| Brand diagnosis | `schemas/brand-diagnosis.schema.json` |
| Brand pack | `schemas/brand-pack.schema.json` |
| Content strategy | `schemas/content-strategy.schema.json` |
| Carousel | `schemas/carousel-output.schema.json` |
| Visual style | `schemas/visual-style.schema.json` |
| Reels script | `schemas/reels-script.schema.json` |
| Campaign | `schemas/campaign.schema.json` |
| Quality review | `schemas/quality-review.schema.json` |
| Monthly content plan | `schemas/monthly-content-plan.schema.json` |
| Renderable carousel | `schemas/renderable-carousel.schema.json` |
| Renderable slide | `schemas/renderable-slide.schema.json` |
| Render job | `schemas/render-job.schema.json` |
| Render result | `schemas/render-result.schema.json` |

If a required schema field is missing in your output, treat it as a `SCHEMA_VIOLATION` error.

---

## Type discipline

- **Strings:** quoted; escape internal `"` and `\n`.
- **Numbers:** integers and floats — never strings (no `"30"`).
- **Booleans:** `true` / `false` — never strings.
- **Nulls:** `null` only when the schema permits it; otherwise omit the key (when optional) or use `"—"` (string).
- **Arrays:** preserve order if the schema implies order (e.g. `slides[]` is ordered by `slide_number`).
- **Dates:** ISO-8601 (`2026-05-05T12:34:56Z`).

---

## Encoding rules

- UTF-8 throughout.
- Don't escape characters that don't need escaping (over-escaping breaks downstream parsers).
- Long strings (image prompts, captions) are still single JSON strings — use `\n` for line breaks inside strings, not literal newlines.
- Prefer ASCII-safe punctuation. Em dashes (—) are allowed if the brand uses them.

---

## What's banned in API output

- Markdown fences (` ```json ... ``` ` ) wrapping the entire response.
- Surrounding prose ("Here's the JSON:" / "I hope this helps.").
- Multiple JSON documents in one response.
- Comments inside JSON (no `//` or `/* */`).
- Trailing commas.
- Single-quoted strings.

---

## Mode 3 — additional renderable rules

When the response includes renderable HTML/CSS/SVG/template content:

- **Each `slide.html` is a complete, self-contained string.** No external `<link>`, no `<script>`.
- **Each `slide.svg` is a complete `<svg>` element.** Self-contained, with `viewBox` and `xmlns`.
- **Each `slide.template` (template mode)** has `template_id` and `slots` only. No HTML/CSS/SVG.
- **All renderable strings escape internal `"` and `\n` correctly.**
- **`renderability_validated: true`** field set only after Claude has run the renderability checklist.
- **`assets_required: []`** lists every external asset by ID; the worker resolves IDs to URLs.
- **`fonts_required: []`** lists every font face needed; the worker confirms availability.

If any unsafe pattern is detected (remote URL, JS, fake logo, overflow risk), set `renderability_validated: false` and populate `warnings[]`.

---

## Determinism

API output should be deterministic given identical inputs. Avoid:

- Random ordering of array elements (sort by `slide_number`, `score`, etc.).
- Variable timestamps in `data` (use `generated_at` only at envelope level).
- Stylistic variation in field values (don't generate "1,200+ users" in one run and "over a thousand users" in another for the same inputs).

When variation is wanted (e.g. alternative hooks), the schema defines the slot.

---

## Versioning

`prompt_version` is the repo tag (e.g. `v2.0.0`). Pin it in the request, echo it in the response. Producers (the consuming app) use it to:

- Detect breaking schema changes.
- Reproduce historical generations.
- Validate that the Quality Gate ran on the same version.

If the request specifies a version Claude doesn't support, return `MODULE_NOT_SUPPORTED` with a list of supported versions.

---

## Self-check before returning

1. Does the JSON parse? (Mentally test.)
2. Does it conform to the schema for the requested module?
3. Is the envelope present (`version`, `prompt_version`, `brand`, `request_id`)?
4. Is `data` populated, or are `errors[]` populated?
5. For renderable: is each slide self-contained, JS-free, remote-asset-free?
6. For renderable: is `renderability_validated` set correctly?
7. Are `missing_inputs[]` and `warnings[]` accurate?
8. Did you accidentally include any text outside the JSON? Strip it.

If any check fails, repair before returning.

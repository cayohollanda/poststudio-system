# Rendering Quality Assurance

> Post-render checks. Confirms rendered output meets the brand's visual standard.

The Quality Gate has already validated the renderable JSON before render. This file covers what the worker / CRUD platform checks *after* render.

---

## Per-slide post-render checks

| Check | Method | Pass criteria |
|---|---|---|
| Output dimensions match canvas | image metadata | `width === canvas.width && height === canvas.height` |
| File size in platform limit | image metadata | < 5MB (LinkedIn) / < 30MB (Instagram) |
| No transparent pixels at edges (unless intended) | edge sampling | edge pixels match expected background |
| No overflow at right edge (text overflow) | text-region sampling | bottom-right region not clipped |
| Asset placeholder didn't leak (no `{{asset:` strings visible) | OCR or pixel diff | no template syntax visible |
| File parses as valid PNG/JPG | image library | reads without error |
| Color profile preserved (sRGB) | image metadata | profile present |

---

## Cross-slide checks

| Check | Method | Pass criteria |
|---|---|---|
| Consistent canvas across all slides | image metadata | all slides same dimensions |
| Consistent margins across slides | corner pixel sampling | similar within tolerance |
| Brand mark present on slide 1 + N (if expected) | small-region pixel diff | brand mark detected in expected position |
| Slide numbers in order | OCR on slide-number region (if used) | "01", "02", ... "08" in sequence |

---

## Brand-recognition tests (optional)

For brands with strict visual identity:

- Compare rendered slides against a reference image at low resolution.
- Use perceptual hash (pHash) to detect drift.
- Flag if pHash distance from reference > threshold.

This catches "the rendered output is technically correct but doesn't look like our brand."

---

## Automated detection of common defects

| Defect | Detection |
|---|---|
| Garbage text / typography | OCR + spell check on visible text region |
| Wrong accent color | dominant-color analysis on accent region |
| Faceted text (font fallback used) | flag if `FONT_NOT_AVAILABLE` warning was emitted |
| Distorted image | aspect-ratio analysis on background regions |

---

## Failure verdict

| Defect severity | Action |
|---|---|
| Hard fail (size, dimensions, parse) | Mark slide failed; trigger repair |
| Soft fail (overflow risk on edges, color mild drift) | Render passes; emit `quality_warning` for human review |
| Cosmetic (anti-aliasing edge cases) | Pass; log only |

---

## Human review queue

Some renders pass automated QA but should still go to human review:

- New brand's first 5 carousels.
- Renders flagged with > 2 warnings.
- Renders for compliance-sensitive categories (medical, financial, legal).
- Any render that triggered a repair loop.

The CRUD platform routes these to a human-review UI before marking `delivered`.

---

## Continuous quality monitoring

Track over time:

- Render success rate per brand.
- Repair-loop trigger rate per brand.
- Average pHash distance from brand reference.
- Mean per-slide render time.
- Asset substitution rate.

Drift in any signal indicates upstream problems (Brand Pack changes, prompt drift, font registry gaps).

---

## Regression corpus

Maintain a corpus of known-good renderable carousels per brand. On worker upgrades:

1. Re-render the corpus.
2. Compare to baseline (pHash + pixel diff).
3. Drift > threshold → block deploy; investigate.

This catches headless-browser version drift, font registry changes, and sanitizer behavior changes.

---

## QA-driven Brand Pack updates

Patterns from QA inform Brand Pack refreshes:

- Repeated overflow on slide 6 → `proof slide` template needs shorter copy guidance in `voice.md`.
- Repeated font fallback warnings → register the brand's font properly.
- Repeated color drift → tighten `visual-style.md` palette specs.

QA isn't a one-way flow. Surface insights upstream.

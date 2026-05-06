# Module — Renderable Creative System

> Claude generates HTML/CSS, SVG, or template-driven JSON that a separate rendering worker converts into final PNG/JPG carousel slides.

---

## What it does

Produces **renderable creative output** from a brief and a Brand Pack. Output is structured JSON containing per-slide payloads that a worker (Chromium/Playwright, SVG renderer, or template engine) can render at canvas dimensions (default 1080×1350).

This module is the bridge between Claude's content generation and a real production pipeline that ships finished image assets.

---

## When to use it

- You want **finished PNG/JPG slides**, not just a Markdown spec for a designer.
- You're running an automated content pipeline (Mode 2 / 3).
- You need **deterministic, reproducible** outputs across many brands.
- You're prototyping renderable carousels at MVP stage (freeform HTML/CSS).
- You're scaling a production pipeline (template-driven JSON).

## When NOT to use it

- You're a single human in Mode 1 designing slides in Figma. Skip — use the standard Carousel Machine.
- The post is a one-off requiring custom design work outside any template.
- You don't have a rendering worker yet. (Without it, this output is just JSON.)

---

## Required inputs

- An active **Brand Pack** under `brands/[slug]/` (especially `visual-style.md`).
- A **brief** with topic, audience, platform, language, goal.
- A **render mode**: `html-css` | `svg` | `template`.
- A **canvas spec** (default 1080×1350; configurable).
- For template mode: a list of available `template_id`s the worker supports.
- For HTML/CSS and SVG modes: list of available font faces and asset IDs.

## Optional inputs

- Existing carousel content (Markdown or JSON) to transform into renderable output.
- Specific accent color override.
- Slide count override.
- Reference to a prior renderable carousel for style consistency.

---

## Output

A renderable carousel JSON conforming to `schemas/renderable-carousel.schema.json`. Includes:

- Carousel metadata (brand, topic, canvas, mode).
- Per-slide payload:
  - `freeform-html-css` mode → `html` field (self-contained HTML).
  - `svg` mode → `svg` field (self-contained SVG).
  - `template` mode → `template_id` + `slots`.
- Slide metadata (role, headline, accent_color, slide_number).
- Required fonts and assets.
- Renderability validation flags.
- Caption, CTA, first comment, alt hooks (carried through).

---

## Related prompts

- [`prompts/13-generate-renderable-carousel.md`](../../prompts/13-generate-renderable-carousel.md) — full carousel.
- [`prompts/14-generate-html-css-slide.md`](../../prompts/14-generate-html-css-slide.md) — single HTML/CSS slide.
- [`prompts/15-generate-svg-slide.md`](../../prompts/15-generate-svg-slide.md) — single SVG slide.
- [`prompts/16-repair-renderable-output.md`](../../prompts/16-repair-renderable-output.md) — fix broken render output.
- [`prompts/17-export-render-job.md`](../../prompts/17-export-render-job.md) — convert renderable carousel into a worker job payload.

## Related schemas

- [`schemas/renderable-carousel.schema.json`](../../schemas/renderable-carousel.schema.json)
- [`schemas/renderable-slide.schema.json`](../../schemas/renderable-slide.schema.json)
- [`schemas/render-job.schema.json`](../../schemas/render-job.schema.json)
- [`schemas/render-result.schema.json`](../../schemas/render-result.schema.json)

## Related system files

- [`system/renderable-creative-framework.md`](../../system/renderable-creative-framework.md) — universal rules.
- [`rendering/architecture.md`](../../rendering/architecture.md) — worker architecture.
- [`rendering/safety-and-sanitization.md`](../../rendering/safety-and-sanitization.md) — what the worker enforces.

---

## Mode support

| Runtime mode | Supported? |
|---|---|
| Claude Project Manual | Limited (you can preview HTML/CSS but rendering is manual) |
| API Production | Yes — primary use case |
| Renderable Creative Worker | Yes — primary use case |

---

## Quality rules

Before returning, the module self-checks:

- Schema validity.
- Canvas dimensions correct.
- Safe margins respected.
- No remote assets, no JS, no copyrighted logos.
- Fallback fonts present in every text element.
- One accent color applied selectively.
- No headline overflow at the chosen font size.
- Schema field `renderability_validated: true`.

If any fail, the module attempts repair before returning. If repair fails, it returns a structured error and does not produce broken output.

See [`render-quality-checklist.md`](render-quality-checklist.md) for the full checklist.

---

## Files in this module

- `README.md` — this file
- `instructions.md` — the system prompt for Claude when running this module
- `html-css-rules.md` — HTML/CSS sub-mode rules
- `svg-rules.md` — SVG sub-mode rules
- `template-json-rules.md` — template-driven sub-mode rules
- `renderable-output-template.md` — the standard output structure
- `render-quality-checklist.md` — self-check before returning
- `repair-rules.md` — how to fix broken renderable output

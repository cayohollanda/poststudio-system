# Module — Carousel Machine

> Generates a complete carousel: strategic angle, slide-by-slide copy, visual concepts, image prompts, caption, CTA, first comment, and 5 alternative hooks. The workhorse module.

---

## What it does

Takes a brief + active Brand Pack and returns a production-ready carousel following the standard output contract. Optional handoff to the Renderable Creative System for HTML/CSS/SVG/template-driven rendering.

## When to use it

- ~80% of the time. This is the default for any carousel generation.
- For LinkedIn, Instagram, TikTok carousels, and X (when image carousels are used).

## When NOT to use it

- For short-video scripts → use [`reels-script-machine`](../reels-script-machine/).
- For multi-post sequences → use [`campaign-builder`](../campaign-builder/).
- For renderable HTML/CSS/SVG output → use [`renderable-creative-system`](../renderable-creative-system/).

---

## Required inputs

- Active Brand Pack at `brands/[slug]/`.
- Topic (one sentence).
- Audience (role + situation).
- Platform.
- Language.
- Goal.

## Optional inputs

- Angle (from `system/content-system.md`'s 17 angles, or "auto").
- Structure (from `system/carousel-framework.md`'s 8 structures, or "auto").
- Visual mode (from `system/visual-framework.md`'s 12 modes, or "auto").
- Slide count (7-10, default 8).
- Signal (the strongest evidence material).
- Tone override.
- Reference posts.
- Reuse-from-prior-carousel anchor.

---

## Output

Markdown (Mode 1) or JSON (Mode 2 / 3) following the standard output contract. See [`output-template.md`](output-template.md).

---

## Files in this module

- `README.md` — this file
- `instructions.md` — what Claude reads when running the Carousel Machine
- `carousel-structures.md` — module-level cheat sheet of the 8 structures (extends `system/carousel-framework.md`)
- `output-template.md` — output contract reference
- `quality-checklist.md` — module-specific quality items (extends `system/quality-checklist.md`)

---

## Related

- Prompt: [`prompts/04-generate-carousel.md`](../../prompts/04-generate-carousel.md)
- System: [`system/content-system.md`](../../system/content-system.md), [`system/carousel-framework.md`](../../system/carousel-framework.md), [`system/visual-framework.md`](../../system/visual-framework.md), [`system/copywriting-rules.md`](../../system/copywriting-rules.md)
- Schema: [`schemas/carousel-output.schema.json`](../../schemas/carousel-output.schema.json)
- Hand-off to renderable: [`prompts/13-generate-renderable-carousel.md`](../../prompts/13-generate-renderable-carousel.md)
- Critique: [`prompts/08-critique-output.md`](../../prompts/08-critique-output.md)

## Mode support

| Runtime mode | Supported? |
|---|---|
| Claude Project Manual | Yes |
| API Production | Yes |
| Renderable Creative Worker | Yes (chains into Renderable Creative System) |

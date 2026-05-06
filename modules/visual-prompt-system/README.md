# Module — Visual Prompt System

> 12 generic, reusable visual modes + reusable image-prompt templates. Used to produce cinematic / editorial / surreal / technical / minimalist / premium image prompts for any AI image generator.

---

## What it does

- Picks a visual mode aligned with the brand's emotional center.
- Composes image prompts following the universal prompt grammar.
- Adapts prompts per generator (Midjourney, Flux, ChatGPT Images, Leonardo, Ideogram).
- Keeps text out of generated images by default (text is added later in design tool or by the rendering worker).

## When to use it

- After a Carousel Machine output exists, to refine each slide's image prompt.
- Before a renderable carousel is generated (when image assets are needed for the worker).
- When defining a brand's visual identity (paired with `brand-pack-builder`).

## When NOT to use it

- The brand has visual rules already defined and the carousel output already includes prompts you trust. Skip.

---

## Required inputs

- Active Brand Pack at `brands/[slug]/`, especially `visual-style.md`.
- Either:
  - A carousel output to refine, or
  - A briefing for fresh prompts (subject, mood, mode).
- Target generator: `midjourney | flux | chatgpt-images | leonardo | ideogram | sora-images`.
- Aspect ratio: `4:5 | 1:1 | 16:9 | 9:16`.

## Optional inputs

- Quality bias: `draft | balanced | hero` (hero for slide 1 only).
- Existing reference images.
- Generator-specific flags (Midjourney style raw, etc.).

---

## Output

Per slide:

- Refined image prompt (full sentence, generator-specific syntax).
- Generator settings (aspect ratio, style flags).
- Reserved typography space (where to leave room for headline).
- "Watch out for" — the most likely failure mode and how to iterate.

---

## Files in this module

- `README.md` — this file
- `instructions.md` — the module system prompt
- `visual-modes.md` — the 12 modes (extends `system/visual-framework.md`)
- `image-prompt-templates.md` — reusable prompt fragments
- `composition-rules.md` — placement, headline reservation, consistency rules
- `negative-prompts.md` — the universal negatives + per-mode negatives

---

## Related

- Prompt: [`prompts/05-generate-image-prompts.md`](../../prompts/05-generate-image-prompts.md)
- System: [`system/visual-framework.md`](../../system/visual-framework.md), [`system/renderable-creative-framework.md`](../../system/renderable-creative-framework.md)
- Schema: [`schemas/visual-style.schema.json`](../../schemas/visual-style.schema.json)

## Mode support

| Mode | Supported? |
|---|---|
| Claude Project Manual | Yes — primary use |
| API Production | Yes — pre-render image asset generation |
| Renderable Creative Worker | Indirect — visual mode informs the renderable layout |

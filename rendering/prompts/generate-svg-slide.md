# Prompt — Generate SVG Slide (rendering reference)

> Mirror of [`prompts/15-generate-svg-slide.md`](../../prompts/15-generate-svg-slide.md). Used to generate one slide as a single self-contained `<svg>` element.

---

## When to use

- Vector / editorial typography slides.
- Brand-mark-led layouts.
- Repair of a single broken SVG slide.

## Inputs

- The active Brand Pack.
- The slide's content: role, headline, support text, visual concept, accent.
- Canvas dimensions (typically 1080×1350; viewBox matches).
- Available fonts and assets.

## Output

A single `<svg xmlns="...">...</svg>` element conforming to:

- [`modules/renderable-creative-system/svg-rules.md`](../../modules/renderable-creative-system/svg-rules.md)
- [`rendering/svg-generation-rules.md`](../svg-generation-rules.md)

---

## Self-checks

- Valid SVG (`xmlns`, `width`, `height`, `viewBox`).
- All text wrapped manually via `<tspan>`.
- No `<foreignObject>`, no `<script>`, no `<animate>`.
- No remote URLs.
- Font fallback chain ends in generic.
- Asset references via `{{asset:<id>}}`.
- One accent applied to one element.
- All elements within viewBox.

---

## See

- Canonical prompt: [`prompts/15-generate-svg-slide.md`](../../prompts/15-generate-svg-slide.md)
- Rules: [`modules/renderable-creative-system/svg-rules.md`](../../modules/renderable-creative-system/svg-rules.md)

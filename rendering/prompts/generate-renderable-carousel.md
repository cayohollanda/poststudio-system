# Prompt — Generate Renderable Carousel (rendering reference)

> Mirror / extension of [`prompts/13-generate-renderable-carousel.md`](../../prompts/13-generate-renderable-carousel.md). Same prompt, this folder is the rendering-side reference.

When invoked from a rendering pipeline, this prompt receives:

- The active Brand Pack.
- An existing carousel content (Markdown or JSON) to convert into renderable form.
- Render mode: `html-css` | `svg` | `template`.
- Canvas dimensions.
- Available fonts.
- Available assets.
- (Template mode) Available templates.

It returns:

- A `renderable-carousel.schema.json`-compliant JSON document.
- All slides converted into the requested sub-mode.
- Fonts and assets listed in `fonts_required[]` / `assets_required[]`.
- `renderability_validated: true` if all critical checks pass.

See the canonical prompt at [`prompts/13-generate-renderable-carousel.md`](../../prompts/13-generate-renderable-carousel.md).

---

## Rendering-side considerations

When the worker receives the output of this prompt:

1. Validates against `schemas/renderable-carousel.schema.json`.
2. Sanitizes per `safety-and-sanitization.md`.
3. Resolves placeholders.
4. Renders each slide.

If `renderability_validated: false` or `errors[]` populated, the worker rejects the input without rendering.

---

## When to invoke from the worker

Typically the CRUD platform invokes this prompt, not the worker. The worker is stateless on Claude calls.

However, in repair flows, the worker may delegate to the CRUD platform to invoke `repair-broken-render-output.md` and re-render. The worker itself doesn't call Claude.

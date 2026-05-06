# Prompt — Generate HTML/CSS Slide (rendering reference)

> Mirror of [`prompts/14-generate-html-css-slide.md`](../../prompts/14-generate-html-css-slide.md). Used to generate one slide as self-contained HTML/CSS.

---

## When to use

- Repair a single broken slide without regenerating the whole carousel.
- Generate one-off layouts during MVP.
- Test render compatibility on a specific brand's visual style.

## Inputs

- The active Brand Pack (especially `visual-style.md`).
- The slide's content: role, headline, support text, visual concept, accent.
- Canvas dimensions.
- Available fonts and assets.

## Output

A single `<!doctype html>...</html>` document conforming to:

- [`modules/renderable-creative-system/html-css-rules.md`](../../modules/renderable-creative-system/html-css-rules.md)
- [`rendering/html-css-generation-rules.md`](../html-css-generation-rules.md)
- [`rendering/safety-and-sanitization.md`](../safety-and-sanitization.md) (Claude-side awareness)

The output is just the slide's HTML; the worker wraps / sanitizes / renders.

---

## Self-checks

Before returning, validate:

- Self-contained.
- Canvas dimensions correct.
- Box-sizing reset.
- Real text nodes.
- Font fallback chain ends in generic.
- Asset references via `{{asset:<id>}}`.
- One accent color.
- No JS, no remote URLs, no banned CSS.

If any fail → repair before returning.

---

## See

- Canonical prompt: [`prompts/14-generate-html-css-slide.md`](../../prompts/14-generate-html-css-slide.md)
- Rules: [`modules/renderable-creative-system/html-css-rules.md`](../../modules/renderable-creative-system/html-css-rules.md)

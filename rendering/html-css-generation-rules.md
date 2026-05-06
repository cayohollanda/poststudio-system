# HTML/CSS Generation Rules (Worker-side reference)

> The rules Claude must follow for HTML/CSS sub-mode. The worker enforces these via sanitization. Authoritative rules live in [`modules/renderable-creative-system/html-css-rules.md`](../modules/renderable-creative-system/html-css-rules.md).

This file documents the worker's enforcement perspective: what it accepts, rejects, repairs.

---

## Worker accepts

- Self-contained HTML documents (`<!doctype html>` through `</html>`).
- Inline `<style>` blocks.
- `style=""` attributes.
- Standard semantic HTML tags.
- `box-sizing: border-box` reset.
- Flexbox, Grid, padding, margin, gap.
- Font-family chains ending in generic family.
- `data:` URIs (size-capped).
- `{{asset:<id>}}` placeholders.
- `data-*` attributes on elements.

## Worker rejects (sanitizer strips)

- `<script>` tags.
- `<link>` to external stylesheets.
- `on*=` attributes (`onclick`, `onload`, etc.).
- `javascript:` URIs in `href`.
- Remote URLs (`http://`, `https://`) in `href`, `src`, `url()`, `xlink:href`.
- `<iframe>`, `<embed>`, `<object>`.
- `<form>` (renders fine but signals misuse).
- `<meta http-equiv>` redirects.
- `position: sticky`.
- `backdrop-filter` (configurable; default off).
- `mix-blend-mode` (configurable).
- Viewport units (`vw`, `vh`).
- `@container`, `@scope`, `@layer` (configurable).

If any rejected feature is present → strip + emit `UNSAFE_HTML_FEATURE` warning. If the strip leaves the slide non-renderable → fail with `UNSAFE_HTML_FEATURE` error.

## Worker repairs (auto)

- Unicode normalization (NFC).
- Smart quote → straight quote in headlines (configurable per brand).
- `<br>` → `<br/>` for SVG-compatible HTML.
- Trailing whitespace collapsed.

## Worker fails (no repair)

- Invalid HTML that breaks the parser.
- HTML missing `<body>` or root element with explicit dimensions.
- Headline overflow at chosen `font-size` (worker computes after render → `TEXT_OVERFLOW`).

---

## Canvas dimensions

The HTML root (or the wrapper element if `document_shape: "fragment"`) must declare:

```css
width: <canvas.width>px;
height: <canvas.height>px;
```

If absent, the worker injects them from `carousel.canvas`. If they conflict, the worker uses the renderable_carousel's `canvas` (sanity rule).

---

## Asset placeholders

`{{asset:<id>}}` resolves at render time:

```html
<div style="background-image: url('{{asset:slide-1-bg}}');"></div>
```

Becomes:

```html
<div style="background-image: url('https://cdn.example/lumen/slide-1-bg.png');"></div>
```

If the asset doesn't resolve, the worker:

1. Falls back to a flat brand-color background (using `carousel.canvas` + brand primary).
2. Emits `ASSET_NOT_FOUND` warning per slide.

---

## Font handling

Fonts must be in the worker's font registry. When rendering with Playwright:

- The worker injects `@font-face` rules with embedded base64 fonts (or makes them available locally).
- Headless browser respects `font-display: block` to wait for fonts.
- Fallbacks render if the primary font is unavailable.

If a referenced font isn't in the registry:

- Substitute with the first available font from the chain.
- Emit `FONT_NOT_AVAILABLE` warning.

---

## Headless browser behavior

The worker's renderer:

- Disables JavaScript globally.
- Disables network (or restricts to known asset CDNs).
- Disables plugins, GPU, extensions.
- Sets viewport to canvas dimensions.
- Waits for fonts to load before screenshot.
- Takes a full-canvas screenshot at full resolution.
- Returns PNG bytes for optimization.

---

## Common Claude → worker disagreements

| Claude generates | Worker handles |
|---|---|
| `position: sticky` for slide number | Strips; emits warning |
| `backdrop-filter: blur(10px)` for glass effect | Strips by default; configurable |
| `<link>` to Google Fonts | Strips; falls back to system font; emits warning |
| `<script>console.log()` | Strips |
| `https://images.example/...` direct URL | Strips; replaces with empty src or fallback |
| `vw`/`vh` units | Strips/replaces with px values; logs warning |
| Inline base64 image > 500KB | Accepts but logs size warning |

---

## Self-test for Claude

Before returning, mentally render the HTML:

- Does it have `<!doctype html>` or `<html>`?
- Is the body sized to canvas?
- Is text in real text nodes?
- Are font fallbacks present?
- Is the accent applied to one element only?
- Are all asset references via `{{asset:<id>}}`?
- Is there any `<script>`, `on*=`, or remote URL?
- Does the headline fit at the chosen `font-size`?

If any fail → repair before returning. Don't ship broken HTML.

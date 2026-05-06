# Playbook — SVG → PNG Workflow

> When to choose SVG sub-mode and how to render it.

---

## When to use

- Brands with strong typographic identity (editorial, minimal).
- Layouts dominated by typography over imagery.
- Vector-first brand systems.
- When file size matters and slides are mostly type + simple shapes.

## When NOT to use

- Brands that lean photographic (cinematic-dark with hero photos).
- Layouts with heavy filtering / blending (SVG renderer support varies).
- Multi-language brands where text wrapping is unpredictable (SVG doesn't auto-wrap).

---

## Renderer choice

| Renderer | Pros | Cons |
|---|---|---|
| `resvg` (Rust) | Fast, predictable, no browser dependency | Doesn't support all SVG2 features |
| `librsvg` | Wide platform support | Slower than resvg |
| Headless Chromium loading SVG | Most accurate text rendering | Slower; consumes browser process |

**Recommendation:** start with `resvg` for speed. Fall back to Chromium for SVGs that resvg can't render correctly (test on the regression corpus).

---

## Worker pipeline (resvg path)

```javascript
import { Resvg } from '@resvg/resvg-js';
import sharp from 'sharp';

async function renderSvgSlide({ svg, canvas, fonts }) {
  const sanitized = sanitizeSvg(svg);
  const resolved = resolveAssets(sanitized);

  const resvg = new Resvg(resolved, {
    background: '#0E0F12',  // safe fallback
    fitTo: { mode: 'width', value: canvas.width },
    font: {
      fontFiles: fonts.map(f => f.path),  // file paths to embedded woff2
      loadSystemFonts: false,
      defaultFontFamily: 'Söhne',
    },
  });
  const pngBuffer = resvg.render().asPng();

  return await sharp(pngBuffer)
    .png({ compressionLevel: 9 })
    .toBuffer();
}
```

---

## Worker pipeline (Chromium path)

```javascript
async function renderSvgSlideViaBrowser({ svg, canvas, fonts }) {
  const sanitized = sanitizeSvg(svg);
  const resolved = resolveAssets(sanitized);

  const wrapper = `
    <!doctype html><html><head>
    <style>
      ${buildFontFaceCss(fonts)}
      body { margin: 0; padding: 0; background: #0E0F12; }
      svg { display: block; }
    </style></head>
    <body>${resolved}</body></html>
  `;

  const page = await browser.newPage();
  await page.setViewportSize({ width: canvas.width, height: canvas.height });
  await page.setContent(wrapper, { waitUntil: 'networkidle' });
  await page.evaluate(() => document.fonts.ready);
  const buf = await page.screenshot({ type: 'png' });
  await page.close();
  return buf;
}
```

---

## Sanitization

Per [`svg-generation-rules.md`](../svg-generation-rules.md):

- Strip `<script>`, `<foreignObject>`, `<animate*>`.
- Strip `on*=` attributes.
- Strip remote URLs.
- Validate elements against the allowlist.

---

## Common SVG-specific failures

| Failure | Mitigation |
|---|---|
| Text overflow (no auto-wrap) | Claude must compute lines; worker can detect via post-render bbox check |
| Font missing → renderer falls back to Times | Inject fonts before render; verify with regression corpus |
| Filter renders too heavy / slow | Reduce filter complexity; flag in repair loop |
| `<image>` tag fails to load | Same as HTML/CSS asset handling; fallback to flat color |

---

## Output validation

Same as HTML/CSS path: dimensions, file size, dimensions match canvas.

---

## When mixed-mode helps

You can use SVG sub-mode for slide 1 (vector hero) and HTML/CSS for slides 2-N (mixed content). The renderable carousel can specify per-slide `payload.mode` if the request allows.

This is uncommon and adds complexity. Default to single-sub-mode per carousel unless there's a clear benefit.

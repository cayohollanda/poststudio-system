# SVG Generation Rules (Worker-side reference)

> The rules Claude must follow for SVG sub-mode. Authoritative rules in [`modules/renderable-creative-system/svg-rules.md`](../modules/renderable-creative-system/svg-rules.md).

---

## Worker accepts

- Valid SVG with `xmlns`, `width`, `height`, `viewBox`.
- `<defs><style>` for shared styles.
- Common SVG elements: `<rect>`, `<circle>`, `<path>`, `<line>`, `<polygon>`, `<text>`, `<tspan>`, `<g>`, `<image>`, `<defs>`, `<linearGradient>`, `<radialGradient>`, `<filter>`.
- Filter primitives: `feGaussianBlur`, `feDropShadow`, `feColorMatrix`, `feComposite`, `feMerge`.
- `xlink:href` and `href` (treated equivalently).
- `{{asset:<id>}}` placeholders in `<image href>`.
- Manual line breaks via `<tspan>`.

## Worker rejects (sanitizer strips)

- `<script>` tags.
- `<foreignObject>` (renderer support uneven).
- `on*=` attributes.
- `javascript:` URIs.
- Remote URLs in `href`, `xlink:href`.
- `<animate>`, `<animateMotion>`, `<animateTransform>` (we render static).
- CSS animations inside `<style>`.
- `<use href="...">` to external SVGs.
- Filters beyond the supported list.

## Worker repairs (auto)

- Missing `xmlns="http://www.w3.org/2000/svg"` → injected.
- Missing `viewBox` → derived from width/height.
- `<text>` overflowing the viewBox → flagged (no auto-repair; Claude must shorten / re-break lines).
- Smart quotes in text → straightened (configurable).

## Worker fails (no repair)

- Invalid SVG (parser failure).
- Width/height < canvas dimensions.
- Text outside viewBox (after rendering).

---

## Text rendering

SVG renderers handle text differently than browsers. Specifically:

- No automatic line wrapping. Claude must manually break lines via `<tspan>`.
- Font metrics may differ from browser. Worker tests against renderer's actual glyph metrics; on overflow → `TEXT_OVERFLOW`.
- `text-anchor` controls horizontal alignment (`start`, `middle`, `end`).
- `dominant-baseline` controls vertical anchor.

### Multi-line text pattern

```xml
<text class="headline" x="48" y="1180">
  <tspan x="48">Most AI tools quietly</tspan>
  <tspan x="48" dy="80">destroy your deep work.</tspan>
</text>
```

The `dy` value must equal the chosen line-height in pixels.

---

## Renderer choice

Common SVG-to-PNG renderers:

| Renderer | Pros | Cons |
|---|---|---|
| `resvg` (Rust) | Fast, no browser overhead, predictable | Doesn't support all SVG2 features |
| `librsvg` | Wide platform support | Slower than resvg |
| Headless Chromium loading the SVG in an HTML wrapper | Most accurate text rendering | Slower; consumes browser process |

For production, `resvg` is recommended. For maximum compatibility (especially if Claude generates complex filter chains), use a browser-based path.

---

## Asset references in SVG

```xml
<image href="{{asset:slide-1-bg}}" x="0" y="0" width="1080" height="1350" preserveAspectRatio="xMidYMid slice"/>
```

Worker resolves `{{asset:<id>}}` to a real URL (or a base64 data URI for sandboxed renderers).

If asset doesn't resolve → strip the `<image>` tag; render with the underlying `<rect>` background fallback.

---

## Font handling

The renderer (resvg / browser) must have access to the fonts referenced. Worker injects fonts before rendering:

- For browser-based: `@font-face` in the HTML wrapper.
- For resvg: register font files in the loader before parsing.

Fallbacks specified in the SVG's `font-family` apply if the primary font is unavailable.

---

## Filter performance

Heavy filters (large blur radius, complex composite chains) can dramatically slow rendering. Constrain in Claude:

- `feGaussianBlur stdDeviation`: ≤ 8 typical.
- `feDropShadow`: conservative `stdDeviation` (≤ 6).
- Avoid stacking 3+ filters on one element.

If render time exceeds budget → flag for repair (`SLOW_RENDER` warning).

---

## Common Claude → worker disagreements

| Claude generates | Worker handles |
|---|---|
| `<foreignObject>` containing HTML | Strips; emits warning |
| `<animate>` element | Strips |
| Text without manual `<tspan>` line breaks | Renders single-line; may overflow |
| Custom filter not in support list | Strips filter; renders without |
| `xlink:href="https://..."` | Strips; uses fallback |

---

## Self-test for Claude

Before returning, validate:

- SVG parses (no syntax errors).
- `xmlns`, `width`, `height`, `viewBox` present.
- No `<foreignObject>`, no `<script>`, no `<animate>`.
- No remote URLs.
- All text wrapped manually.
- Font fallbacks present.
- Single accent color.
- All elements within viewBox.

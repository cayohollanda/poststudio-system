# Renderable Creative Framework

> Rules and patterns for producing **renderable** creative output: HTML/CSS, SVG, or template-driven JSON that a worker can convert into PNG/JPG slides at 1080×1350 (default).

This framework is the bridge between Claude's content generation and a rendering worker. Used in Mode 2 (when the consumer wants structured renderable output) and Mode 3 (when a worker physically renders).

---

## Three sub-modes

| Sub-mode | Output | When to use |
|---|---|---|
| **freeform-html-css** | Self-contained HTML/CSS per slide | MVP, creative exploration, one-off polished decks |
| **svg** | Valid SVG per slide | Vector-first layouts, editorial typography, brand mark-heavy slides |
| **template** | Template ID + slot data | Production, brand consistency, multi-brand pipelines, deterministic output |

A single carousel uses one sub-mode for all slides. Don't mix.

---

## Universal rules (apply to all sub-modes)

1. **Canvas:** default 1080×1350 (4:5). Configurable via the request's `canvas` field.
2. **Safe margins:** 48px top, 48px bottom, 48px left, 48px right unless the Brand Pack overrides.
3. **No JavaScript** unless the request explicitly enables it (rare; defaults to disabled).
4. **No remote assets** — fonts, images, scripts must be referenced via:
   - approved system fonts (with fallback chain),
   - assets explicitly provided in the Brand Pack `visual-style.md` and listed in the request's `available_assets[]`,
   - or inline base64 if the worker policy allows it.
5. **No copyrighted logos or fake brand marks** Claude does not invent.
   - **Asset embedding rule (NON-NEGOTIABLE):** when the brand mark, lockup, favicon, or any pre-existing visual asset must appear in a slide, Claude reads the **actual file** from `brands/[slug]/{mark,favicon,lockup,svg}/` and embeds it (as base64 in `<image href="data:...">`, as `<img src="data:...">`, or as a referenced asset_id resolved by the worker). Claude **NEVER** redraws the mark via SVG primitives (`<circle>`, `<path>`, etc.), geometric approximations, or its own interpretation.
   - If the asset file isn't accessible to Claude in the current runtime (e.g. Project Knowledge text-only mode), Claude stops and surfaces `[ASSET_NOT_FOUND]` plus a request to the user to upload the binary file. Claude does not "best-effort redraw."
   - This rule applies in all three modes (Manual, API, Renderable Worker).
6. **Deterministic layout.** Given the same input, the output should render identically.
7. **Editable text in source** — the headline must be a real text node, not rasterized.
8. **No untrusted URLs** in any `href`, `src`, `url()`, or `xlink:href`.
9. **Fallback fonts** specified in every text element's font-family chain.
10. **Slide metadata** preserved alongside renderable code (slide_number, role, headline, accent_color, brand_slug).

---

## Freeform HTML/CSS rules

When the request specifies `render_mode: "html-css"`:

- Each slide is a complete HTML document or a clearly scoped fragment (the request specifies which).
- CSS lives inline (`style=""`), in a `<style>` tag, or in a CSS-in-JS-style string field — never as an external `<link>`.
- The root element has explicit `width: 1080px; height: 1350px;` (configurable).
- Text is set in a text-rendered tag (`<h1>`, `<h2>`, `<p>`), not as image text.
- Use a single accent color from the Brand Pack. Apply selectively (one element per slide).
- All text within safe margins. Test mental model: "would this overflow at the largest plausible language version?"
- Use `box-sizing: border-box` globally.
- Avoid these CSS features (frequently break in headless browsers): `position: sticky`, `mix-blend-mode` on rasterized backgrounds, `backdrop-filter` (confirm worker supports), `@container`, `@scope`, `@layer` (confirm).
- Prefer flexbox / grid for layout. Avoid float-based layouts.
- Use `font-display: block` for embedded fonts (so the renderer waits for them).
- Aspect-locked: do not use viewport units (`vw`, `vh`); use `px` or relative units anchored to the canvas.
- Each slide can include `data-*` attributes carrying slide metadata for the worker to introspect.

### Example slide skeleton

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <style>
    :root { --accent: #E2A852; --bg: #0E0F12; --ink: #F4F1EC; --muted: #A4A8B0; }
    *, *::before, *::after { box-sizing: border-box; }
    html, body { margin: 0; padding: 0; }
    body {
      width: 1080px; height: 1350px;
      background: var(--bg);
      color: var(--ink);
      font-family: "Söhne", "Inter", system-ui, sans-serif;
      position: relative;
      overflow: hidden;
    }
    .frame { padding: 48px; height: 100%; display: flex; flex-direction: column; justify-content: flex-end; }
    .headline {
      font-family: "GT Sectra Display", Georgia, serif;
      font-weight: 500;
      font-size: 76px;
      line-height: 1.05;
      letter-spacing: -0.5px;
      max-width: 880px;
    }
    .accent { color: var(--accent); }
    .support { font-size: 22px; color: var(--muted); margin-top: 16px; }
    .slide-num { position: absolute; top: 48px; right: 48px;
      font-family: "IBM Plex Mono", monospace; font-size: 14px; color: var(--muted); }
    .brand { position: absolute; bottom: 48px; right: 48px;
      font-family: "GT Sectra Display", Georgia, serif; font-size: 14px; color: var(--ink); }
  </style>
</head>
<body data-slide="1" data-role="hook" data-brand="lumen">
  <div class="slide-num">01 / 08</div>
  <div class="frame">
    <h1 class="headline">Most AI tools quietly <span class="accent">destroy</span> your deep work.</h1>
    <p class="support">And the chat is why.</p>
  </div>
  <div class="brand">lumen</div>
</body>
</html>
```

The image visual (e.g. cinematic-dark photograph) is layered separately by the worker — typically composited under the text frame as a background image specified by the slide's `image_asset_url` or generated upstream.

### Common HTML/CSS failure modes

- Text overflows the canvas → cap headline length in copy generation; test wrap.
- Custom fonts don't load → always include a system fallback (`serif`, `sans-serif`, `monospace`).
- Background image not loaded → worker should fail loud; Claude should not assume image is present without `image_asset_url` in metadata.
- Subpixel positioning artifacts → snap key elements to whole pixels.
- Z-index issues with overlay text → use `position: relative` on the frame and `z-index: 2` on text.

---

## SVG rules

When the request specifies `render_mode: "svg"`:

- Output is a single valid `<svg>` document per slide.
- Required attributes: `xmlns="http://www.w3.org/2000/svg"`, `width`, `height`, `viewBox`.
- `viewBox` matches canvas (default `0 0 1080 1350`).
- All text in `<text>` elements with explicit `font-family` chains and `font-size`.
- No `<foreignObject>` (poor renderer support).
- No remote `<image href="https://...">` references — only base64 data URIs or assets injected by the worker via placeholder IDs.
- No `<script>` tags.
- Filters limited to common ones (`feGaussianBlur`, `feDropShadow`); avoid esoteric SVG2 features.
- Text wrapping is manual: SVG doesn't natively wrap. Either compute line breaks in copy generation or use `<tspan>` per line.
- Accent color applied selectively to one element.

### Example slide skeleton

```xml
<svg xmlns="http://www.w3.org/2000/svg" width="1080" height="1350" viewBox="0 0 1080 1350">
  <defs>
    <style>
      .headline { font-family: "GT Sectra Display", Georgia, serif; font-size: 76px; font-weight: 500; fill: #F4F1EC; }
      .accent { fill: #E2A852; }
      .support { font-family: "Söhne", Inter, sans-serif; font-size: 22px; fill: #A4A8B0; }
      .num { font-family: "IBM Plex Mono", monospace; font-size: 14px; fill: #A4A8B0; }
      .brand { font-family: "GT Sectra Display", Georgia, serif; font-size: 14px; fill: #F4F1EC; }
    </style>
  </defs>
  <rect width="1080" height="1350" fill="#0E0F12"/>
  <text class="num" x="1032" y="62" text-anchor="end">01 / 08</text>
  <text class="headline" x="48" y="1180">
    <tspan x="48">Most AI tools quietly</tspan>
    <tspan x="48" dy="80"><tspan class="accent">destroy</tspan> your deep work.</tspan>
  </text>
  <text class="support" x="48" y="1265">And the chat is why.</text>
  <text class="brand" x="1032" y="1296" text-anchor="end">lumen</text>
</svg>
```

### Common SVG failure modes

- Text wrapping not handled → break lines manually with `<tspan>`.
- Font not available in renderer → ensure fallback fonts and accept that exact metrics may shift.
- Drop shadows render heavily → use `feDropShadow` with conservative radius.
- Image fills break with non-square viewBox → always set `viewBox` and `preserveAspectRatio` explicitly.

---

## Template-driven rules

When the request specifies `render_mode: "template"`:

- Claude does **not** generate HTML/CSS/SVG. Claude generates structured JSON conforming to one of the worker's known templates.
- Templates are managed by the worker — Claude only fills slots.
- Each slide must specify:
  - `template_id` (string) — one of the worker's known templates.
  - `slots` (object) — the slot content (`headline`, `support_text`, `accent_word`, `image_asset_url`, `slide_number`, etc.).
  - `metadata` (object) — `role`, `brand_slug`, `accent_color`, etc.
- Claude must respect the template's character limits and slot types (defined upstream — Claude does not invent template names).

### Example slide

```json
{
  "slide_number": 1,
  "template_id": "lumen.cinematic-dark.hook.v1",
  "slots": {
    "headline": "Most AI tools quietly destroy your deep work.",
    "accent_word": "destroy",
    "support_text": "And the chat is why.",
    "slide_number_label": "01 / 08",
    "image_asset_id": "fractured-glass-dome-01"
  },
  "metadata": {
    "role": "hook",
    "brand_slug": "lumen",
    "accent_color": "#E2A852"
  }
}
```

### Why template-driven is preferred for production

- Deterministic — same input, same output, every time.
- Brand consistent — the template enforces visual rules at render time.
- Easy to validate — only schema-defined slots can change.
- Faster to render — no HTML parsing surprises.
- Safer — no Claude-generated CSS to sanitize.

The trade-off: less creative flexibility per slide. The worker must maintain a library of templates.

---

## How Claude decides which sub-mode to use

The request specifies the sub-mode. Claude does not decide.

If the request is ambiguous and Mode 1 manual usage:
- Default to `freeform-html-css` for first iterations.
- Suggest `template` mode once the brand has a stable visual language.

---

## Repair pattern

When a render fails (worker returns errors):
- Use `prompts/16-repair-renderable-output.md`.
- Claude receives the renderable JSON + the worker's error report.
- Claude returns a fixed renderable JSON (same schema, fixed payload).
- Repair loop max 2 iterations before flagging for human review.

Common repairs:
- Headline shortened to fit canvas.
- Font-family chain expanded with fallbacks.
- Remote asset URL replaced with `image_asset_id` placeholder.
- CSS feature replaced with worker-supported equivalent.
- SVG `<text>` lines re-broken into `<tspan>` elements.

---

## Renderability checklist

Before returning, Claude self-checks (Mode 2 / 3):

- [ ] Canvas dimensions present and correct.
- [ ] Safe margins respected (visual estimate + character-count check on headline).
- [ ] No JS, no remote assets, no copyrighted logos.
- [ ] Fallback fonts in every text element.
- [ ] One accent color applied selectively.
- [ ] Slide metadata complete (`role`, `headline`, `slide_number`, `brand_slug`, `accent_color`).
- [ ] Schema-validated against `schemas/renderable-carousel.schema.json`.
- [ ] No JSON syntax errors.

If any check fails, repair before returning. If repair is impossible, return a structured error.

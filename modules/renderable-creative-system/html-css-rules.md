# HTML/CSS Sub-mode Rules

> Specific rules when `render_mode: "html-css"`. Extends [`system/renderable-creative-framework.md`](../../system/renderable-creative-framework.md).

---

## Document shape

Each slide is a complete HTML document **or** a clearly scoped fragment, depending on the request's `document_shape` flag (default: `complete`).

```html
<!doctype html>
<html lang="<language>">
<head>
  <meta charset="utf-8">
  <style>/* scoped styles */</style>
</head>
<body data-slide="<n>" data-role="<role>" data-brand="<slug>">
  <!-- slide content -->
</body>
</html>
```

If `document_shape: "fragment"`: return only the `<body>` content with a wrapping element that has explicit dimensions.

---

## Required structural rules

1. **Canvas:** body (or fragment wrapper) has `width: <canvas.width>px; height: <canvas.height>px;`.
2. **Box-sizing global reset:** `*, *::before, *::after { box-sizing: border-box; }`.
3. **No external stylesheets, scripts, or fonts** referenced by URL.
4. **All CSS inline or in a single `<style>` tag.**
5. **Text in real text nodes** — `<h1>`, `<h2>`, `<p>`, `<span>`. Not `<img>`-rendered text.
6. **Position context:** the wrapper has `position: relative; overflow: hidden;` so absolute children are bounded.
7. **Font-family chain** on every text element ending in a generic family.
8. **Z-index hygiene:** if you stack a background image and text, give the text `position: relative; z-index: 2;`.

---

## CSS feature allowlist

Safe to use:

- Flexbox, Grid.
- `padding`, `margin`, `gap`.
- `font-size`, `font-weight`, `line-height`, `letter-spacing`.
- `color`, `background-color`, `background-image` (linear-gradient or `data:` URI).
- `border`, `border-radius`, `box-shadow`.
- `text-align`, `text-transform`.
- Inline images (`<img>` with `data:` URI or asset ID resolved by worker).
- CSS custom properties (`--var`).
- `position: absolute / relative / fixed`.

Avoid (unless request explicitly enables):

- `position: sticky`.
- `backdrop-filter` (renderer support varies).
- `mix-blend-mode` over rasterized backgrounds.
- `@container`, `@scope`, `@layer`.
- `clip-path` with complex polygons.
- Animations (`@keyframes`, `transition`) — irrelevant for screenshots; can confuse renderers.
- Viewport units (`vw`, `vh`).

---

## Asset references

You may reference assets *only* by ID:

```html
<img src="{{asset:image_asset_id}}" alt="">
```

The worker resolves `{{asset:<id>}}` to a real URL or inline data at render time. Don't hardcode URLs.

If the request didn't supply the needed asset ID, fall back to a flat brand-color background and surface the gap in `warnings[]`.

---

## Font references

```css
font-family: "GT Sectra Display", "Tiempos Headline", Georgia, serif;
```

The worker injects approved fonts. Always include 1-2 web-safe fallbacks and a generic family.

If a font in the brand's `visual-style.md` isn't in the request's `available_fonts[]`, surface it in `warnings[]` and fall back to a system equivalent.

---

## Headline overflow prevention

The most common HTML/CSS render failure is text overflow. Mitigate:

1. Cap headline character count by structure (the brand's tested limit, typically 40-90 chars).
2. Use `max-width` on the headline container (e.g. `880px` for 1080-wide canvas).
3. Use `line-height: 1.05-1.15` for tight cinematic looks.
4. Don't set `white-space: nowrap` on multi-word headlines.
5. If headline is at risk, shorten the copy. Don't shrink the font below the Brand Pack's minimum.

---

## Slide skeleton (production-ready)

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <style>
    :root {
      --bg: #0E0F12;
      --ink: #F4F1EC;
      --muted: #A4A8B0;
      --accent: #E2A852;
      --pad: 48px;
    }
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
    .bg-image {
      position: absolute; inset: 0;
      background-image: url("{{asset:slide-1-bg}}");
      background-size: cover;
      background-position: center;
      z-index: 1;
    }
    .frame {
      position: relative; z-index: 2;
      padding: var(--pad);
      height: 100%;
      display: flex; flex-direction: column; justify-content: flex-end;
    }
    .headline {
      font-family: "GT Sectra Display", Georgia, serif;
      font-weight: 500;
      font-size: 76px;
      line-height: 1.05;
      letter-spacing: -0.5px;
      max-width: 880px;
      margin: 0;
    }
    .accent { color: var(--accent); }
    .support {
      font-size: 22px;
      color: var(--muted);
      margin: 16px 0 0;
    }
    .slide-num {
      position: absolute; top: var(--pad); right: var(--pad);
      font-family: "IBM Plex Mono", monospace;
      font-size: 14px; color: var(--muted);
      z-index: 3;
    }
    .brand {
      position: absolute; bottom: var(--pad); right: var(--pad);
      font-family: "GT Sectra Display", Georgia, serif;
      font-size: 14px; color: var(--ink);
      z-index: 3;
    }
  </style>
</head>
<body data-slide="1" data-role="hook" data-brand="lumen">
  <div class="bg-image"></div>
  <div class="slide-num">01 / 08</div>
  <div class="frame">
    <h1 class="headline">Most AI tools quietly <span class="accent">destroy</span> your deep work.</h1>
    <p class="support">And the chat is why.</p>
  </div>
  <div class="brand">lumen</div>
</body>
</html>
```

---

## Self-check before returning each slide

- [ ] Document is self-contained (no external `<link>` or `<script>`).
- [ ] Canvas dimensions correct.
- [ ] Box-sizing reset present.
- [ ] All text in real text nodes.
- [ ] Font-family chains end in a generic family.
- [ ] Asset references use `{{asset:<id>}}` placeholder.
- [ ] One accent color, applied to one element max.
- [ ] Headline fits at the chosen `font-size` (estimate by character count).
- [ ] No JS, no `position: sticky`, no `backdrop-filter`, no viewport units.
- [ ] `data-slide`, `data-role`, `data-brand` attributes present.

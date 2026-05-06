# SVG Sub-mode Rules

> Rules when `render_mode: "svg"`. Extends [`system/renderable-creative-framework.md`](../../system/renderable-creative-framework.md).

---

## Document shape

Each slide is a complete `<svg>` element:

```xml
<svg xmlns="http://www.w3.org/2000/svg"
     width="1080" height="1350"
     viewBox="0 0 1080 1350"
     preserveAspectRatio="xMidYMid meet">
  <!-- slide content -->
</svg>
```

Required attributes: `xmlns`, `width`, `height`, `viewBox`.

---

## Required structural rules

1. **No `<foreignObject>`.** Renderer support is uneven.
2. **No `<script>` tags.** No JS event handlers.
3. **No remote refs:** `href`, `xlink:href`, `<image href>` must point to `data:` URIs or `{{asset:<id>}}` placeholders.
4. **All text in `<text>` elements** with explicit `font-family` chains and `font-size`.
5. **Manual line breaks** via `<tspan>` — SVG does not auto-wrap.
6. **`<defs><style>` for shared styles** — keep CSS minimal.
7. **Filters limited to common ones:** `feGaussianBlur`, `feDropShadow`, `feColorMatrix`. Avoid unsupported SVG2 features.
8. **No CSS animations or `<animate>` / `<animateMotion>`.** Static screenshots only.

---

## Text wrapping

SVG does not wrap text. You must compute lines yourself.

**Pattern:**

```xml
<text class="headline" x="48" y="1180">
  <tspan x="48">Most AI tools quietly</tspan>
  <tspan x="48" dy="80">destroy your deep work.</tspan>
</text>
```

Rules:
- Each visual line is a `<tspan>` with the same `x` and a `dy` equal to your line-height-in-pixels.
- Estimate line breaks based on character width at the chosen `font-size`. As a rule of thumb, a 76px serif headline fits ~22-26 characters per line in a 1000px-wide region.
- For longer headlines, break on natural phrase boundaries.

---

## Asset references

Same pattern as HTML/CSS:

```xml
<image href="{{asset:slide-1-bg}}" x="0" y="0" width="1080" height="1350" preserveAspectRatio="xMidYMid slice"/>
```

Worker resolves `{{asset:<id>}}` at render time.

---

## Font handling

SVG renderers can be picky about fonts. Practical guidance:

- Always include `font-family` with at least 2 fallbacks ending in a generic family.
- Don't rely on the renderer to embed fonts — assume they're injected by the worker.
- Use `font-weight` numerically (`400`, `500`, `700`).
- Use `letter-spacing` in pixels (e.g. `-0.5`).

---

## Slide skeleton

```xml
<svg xmlns="http://www.w3.org/2000/svg"
     width="1080" height="1350"
     viewBox="0 0 1080 1350"
     preserveAspectRatio="xMidYMid meet">
  <defs>
    <style>
      .bg { fill: #0E0F12; }
      .headline {
        font-family: "GT Sectra Display", Georgia, serif;
        font-size: 76px;
        font-weight: 500;
        fill: #F4F1EC;
      }
      .accent { fill: #E2A852; }
      .support {
        font-family: "Söhne", Inter, sans-serif;
        font-size: 22px;
        fill: #A4A8B0;
      }
      .num {
        font-family: "IBM Plex Mono", monospace;
        font-size: 14px;
        fill: #A4A8B0;
      }
      .brand {
        font-family: "GT Sectra Display", Georgia, serif;
        font-size: 14px;
        fill: #F4F1EC;
      }
    </style>
  </defs>
  <rect class="bg" x="0" y="0" width="1080" height="1350"/>
  <image href="{{asset:slide-1-bg}}" x="0" y="0" width="1080" height="1350" preserveAspectRatio="xMidYMid slice" opacity="0.85"/>
  <text class="num" x="1032" y="62" text-anchor="end">01 / 08</text>
  <text class="headline" x="48" y="1180">
    <tspan x="48">Most AI tools quietly</tspan>
    <tspan x="48" dy="80"><tspan class="accent">destroy</tspan> your deep work.</tspan>
  </text>
  <text class="support" x="48" y="1265">And the chat is why.</text>
  <text class="brand" x="1032" y="1296" text-anchor="end">lumen</text>
</svg>
```

---

## Self-check before returning each slide

- [ ] Valid SVG (parses without errors).
- [ ] `xmlns`, `width`, `height`, `viewBox` all present.
- [ ] No `<script>`, no `<foreignObject>`.
- [ ] No remote URLs.
- [ ] Asset references use `{{asset:<id>}}`.
- [ ] All text wrapped manually via `<tspan>`.
- [ ] Font-family chains end in a generic family.
- [ ] One accent applied to one element.
- [ ] Visual elements within the viewBox.
- [ ] Slide metadata captured in the carousel JSON's `metadata` field (SVG itself doesn't carry it).

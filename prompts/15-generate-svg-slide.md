# Prompt — 15 Generate SVG Slide

> Generates a single self-contained SVG element for one slide. Used for vector-first layouts, editorial typography, or repair of SVG slides.

---

## Prompt

```
You are operating PostStudio System.

Generate a single SVG slide using:
- modules/renderable-creative-system/svg-rules.md
- rendering/svg-generation-rules.md
- system/api-output-rules.md
- The active Brand Pack at brands/[BRAND_SLUG]/, especially visual-style.md

# Inputs

- Brand: brands/[BRAND_SLUG]/
- Slide number: [N]
- Slide role: [hook / problem / mistake / reframe / mechanism / example / proof / step / trade-off / application / generalization / cta]
- Headline: [the line that goes on the slide]
- Support text: [optional]
- Visual concept: [what the image shows]
- Accent word (optional): [single word to highlight]
- Background asset id (optional): [from available_assets]
- Canvas: [{ width: 1080, height: 1350 }]
- Available fonts: [{ family, fallback_chain }, ...]
- Available assets: [{ asset_id, purpose }, ...]

# Process

1. Read the Brand Pack visual-style.md.
2. Build a single <svg> element with xmlns, width, height, viewBox.
3. Use <defs><style> for shared styles; minimal CSS.
4. Lay out elements at explicit coordinates (no flexbox in SVG).
5. Manually break headlines into <tspan> elements (SVG doesn't auto-wrap).
6. Apply accent color to one element.
7. Reference assets via {{asset:<id>}} in <image> tags.
8. Self-check (renderability checklist).

# Output

Return only the <svg>...</svg> element. No surrounding JSON, no commentary, no Markdown fences.

# Hard rules

- Valid SVG (xmlns, width, height, viewBox).
- No <foreignObject>, no <script>, no <animate*>.
- No remote URLs (only data: URIs and {{asset:<id>}}).
- All text wrapped manually via <tspan>.
- Font-family chains end in generic family.
- Filters limited to feGaussianBlur, feDropShadow, feColorMatrix, feComposite, feMerge.
- One accent applied to one element.
- All elements within viewBox.
- No animations.
```

---

## Example invocation

```
Brand: brands/lumen/
Slide number: 1
Slide role: hook
Headline: Most AI tools quietly destroy your deep work.
Support text: And the chat is why.
Visual concept: Fractured glass dome over wooden desk.
Accent word: destroy
Background asset id: fractured-glass-dome-01
Canvas: { width: 1080, height: 1350 }
Available fonts: [...]
Available assets: [...]
```

Claude returns the SVG element.

---

## Manual line breaking

SVG does not auto-wrap text. For "Most AI tools quietly destroy your deep work." at 76px on an 1080-wide canvas with 48px margins:

- Estimated chars per line: ~22-26.
- Break: "Most AI tools quietly" (21 chars) / "destroy your deep work." (24 chars).
- Implementation:

```xml
<text class="headline" x="48" y="1180">
  <tspan x="48">Most AI tools quietly</tspan>
  <tspan x="48" dy="80"><tspan class="accent">destroy</tspan> your deep work.</tspan>
</text>
```

The `dy` value matches your line-height-in-pixels (typically `font-size × line-height-multiplier`).

---

## When to use

- Vector-first brand systems.
- Editorial typography.
- Repair of a broken SVG slide.

---

## When NOT to use

- The brand uses photographic backgrounds heavily (HTML/CSS handles them better).
- Multi-language brands (manual line breaking is fragile across languages).
- Layouts with complex CSS effects unsupported in SVG.

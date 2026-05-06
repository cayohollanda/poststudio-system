# Prompt — 14 Generate HTML/CSS Slide

> Generates a single self-contained HTML/CSS document for one slide. Used for repair, exploration, or per-slide regeneration.

---

## Prompt

```
You are operating PostStudio System.

Generate a single HTML/CSS slide using:
- modules/renderable-creative-system/html-css-rules.md
- rendering/html-css-generation-rules.md
- system/api-output-rules.md
- The active Brand Pack at brands/[BRAND_SLUG]/, especially visual-style.md

# Inputs

- Brand: brands/[BRAND_SLUG]/
- Slide number: [N]
- Slide role: [hook / problem / mistake / reframe / mechanism / example / proof / step / trade-off / application / generalization / cta]
- Headline: [the line that goes on the slide]
- Support text: [optional second line]
- Visual concept: [what the image shows]
- Accent word (optional): [the single word to highlight in accent color]
- Background asset id (optional): [from available_assets]
- Canvas: [{ width: 1080, height: 1350 }]
- Available fonts: [{ family, fallback_chain }, ...]
- Available assets: [{ asset_id, purpose }, ...]
- Document shape: ["complete" | "fragment"]

# Process

1. Read the Brand Pack visual-style.md; lock palette + typography + composition.
2. Compose the slide following modules/renderable-creative-system/html-css-rules.md.
3. Embed the headline as real text (h1 / span / etc.).
4. Apply accent to one element (the accent word if specified).
5. Reference assets via {{asset:<id>}} placeholders.
6. Include data-slide, data-role, data-brand attributes on body.
7. Self-check (renderability checklist).

# Output

Return only the HTML document (or fragment if requested). No surrounding JSON, no commentary, no Markdown fences.

# Hard rules

- Self-contained (no external <link>, <script>).
- Body sized to canvas.
- Box-sizing reset.
- Real text nodes for all copy.
- Font-family chains end in generic family.
- Asset references via {{asset:<id>}}.
- One accent color, one element max.
- No JS, no remote URLs, no banned CSS features.
```

---

## Example invocation

```
Brand: brands/lumen/
Slide number: 1
Slide role: hook
Headline: Most AI tools quietly destroy your deep work.
Support text: And the chat is why.
Visual concept: Fractured glass dome over wooden desk in dusk study, warm rim light.
Accent word: destroy
Background asset id: fractured-glass-dome-01
Canvas: { width: 1080, height: 1350 }
Available fonts: [
  { family: "GT Sectra Display", fallback_chain: ["Tiempos Headline", "Georgia", "serif"] },
  { family: "Söhne", fallback_chain: ["Inter", "system-ui", "sans-serif"] },
  { family: "IBM Plex Mono", fallback_chain: ["Menlo", "monospace"] }
]
Available assets: [
  { asset_id: "fractured-glass-dome-01", purpose: "slide-1 background" }
]
Document shape: complete
```

Claude returns the HTML document.

---

## When to use

- Repair a single broken slide (paired with the worker's error report).
- Test render compatibility on a brand's visual style.
- One-off layouts during MVP.

---

## When NOT to use

- Generating a whole carousel — use `prompts/13-generate-renderable-carousel.md` instead.
- Mode 1 manual workflows — the slide will need a designer to lay out.

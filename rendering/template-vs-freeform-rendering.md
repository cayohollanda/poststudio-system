# Template-Driven vs Freeform Rendering

> The decision guide. Pick template-driven for production. Pick freeform for MVP and creative exploration.

---

## TL;DR

| Need | Pick |
|---|---|
| Stable production output across many brands | template-driven |
| Brand consistency at the slide-template level | template-driven |
| Cheap to validate and audit | template-driven |
| Maximum creative flexibility per slide | freeform HTML/CSS |
| MVP / first month of production | freeform HTML/CSS |
| Vector / editorial typography | SVG |
| One-off polished decks | freeform HTML/CSS |

---

## Freeform HTML/CSS

**Strengths:**
- Maximum flexibility — Claude can compose any layout.
- Fast to start: no template library needed.
- Easy to iterate during MVP.
- Direct creative expression per slide.

**Weaknesses:**
- Less deterministic — same brief can produce different layouts.
- Higher sanitization burden — all HTML/CSS comes from the model.
- Higher render variance — small CSS differences cause visible shifts.
- Harder to enforce brand consistency at the *visual* level.

**When it shines:**
- First month of brand production.
- Founder-first carousels (the founder may want bespoke layouts).
- Cultural moments / meme posts.
- Project announcements / OSS launches.

---

## Template-driven

**Strengths:**
- Deterministic — same input → same output.
- Brand consistent — template enforces visual rules.
- Easy to validate — only slot values change.
- Fast to render — no HTML parsing surprises.
- Safe — worker controls all HTML/CSS.

**Weaknesses:**
- Requires a template library per brand.
- Less creative flexibility per slide.
- Slot constraints (length, type) can feel limiting.
- Adding a new look = new template + worker deploy.

**When it shines:**
- Steady-state production.
- Multi-brand pipelines (templates per brand).
- Legal / compliance-heavy categories (consistent output is auditable).
- High-volume content engines.

---

## SVG

**Strengths:**
- Vector — sharp at any size.
- Excellent for editorial typography and brand mark-led layouts.
- Smaller file sizes for vector content.

**Weaknesses:**
- No automatic text wrapping — Claude must compute lines.
- Renderer support varies (especially for SVG2 features).
- More fragile across renderers than HTML/CSS.

**When it shines:**
- Brands with strong typographic identity.
- Editorial / magazine-style layouts.
- Slide patterns dominated by typography over imagery.

---

## Migration path: freeform → template-driven

Most brands start freeform and migrate to template-driven once the visual language is stable.

```
Month 1: Freeform HTML/CSS for everything.
         Capture which layouts work; identify recurring patterns.

Month 2: Codify the top 5-7 layouts as templates.
         Switch to template-driven for those layouts.
         Keep freeform for one-offs.

Month 3+: Template-driven is default.
         Freeform reserved for explicit creative-exploration jobs.
```

---

## Hybrid

A single carousel can mix sub-modes if the request allows:

- Slide 1 (hero) → freeform HTML/CSS for max flexibility.
- Slides 2-7 → template-driven for consistency.
- Slide 8 (CTA) → template-driven.

Worker supports per-slide sub-mode if `payload.mode` is set per slide. This is uncommon but legal.

---

## Cost considerations

| Mode | Per-slide render cost (compute) | Per-slide validation cost | Per-slide failure rate |
|---|---|---|---|
| Freeform HTML/CSS | medium | medium-high (sanitize) | medium |
| SVG | low-medium | medium | medium |
| Template-driven | low | low | low |

Template-driven is the cheapest to operate at scale.

---

## Quality guarantees

Template-driven renders with the *same* template produce visually-equivalent output:

- Same fonts.
- Same accent placement.
- Same margins.
- Same brand mark.

Freeform renders may drift — Claude uses different `font-size` choices or slightly different `padding`. The brand reads similar but not identical.

For brand-recognition-critical operations (e.g. paid ads, regulated industries), template-driven is the safer pick.

---

## When to introduce a new template

- A recurring layout pattern emerges (3+ uses of similar structure).
- A new brand with distinct visual rules joins.
- A regulatory requirement demands a specific layout.
- A new content type (e.g. data drop) needs a dedicated layout.

Avoid adding templates for one-off needs. Use freeform for those.

---

## Anti-pattern

Don't try to make freeform "as deterministic as templates" by over-constraining the prompt. The cost of creative flexibility is variance; the price of determinism is template scaffolding. Pick one.

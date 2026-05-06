# Visual Style — `[brand-slug]`

> The brand's visual identity for carousels and social posts. Pairs with `system/visual-framework.md` (8 visual modes) for the universal rules.

---

## Visual mode

- **Primary mode:** [one of: cinematic-dark | editorial-minimal | futuristic-interface | surreal-product-metaphor | premium-founder-operator | technical-blueprint | exploded-diagram | meme-but-premium]
- **Secondary mode (optional):** [one of the 8, or "none"]
- **Why this mode fits:** [1-3 sentences explaining the audience emotional center and why this mode aligns]

---

## Color palette

| Role | Hex | When to use |
|---|---|---|
| Primary | `#______` | dominant background or main type color |
| Accent | `#______` | one element per slide max — headline emphasis or rim light |
| Neutral 1 | `#______` | body text, support text |
| Neutral 2 | `#______` | secondary backgrounds |
| Dark base (if applicable) | `#______` | primary background for dark-mode slides |
| Light base (if applicable) | `#______` | primary background for light-mode slides |

**Accent rule:** the accent appears once per slide, never twice. Never as a full-bleed background. Used for: a single edge glow, a highlighted node in a diagram, the slide number, or one word in the headline.

---

## Typography

- **Headline typeface:** [e.g. "Tiempos Headline," "GT Sectra Display," "Söhne Breit," "Inter Display"]
  - Weight: [e.g. Medium / SemiBold / Bold]
  - Case: [sentence | title | upper | lower]
  - Letter-spacing: [e.g. tight / -1% / 0]
- **Body typeface:** [e.g. "Inter," "Söhne," "Tiempos Text"]
  - Weight: [Regular / Medium]
- **Mono typeface (optional):** [e.g. "IBM Plex Mono," "JetBrains Mono"]
  - Used for: [code labels, slide numbers, technical annotations — or "none"]

**Pairing rule:** one headline face + one body face. Don't add a third unless mono is genuinely needed.

---

## Logo / brand mark

- **Type of mark:** [wordmark | symbol | combined]
- **Placement on slide 1:** [top-left small / bottom-right small / hidden]
- **Placement on slides 2-N:** [hidden / bottom-right small / hidden on all]
- **Placement on final slide (CTA):** [bottom-right small / center small]
- **Size:** [12-16px height — small enough to read as identity, not advertising]
- **Color:** [white / accent / brand primary]

---

## Composition rules

- **Subject placement:** [centered / off-center upper-third / rule-of-thirds / cropped close-up]
- **Headline placement:** [bottom-third (default) / top-left / right-side / center]
- **Slide-number style:** [top-right small / hidden]
- **Margins (px from canvas edge):** [top: __, right: __, bottom: __, left: __]
- **Aspect ratios per platform:**
  - LinkedIn: [4:5 (1080×1350)]
  - Instagram: [4:5 or 1:1]
  - TikTok: [4:5]
  - X: [16:9 or 1:1]

---

## Lighting & mood defaults

[2-4 sentences describing the brand's default lighting and mood language. Used as a building block in image prompts.]

Example for a cinematic-dark brand:
> Single warm rim light from the upper left. Deep shadows. Atmospheric haze with light dust motes. Cool ambient fill from below. Mood: calm, considered, slightly nocturnal.

---

## Texture & finish defaults

[1-3 sentences describing the brand's default surface and material vocabulary.]

Example for a surreal-product-metaphor brand:
> Hyperreal materials, museum-quality. Glass with light absorption. Wood with visible grain. Metal with brushed finish. Restrained micro-detail — the subject reads in 0.5 seconds.

---

## Depth & focus defaults

[1-2 sentences on depth-of-field, atmospheric perspective, and focus rules.]

Example:
> Shallow depth of field on slide 1 (subject sharp, environment falls off). Slides 2-N: medium DoF, softer than slide 1. No flat 2D illustrations.

---

## Reusable image-prompt building blocks

> 3-5 modular prompt fragments the brand combines into image prompts. Helps the writer assemble new prompts in 60 seconds without thinking from scratch.

### Block 1 — environment
> [paste 1-3 sentences describing the brand's default environment language]

### Block 2 — lighting
> [paste 1-2 sentences describing the brand's default lighting language]

### Block 3 — mood
> [paste 1 sentence describing the brand's default mood adjectives]

### Block 4 — typography reservation
> "leave the bottom third of the canvas empty for headline"

### Block 5 — negative prompt
> "no text, no logos, no watermark, no real brand names, no flat 2D illustration look, no AI-typical hand anatomy, no oversaturated gradient blobs, no stock-photo composition"

---

## Three example image prompts

> Three different subjects, all unmistakably the same brand visual.

### Example 1 — slide 1 hero
> [paste a full slide-1-quality image prompt that demonstrates the brand's mode]

### Example 2 — proof slide
> [paste a full image prompt for a proof / data slide]

### Example 3 — CTA slide
> [paste a full image prompt for a CTA slide]

---

## Visual do-nots (off-limits)

- [Visual choice 1 that's banned for this brand — e.g. "no people in the photography"]
- [Visual choice 2 — e.g. "no neon glow effects"]
- [Visual choice 3 — e.g. "no 2D flat illustration"]

---

## Visual decisions to revisit

> Items intentionally deferred. Re-examine on the next quarterly Brand Pack refresh.

- [Decision 1]
- [Decision 2]
- [Decision 3]

---

> When this file is filled in, you should be able to brief a designer or render a slide-1 image without re-asking what the brand looks like.

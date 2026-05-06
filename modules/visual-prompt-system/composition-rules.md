# Composition Rules

> Where things go on the canvas, regardless of mode.

---

## The non-negotiables

1. **Headline lives in negative space.** Bottom-third by default. Never on the subject's face or busiest area.
2. **One dominant subject.** The eye lands on one thing.
3. **Strong contrast at the silhouette.** Survives thumbnail size.
4. **Depth.** Foreground / midground / background. Atmospheric falloff.
5. **One accent color, applied selectively.**
6. **Consistent margins across slides.** 48px default unless brand overrides.
7. **Brand mark small** (12-16px), corner placement, not banner.
8. **Slide numbering optional.** If used, top-right, mono, muted gray.

---

## Reserve typography space

State explicitly in the image prompt:

> "leave the bottom-third of the canvas intentionally empty for headline placement"

If headline goes elsewhere:

> "leave the [top-left / top-right / right-side / center] empty for headline"

Image generators respect this when said explicitly.

---

## How to keep composition consistent across a carousel

For a carousel (7-10 slides), composition consistency is the brand. Rules:

- **Same canvas dimensions** on every slide.
- **Same headline placement** across the whole set.
- **Same lighting direction.** If slide 1 is lit from upper-left, all slides are lit from upper-left.
- **Same color grading.** All slides share the same hue / saturation feel.
- **Same depth-of-field language.** All slides shallow, or all medium. Don't mix.
- **Same subject scale.** Don't go from a tight macro on slide 1 to a wide environment on slide 2 unless the brief calls for it.

---

## Slide 1 vs. slides 2-N

Slide 1 is a *poster*. Slides 2-N are *frames*.

Slide 1 may receive:
- More elaborate composition.
- Hero subject (Mode 4 / Mode 9 even if the rest is Mode 1 / 2).
- More atmospheric haze.
- Generous typography space.

Slides 2-N receive:
- Cleaner, more functional compositions.
- Lower visual cost (faster to render, faster to read).
- Same lighting and palette as slide 1.

---

## Headline placement table

| Mode | Default headline placement |
|---|---|
| Cinematic Dark | bottom-third left-aligned |
| Editorial Minimal | bottom-third left-aligned |
| Futuristic Interface | bottom-third or top-left |
| Surreal Product Metaphor | bottom-third anchored in negative space |
| Premium Founder/Operator | right two-thirds, anchored bottom-third |
| Technical Blueprint | top or bottom — never inside the diagram |
| Exploded Diagram | bottom-third |
| Meme-but-Premium | bottom-third or top — match the meme structure |
| Luxury Object | bottom-third, sentence-case serif |
| Abstract System | bottom-third or center on neutral backdrop |
| AI Command Center | bottom-third, left-aligned |
| Before/After Split Scene | bottom-center anchored over the split |

---

## Avoiding AI image artifacts

Bake these into prompts:

- "no AI-typical hand anatomy" — or keep hands out of frame.
- "no garbled text" + "no readable text" — generators put garbage characters on rendered surfaces.
- "no awkward face anatomy" — or shoot back-of-head, silhouette, or no faces.
- "no inconsistent perspective on architecture" — for architectural metaphors.
- "no random typography overlays" — the worker / designer adds typography after.

---

## Per-slide consistency checklist

Before finalizing a carousel's image prompts, confirm:

- [ ] Same canvas dimensions specified on every slide.
- [ ] Same lighting language on every slide.
- [ ] Same headline placement across the set.
- [ ] Same color grading hint on every slide.
- [ ] Same negative prompt list on every slide.
- [ ] Slide 1 is the visual hero; slides 2-N sustain.
- [ ] Brand mark and slide-number style declared once and applied consistently.

If any item drifts across the carousel, the audience feels something off without naming it. Fix it.

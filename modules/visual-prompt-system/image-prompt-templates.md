# Image Prompt Templates

> Reusable, modular prompt fragments. Combine them with brand-specific blocks from `brands/[slug]/visual-style.md`.

---

## The 8-ingredient grammar

```
[SUBJECT] + [ENVIRONMENT] + [MOOD] + [LIGHTING] + [COMPOSITION] +
[CAMERA / FRAMING] + [TEXTURE / STYLE] + [NEGATIVES]
```

---

## Reusable subject fragments by mode

### Cinematic Dark
> "a single [OBJECT/SUBJECT] anchored in the center of frame, slightly luminous against the dark"

### Editorial Minimal
> "a [OBJECT] oversized and slightly cropped against a soft cream backdrop"

### Surreal Product Metaphor
> "a [OBJECT] suspended in [LIQUID/AIR/GLASS], frozen in slow motion, breaking through [SURFACE]"

### Premium Founder/Operator
> "a figure seen from behind, looking at [HORIZON/SCREEN/CITY], occupying the left third of frame"

### Technical Blueprint
> "a thin-stroke white line drawing of [SYSTEM] on a deep navy background with subtle paper grain"

### Exploded Diagram
> "a hyperreal 3D exploded view of [OBJECT], components separated and floating outward"

### Luxury Object
> "[PRODUCT] resting on a [SURFACE], single soft key light at 45°, generous negative space"

### Abstract System
> "abstract geometric forms arranged to suggest [RELATIONSHIP], single accent color among neutrals"

### AI Command Center
> "a floating holographic control surface displaying [ABSTRACT WORKFLOW], soft edge glow in [ACCENT]"

### Before/After Split Scene
> "single composition split vertically: [BEFORE STATE] on the left, [AFTER STATE] on the right"

---

## Reusable environment fragments

- "in a quiet study at dusk, wooden desk anchored in the lower frame"
- "against a soft cream backdrop with generous negative space"
- "in a vast dark space with atmospheric haze and dust motes"
- "in a museum lighting environment with neutral seamless backdrop"
- "in a softly lit interior with single window light from the left"
- "in a deep midnight scene with cool ambient fill from below"

---

## Reusable lighting fragments

- "single warm rim light from the upper-left at ~3200K"
- "soft natural daylight from the left, like a magazine cover"
- "single dramatic key light from upper-right, soft shadow falling left"
- "rembrandt lighting with one window source, deep shadows"
- "cool ambient fill from below, warm key light from above"
- "atmospheric haze with light dust motes catching the warm light"

---

## Reusable mood fragments

- "calm, considered, slightly nocturnal"
- "editorial, restrained, premium-feeling"
- "intimate, slow, deliberate"
- "tense, hushed, late-evening"
- "warm but uncompromising, like a senior operator's desk"

---

## Reusable composition fragments

- "subject occupies the upper two-thirds; bottom-third intentionally empty for headline"
- "off-center upper-third placement; left-third empty for typography"
- "rule-of-thirds with subject anchored to lower-left intersection"
- "centered subject with generous bottom margin for headline"

---

## Reusable camera / framing fragments

- "shot on 35mm film, shallow depth of field"
- "medium-format, f/4, soft falloff"
- "50mm prime, eye-level, slight low angle"
- "macro photograph, hyperreal material detail"
- "studio still life, museum-quality render"

---

## Reusable texture / style fragments

- "hyperreal materials with restrained micro-detail"
- "3D rendered with museum-quality material studies"
- "cinematic 35mm photograph with film grain"
- "editorial product photography, soft gradient backdrop"
- "vintage engineering blueprint with thin white linework"

---

## Composing a full prompt

Pick one fragment per ingredient. Combine with the brand's specific accent color, asset references, and negatives.

Example (Lumen, slide 1):

```
A fractured glass dome floating above a wooden desk in a quiet study at dusk
+ atmospheric haze with light dust motes
+ calm, considered, slightly nocturnal mood
+ single warm rim light from the upper-left at ~3200K, cool ambient fill from below
+ subject occupies the upper two-thirds; bottom-third intentionally empty for headline
+ shot on 35mm film, shallow depth of field
+ hyperreal materials: glass with light-absorption, wood with visible grain
+ no text, no logos, no watermark, no real brand names, no flat 2D illustration, no AI-typical hand anatomy, no faces.
```

---

## Per-generator suffixes

| Generator | Append |
|---|---|
| Midjourney | `--ar 4:5 --style raw --no text logo watermark` |
| ChatGPT Images | (no flags; aspect ratio in plain words) |
| Flux | (long-form natural language; no flags) |
| Leonardo | (specify model + aspect ratio in app settings) |
| Ideogram | (only when readable text needed; rare) |

---

## Prompt length

- 80-160 words is the sweet spot.
- Longer prompts often produce *worse* results — generators get confused.
- If you can't compress, the prompt is asking for too much.

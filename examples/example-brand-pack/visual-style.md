# Visual Style — `lumen`

> Cinematic dark, lit warmly, restrained typography. The visual feels like a quiet desk at 9pm, not a dashboard at noon.

---

## Visual mode

- **Primary mode:** `cinematic-dark`
- **Secondary mode:** `surreal-product-metaphor` (used selectively for hero slides; always lit and graded to match the primary mode's mood)
- **Why this mode fits:** Lumen's audience is operators in their slightly-nocturnal, slightly-tired moment of the week. The brand voice is quiet and confident. Cinematic dark mirrors the audience's actual emotional state — when they're reading our posts, it's typically late evening or early morning. Cinematic dark also separates Lumen from the bright/blue/glossy default of every other "AI productivity" brand.

---

## Color palette

| Role | Hex | When to use |
|---|---|---|
| Primary (dark base) | `#0E0F12` | dominant background on every slide |
| Accent (warm amber) | `#E2A852` | one element per slide max — single word, slide number, or rim-light highlight |
| Neutral 1 (warm cream / headline ink) | `#F4F1EC` | headline copy on dark backgrounds |
| Neutral 2 (cool gray) | `#A4A8B0` | support text, slide numbers, brand mark |
| Neutral 3 (deep midnight) | `#1A1D24` | subtle layered surfaces, gradient stops |
| Light alt base (rare) | `#F4F1EC` | only for special "editorial inversion" slides |

**Accent rule:** the warm amber `#E2A852` appears once per slide. Most often as: a single rim-light highlight on the subject, a single italicized word in the headline, or the slide number. Never as a full background.

---

## Typography

- **Headline typeface:** *GT Sectra Display* (or fallback: *Tiempos Headline*)
  - Weight: Medium
  - Case: sentence
  - Letter-spacing: tight (-0.5%)
- **Body typeface:** *Söhne* (or fallback: *Inter*)
  - Weight: Regular for body, Medium for emphasis
- **Mono typeface:** *IBM Plex Mono*
  - Used for: slide numbers and one-word labels only

**Pairing rule:** GT Sectra Display headlines + Söhne body. Plex Mono is reserved for slide numbers and the rare label. No fourth typeface.

---

## Logo / brand mark

- **Type of mark:** wordmark — "lumen" set in GT Sectra Display Medium, lowercase.
- **Placement on slide 1:** bottom-right small (14px height).
- **Placement on slides 2-N:** hidden.
- **Placement on final slide (CTA):** bottom-right small (14px), accent amber.
- **Color:** Neutral 1 (`#F4F1EC`) on dark. Accent (`#E2A852`) on the CTA slide.

---

## Composition rules

- **Subject placement:** off-center upper-third, occupying ~60% of canvas height. Bottom-third reserved for headline.
- **Headline placement:** bottom-third, left-aligned, with a 48px left margin. Two lines maximum.
- **Slide-number style:** top-right, IBM Plex Mono Regular, 14px, neutral gray (`#A4A8B0`). Format: `01 / 08`.
- **Margins:** top 48px, right 48px, bottom 48px, left 48px (for 1080×1350 canvas).
- **Aspect ratios per platform:**
  - LinkedIn (carousel doc): 4:5 (1080×1350)
  - Instagram: 4:5 (1080×1350)
  - TikTok: 4:5 (1080×1350)
  - X: 1:1 (1080×1080) — only when posting individual images

---

## Lighting & mood defaults

Single warm rim light from the upper left (color temperature ~3200K). Ambient cool fill from below at ~10% intensity. Deep blacks held to `#0E0F12`, never crushed to true black. Atmospheric haze with visible light dust motes catching the warm rim. Mood: quiet, slightly nocturnal, considered.

---

## Texture & finish defaults

Hyperreal materials when objects are present — glass with light-absorption properties, brushed metal with subtle anisotropic reflections, wood with visible grain but restrained micro-detail. The subject must read in 0.5 seconds at thumbnail size. No plastic-skin look.

---

## Depth & focus defaults

Slide 1: shallow depth of field (subject sharp, background falls off into atmospheric haze). Slides 2-N: medium DoF, softer than slide 1. No flat 2D illustrations. Always 3D-rendered or photographic look.

---

## Reusable image-prompt building blocks

### Block 1 — environment
> "in a quiet study at dusk, a wooden desk anchored in the lower frame, a deep dark backdrop receding into atmospheric haze"

### Block 2 — lighting
> "single warm rim light from the upper left at ~3200K, cool ambient fill from below at low intensity, dust motes catching the warm light"

### Block 3 — mood
> "calm, considered, slightly nocturnal, restrained, cinematic"

### Block 4 — typography reservation
> "the bottom-third of the canvas is intentionally empty for headline placement"

### Block 5 — negative prompt (always include)
> "no text, no logos, no watermark, no real brand names, no flat 2D illustration look, no AI-typical hand anatomy, no oversaturated gradient blobs, no stock-photo composition, no plastic skin, no neon glow, no cyberpunk RGB, no dashboard mockups with readable text, no faces"

---

## Three example image prompts

### Example 1 — slide 1 hero (Cinematic Dark)
> A fractured glass dome over a small wooden desk in a quiet study at dusk, the dome cracked from inside as if pressure escaped outward. Single warm rim light from the upper left at ~3200K, cool ambient fill from below, atmospheric haze with light dust motes. Hyperreal materials: glass with light-absorption properties, wood with visible grain. The dome occupies the upper two-thirds of the frame; the bottom-third is intentionally empty for headline placement. Cinematic 35mm photograph, shallow depth of field, deep blacks held to #0E0F12. No text, no logos, no watermark, no real brand names, no flat 2D illustration, no AI-typical hand anatomy, no plastic skin, no faces.

### Example 2 — proof slide (Surreal Product Metaphor — secondary mode)
> A still-life of an old analog clock floating in slow-motion outward from a wooden desk, fragments of paper calendar pages drifting in the air around it, single warm rim light from upper left, cool ambient fill from below. Hyperreal materials, atmospheric haze. The clock is centered horizontally and offset vertically toward the upper-third; the bottom-third of the canvas is intentionally empty for headline. Cinematic 35mm photograph, shallow depth of field, deep blacks held to #0E0F12. No text, no logos, no watermark, no real brand names, no flat 2D illustration, no AI-typical hand anatomy, no plastic skin, no faces.

### Example 3 — CTA slide
> A single brass key resting on a dark wooden desk surface, lit by a warm rim light from upper left, deep shadows wrapping the key into the surface, atmospheric haze in the background. The key occupies the upper-third center; the bottom-third of the canvas is intentionally empty for the CTA headline. Cinematic 35mm photograph, hyperreal material detail on the brass. No text, no logos, no watermark, no real brand names, no flat 2D illustration, no AI-typical hand anatomy, no faces.

---

## Visual do-nots (off-limits)

- **No people / faces / hands.** Lumen never shows human bodies. Anonymity is part of the brand.
- **No real product UI screenshots.** We don't want the visual to be confused with a SaaS landing page.
- **No neon / RGB / cyberpunk styling.** Wrong audience, wrong mood.
- **No flat 2D illustration.** Always 3D-rendered or photographic.
- **No bright daytime imagery.** Lumen's light is always evening or pre-dawn.
- **No multi-accent palettes.** One amber. Period.
- **No chat-bubble visuals.** Lumen is the *opposite* of chat — visualizing chat would contradict the brand.

---

## Visual decisions to revisit

- A "Mode 6 — Technical Blueprint" sub-mode for engineering-leadership posts (deferred to Q3).
- Whether to allow a single "morning brief" UI mockup as a hero on launch posts (deferred — currently leans no).
- A localized variant for non-English posts (font fallbacks for Cyrillic and CJK — deferred).

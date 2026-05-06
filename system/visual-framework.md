# Visual Framework — 9 Visual Modes

> Carousels live and die on slide 1. Slide 1 is 90% visual. The visual framework is therefore the highest-leverage part of this system.

This file defines:

1. **Universal visual rules** — what every PostStudio carousel does.
2. **Nine visual modes** — pick one per carousel. Don't mix.
3. **Composition guidelines** — what goes where on the canvas.
4. **Typography rules** — when to use what, and why.
5. **Image prompt grammar** — how to write prompts that don't produce AI slop.

---

## Universal visual rules

These apply across every mode. Break them only with intent.

1. **Strong central subject.** One thing the eye lands on. Not a collage.
2. **Bottom-third headline composition.** Headline lives in the bottom third of the canvas, not stacked on top of the subject's face.
3. **High contrast.** Light subject on dark background, or dark subject on light background. Mid-tone backgrounds kill the scroll-stop.
4. **Cinematic depth.** Foreground / midground / background. Depth-of-field. Atmospheric haze. Things receding into shadow.
5. **One accent color.** From the Brand Pack. Used for the headline, a small graphic element, or a glow on the subject. Never two accents.
6. **Brand signature, small.** A 12-16px brand mark in a corner. Not a banner. Not a logo splash.
7. **Slide numbering.** Optional. If used, small, top-right, in the accent color or muted gray.
8. **Consistent margins.** Same padding from edges across every slide. Inconsistent margins read as amateur.
9. **Minimal support text.** Body text is for the caption, not the slide. If a slide needs more than ~25 words on it, you're using slides like blog paragraphs.
10. **Visual metaphor over decoration.** A glass dome over a city says something. A gradient blob says nothing. Always reach for metaphor first.

---

## The 9 visual modes

You pick one mode per carousel. Carry it through all slides for consistency.

| # | Mode | Best for |
|---|---|---|
| 1 | Cinematic Dark | Founder takes, contrarian, premium positioning |
| 2 | Editorial Minimal | Educational, thought leadership, B2B |
| 3 | Futuristic Interface | AI / SaaS launches, technical breakdowns |
| 4 | Surreal Product Metaphor | Product-led posts, before/after, storytelling |
| 5 | Premium Founder/Operator | Founder narratives, build-in-public |
| 6 | Technical Blueprint | Architecture posts, technical breakdowns |
| 7 | Exploded Diagram | "How X works under the hood," teardowns |
| 8 | Meme-but-Premium | Cultural moment, contrarian, viral attempts |
| 9 | Decoded Editorial | Long-form (10-14 slide) tactical breakdowns, signature-density posts (adapted from `@brandsdecoded__`) |

---

### Mode 1 — Cinematic Dark

**When to use:** when you want the post to feel important. Contrarian takes, founder essays, manifesto-style content.

**Composition:**
- Deep black or near-black background (not pure #000; a desaturated dark navy or oxblood reads richer).
- A single luminous subject in the center or slight offset.
- Volumetric light, dust particles, atmospheric haze.
- Hard rim lighting on the subject.
- Headline in the bottom third, large, in white or the accent.

**Image prompt style:**
> "A [SUBJECT] in a vast dark space, lit by a single warm rim light from the upper left, cinematic 35mm photograph, deep blacks, volumetric haze, shallow depth of field, hyperreal texture, dust motes catching the light, mood of [TONE], no text, no logos, leave the bottom third empty for typography."

**Typography:**
- Large, condensed serif or geometric sans-serif.
- White or the accent color.
- Tight letter-spacing.

**Risks to avoid:**
- Black-and-orange "cyberpunk dashboard" look — it's been done to death.
- Pure black background with floating text — reads as a Notion note, not cinema.
- Too much text in the dark space — the mode demands restraint.

---

### Mode 2 — Editorial Minimal

**When to use:** when you want the post to feel like the cover of a thoughtful magazine. Educational content, thought leadership, B2B.

**Composition:**
- Off-white or soft cream background (not #FFFFFF; aim for #F4F1EC range).
- Subject is a single object, person, or graphic shape, oversized, slightly cropped.
- Generous negative space.
- Minimal shadows, soft natural lighting.
- Headline in the bottom third, set in a serif typeface.

**Image prompt style:**
> "A [SUBJECT] on a soft cream background, oversized and slightly cropped, soft natural daylight from the left, editorial magazine cover style, minimal styling, generous negative space, photographed on a medium-format camera, no text, no logos, composition leaves room at the bottom."

**Typography:**
- Editorial serif (Tiempos, GT Sectra, Canela energy).
- Black or deep ink color, never gray.
- Small caption-style label can sit in the top corner.

**Risks to avoid:**
- Pure-white plus thin sans-serif — reads as a generic SaaS landing page.
- Stock-photo composition (subject dead-center, perfect lighting). Aim for *editorial*, not *catalog*.

---

### Mode 3 — Futuristic Interface

**When to use:** AI launches, SaaS feature reveals, technical "how it works" posts.

**Composition:**
- Dark or charcoal base (#0E0F12 to #15171C range).
- Subject is a UI surface, terminal, holographic display, or glowing interface mockup.
- Subtle grid lines or data visualization in the background.
- Glowing accents (in the brand accent color) at edges or buttons.
- Bloom and chromatic aberration kept subtle (not RGB explosion).

**Image prompt style:**
> "A floating glass interface displaying [THING], soft edge glow in [ACCENT_COLOR], deep charcoal background with a subtle grid floor, volumetric light from above, depth of field falling off behind the interface, ultra-detailed, cinematic 3D render, no real brand logos, no readable text on the interface — keep UI elements abstract and shape-driven."

**Typography:**
- Geometric sans (Inter, Söhne, Neue Haas Grotesk energy).
- Headline white, accent for emphasis on one word per slide.
- Monospace allowed for one tag-like label.

**Risks to avoke:**
- "Iron Man HUD" cliché — too many circles and numbers.
- Fake screenshots that look like real apps — they read as low-effort and risk competitor confusion.

---

### Mode 4 — Surreal Product Metaphor

**When to use:** when the product itself is the hero, but you want to *say something* with the product, not just photograph it.

**Composition:**
- A scene that turns the product into a metaphor: a [PRODUCT] floating inside a glass dome, growing out of soil, suspended in liquid, breaking through a wall.
- Hyperreal texture and lighting.
- Single dramatic light source.
- Background simple (gradient, soft sky, dark void) so the metaphor pops.

**Image prompt style:**
> "[PRODUCT visualization], rendered as a hyperreal 3D scene: [METAPHOR — e.g. floating inside a glass dome over a miniature city / suspended in slow-motion splashes of water / growing out of cracked concrete]. Single dramatic key light from upper right, soft fill from below, cinematic depth of field, museum-quality material textures, photographed as still life, no text, no real brand marks."

**Typography:**
- Match Mode 1 (Cinematic Dark) or Mode 2 (Editorial Minimal) depending on overall mood.
- Headline lives in the bottom third or floats in negative space, never on top of the metaphor.

**Risks to avoid:**
- Random surrealism. "A floating pyramid of donuts" is not a metaphor; it's noise. The metaphor must connect to the carousel's argument.
- Over-rendered everything. Restrain the texture detail to keep the subject clear.

---

### Mode 5 — Premium Founder/Operator

**When to use:** founder narratives, build-in-public, "I've been thinking about" posts.

**Composition:**
- A character (the founder, an operator, an archetype). Not always a face.
- Often a back-of-the-head or three-quarter view, looking at a city, a screen, a horizon.
- Cinematic, color-graded like a film still (think *Severance*, *Foundation*, *House of Cards*).
- Low-key lighting. Strong shadows. Single key light.

**Image prompt style:**
> "A [PERSON_DESCRIPTION] seen from behind, looking at [THING_THEY_ARE_LOOKING_AT], shot on 35mm film, color-graded like a moody prestige drama, single window light, deep shadows, atmospheric haze, painterly composition, the figure occupies the left third leaving the right two-thirds for typography, no readable text, no logos."

**Typography:**
- Editorial serif or condensed display sans.
- Headline anchored to the negative space (right two-thirds, bottom third).
- White, off-white, or accent.

**Risks to avoid:**
- Real-person likeness for someone who isn't actually the post author. If the founder isn't in the photo, don't fake one.
- "AI hands" giveaway. Keep hands out of frame or partially obscured.

---

### Mode 6 — Technical Blueprint

**When to use:** architecture posts, technical deep-dives, system explanations.

**Composition:**
- Dark navy or paper-blue background (think old engineering blueprints).
- Hand-drawn or thin-stroke vector diagram of the system.
- White or accent linework.
- Annotations are minimal: short labels, arrows, and one or two highlighted nodes.
- Subtle paper texture or grid background for warmth.

**Image prompt style:**
> "A [SYSTEM_NAME] architecture diagram in the style of a vintage engineering blueprint: thin white line drawings on a deep navy background, slight paper grain, subtle grid, hand-drawn imperfection, two or three nodes highlighted in [ACCENT_COLOR], minimal labels, top-down or isometric view, no real product names, no readable text inside boxes."

**Typography:**
- Monospace or technical sans (IBM Plex Mono, JetBrains Mono).
- Small caps for labels.
- Headline above or below the diagram, not inside it.

**Risks to avoid:**
- Generic "boxes-and-arrows" with no visual hierarchy. Highlight the 1-2 nodes that matter.
- Too many labels. The carousel will explain; the diagram should *suggest*.

---

### Mode 7 — Exploded Diagram

**When to use:** teardowns, "how it works under the hood," product anatomy.

**Composition:**
- A central subject literally exploded into its parts in 3D space.
- Each part separated, with subtle motion-blur lines suggesting outward force.
- Dramatic single-source lighting.
- Studio-style background (gradient or soft seamless backdrop).

**Image prompt style:**
> "A hyperreal 3D exploded diagram of [SUBJECT], components separated and floating outward in space, museum-quality render, single key light from upper left, soft shadows, studio gradient backdrop, motion lines suggesting expansion, ultra-detailed materials, no text, no real brand logos, leave the bottom third empty for typography."

**Typography:**
- Geometric sans, headline at the bottom.
- Optional: tiny annotation labels with leader lines pointing to one or two parts.

**Risks to avoid:**
- Cluttered explosions where the parts are unrecognizable. The whole point is *clarity through separation*.
- Cartoon style. The mode demands hyperreal materials.

---

### Mode 8 — Meme-but-Premium

**When to use:** cultural moments, contrarian takes, posts where the audience expects play, but you want to keep brand.

**Composition:**
- Borrow meme structure (e.g., the two-button choice, the panicked archer, the "this is fine" room) but render it as if it belonged in *Wired* or *The New York Times Magazine*.
- Hyperreal photography or 3D render replaces the cartoon original.
- Subtle absurdity, not slapstick.

**Image prompt style:**
> "A photoreal cinematic still inspired by [MEME_STRUCTURE]: [DESCRIBE_SCENE], shot on a medium-format camera, magazine editorial lighting, slight surreal undertone, no text overlays, no real brand logos, leave space at the bottom for headline."

**Typography:**
- Editorial serif or chunky display sans.
- The headline should *play* against the visual (deadpan beats over-explanation).

**Risks to avoid:**
- Using a worn-out meme template. If it's been seen 10,000 times, the upgrade to "premium" doesn't save it.
- Letting the joke override the message. The meme should *carry* the argument, not replace it.

---

### Mode 9 — Decoded Editorial

**When to use:** long-form (10-14 slide) tactical breakdowns where you want signature-density. Reference: the visual language of `@brandsdecoded__`. Best for "techniques," "commands," "tactics," "playbooks." Not for short carousels (≤8 slides) — use Mode 2 instead.

This is a **system-level template** that any brand can adopt. The brand's `visual-style.md` declares it via `primary_visual_mode: decoded-editorial` and supplies the placeholder values. The Brand Pack provides the colors, fonts, mark file, header labels, and CTA pattern; this Mode provides the canonical layout.

**Composition (canvas 1080×1350; margins 64px top/bottom, 80px left/right):**

Universal chrome on every slide:
- 4px `[BRAND_ACCENT]` line on top edge.
- Header band: 11px `[BODY_FONT]` Regular UPPERCASE — `[BRAND_HEADER_LABEL_LEFT]` left + `[BRAND_HEADER_LABEL_RIGHT]` right. Opacity 50/60% on dark, 45/55% on light.
- Footer: 2px `[BRAND_ACCENT]` progress bar (proportional fill across the carousel) + 11px slide number "N/M" bottom-right.
- Background alternates between `[BRAND_DARK_BG]` and `[BRAND_LIGHT_BG]` per slide rhythm. Never two adjacent dark or light slides without a reason.

Slide types:

1. **Hero (slide 1):** full-bleed object photography (no faces) + dark gradient overlay on bottom 60%. Profile chip pill ~600px from top, 80px from left: dark rounded pill with `[BRAND_MARK_FILE]` + `[@BRAND_HANDLE]` + tiny `[BRAND_ACCENT]` verified dot. Headline (lower 40%): 76-92px `[HEADLINE_FONT]` SemiBold/Bold, `[BRAND_INK_ON_DARK]`, with one word/phrase in `[BRAND_ACCENT]`. Tease arrow below headline: 14px `[BODY_FONT]` Regular UPPERCASE.

2. **Body Light:** `[BRAND_LIGHT_BG]` solid background. Empty top 30-40% (intentional negative space). Section eyebrow ~480px from top: 11px `[BODY_FONT]` Regular UPPERCASE letter-spaced 0.08em, `[BRAND_INK_ON_LIGHT]` 50%. Headline ~60% from top: 80-110px `[HEADLINE_FONT]` SemiBold/Bold, sentence-case or ALL CAPS, with one word in `[BRAND_ACCENT]`. Body text below: 22-26px `[BODY_FONT]` Regular `[BRAND_INK_ON_LIGHT]` 65%, with selective bold inline. 2-4 short paragraphs, 80-200 words. Optional mockup card (radius 16px, soft shadow, 70% width centered).

3. **Body Dark:** same as Body Light but inverted colors. Background `[BRAND_DARK_BG]`. Eyebrow `[BRAND_INK_ON_DARK]` 40%, headline `[BRAND_INK_ON_DARK]` (with `[BRAND_ACCENT]` word), body `[BRAND_INK_ON_DARK]` 75%. Optional ghost number watermark: 480-580px `[HEADLINE_FONT]` Bold `[BRAND_INK_ON_DARK]` 4% (very subtle), anchored bottom-right behind the text.

4. **CTA (final slide):** `[BRAND_LIGHT_BG]`. Two-line headline pattern: `[CTA_HEADLINE_LINE_1]` (`[BRAND_INK_ON_LIGHT]`) / `[CTA_HEADLINE_LINE_2]` (`[BRAND_ACCENT]`), 80-110px `[HEADLINE_FONT]` Bold ALL CAPS. `[BRAND_ACCENT]` pill action bar (full content-width, 64px tall, radius 12px). CTA card: white card + soft shadow, label + HUGE TRIGGER WORD (76-92px `[HEADLINE_FONT]` Bold). Footer attribution: tiny `[BRAND_MARK_FILE]` + `[@BRAND_HANDLE] · [CTA_FOOTER_TEXT]`.

**Slide rhythm template:**

| Slide # | Type | Background |
|---|---|---|
| 1 | Hero | full-bleed photo + dark overlay |
| 2 | Body | LIGHT or DARK (the "what they say" beat) |
| 3 | Body Dark | the problem named with ALL CAPS |
| 4 — N-2 | Alternating Body Light / Body Dark | one technique per slide |
| N-1 | Body Light | summary list ("THE N TACTICS." pattern) |
| N | CTA | LIGHT |

**Required Brand Pack placeholders (adopt this Mode by setting these in your `visual-style.md`):**

| Placeholder | Example value | Purpose |
|---|---|---|
| `[BRAND_ACCENT]` | `#CCFF00` | The single accent color |
| `[BRAND_DARK_BG]` | `#0B0B0A` | Dark slide background |
| `[BRAND_LIGHT_BG]` | `#FAF8F3` | Light slide background |
| `[BRAND_INK_ON_DARK]` | `#F2EFE8` (cream) | Text color on dark slides |
| `[BRAND_INK_ON_LIGHT]` | `#0B0B0A` (deep ink) | Text color on light slides |
| `[HEADLINE_FONT]` | `"Manrope"` | Headline font family |
| `[BODY_FONT]` | `"Manrope"` | Body font family (often same) |
| `[BRAND_MARK_FILE]` | uploaded `mark-primary.svg` | The actual brand mark asset (NEVER redrawn) |
| `[@BRAND_HANDLE]` | `"@your_handle"` | Social handle for profile chip + footer |
| `[BRAND_HEADER_LABEL_LEFT]` | `"POWERED BY POSTSTUDIO"` | Top-left header label |
| `[BRAND_HEADER_LABEL_RIGHT]` | `"@poststudio.ai · MAY 2026 ®"` | Top-right header label (handle + month/year + ®) |
| `[CTA_HEADLINE_LINE_1]` | `"FEITO NO"` / `"BUILT WITH"` | First line of CTA headline (ink color) |
| `[CTA_HEADLINE_LINE_2]` | `"YOURBRAND."` | Second line of CTA headline (accent color) |
| `[CTA_FOOTER_TEXT]` | `"Auto-reply via DM"` | Subtle CTA footer attribution |

**Asset rule:** the brand mark must be the actual SVG/PNG file the user supplied this session — embedded as base64 in `<image href="data:...">` for SVG, or via `<img>` for HTML. Never redraw the mark via SVG primitives.

**Risks to avoid:**
- Using this Mode for short (<10 slides) carousels — the rhythm needs slides to breathe. Use Mode 2 instead.
- Forgetting placeholders. If a brand adopts this Mode without filling all 13 placeholders, output will be inconsistent.
- Two adjacent dark slides or two adjacent light slides — breaks the rhythm.
- Drawing the brand mark from scratch instead of reading the supplied file.

---

## How to choose a mode

A 3-question filter:

1. **What is the carousel's emotional center?**
   - Inspiring → Mode 1, 5
   - Rigorous → Mode 2, 6, 7, 9
   - Energetic → Mode 3, 4
   - Playful → Mode 8
   - Tactical / signature density → Mode 9

2. **Who is the audience?**
   - Investors, executives, strategists → Mode 1, 2, 5
   - Engineers, technical operators → Mode 3, 6, 7, 9
   - Founders, builders → Mode 1, 4, 5, 9
   - Mixed / cultural → Mode 8

3. **What is the platform / slide count?**
   - LinkedIn → Mode 1, 2, 5, 6, 9 perform best
   - Instagram (≥10 slides) → Mode 9 performs best for tactical density; Mode 1, 4, 7 for shorter
   - X — minimal carousel platform; lean Mode 2 or 6 if used
   - TikTok carousels → Mode 4, 8 perform best

If unsure for a short carousel, default to **Mode 2 (Editorial Minimal)**. For a long carousel (≥10 slides) where the brand has adopted it, default to **Mode 9 (Decoded Editorial)**.

---

## Image prompt grammar

A prompt has 8 ingredients. Use this order:

```
[SUBJECT] + [ENVIRONMENT] + [MOOD] + [LIGHTING] + [COMPOSITION] +
[CAMERA / FRAMING] + [TEXTURE / STYLE] + [NEGATIVE / AVOID]
```

Example:
> "A weathered glass dome (subject) sealing a miniature city (environment), end-of-day amber atmosphere with rolling fog (mood), single warm rim light from the upper right, ambient cool fill from below (lighting), bottom-third left empty for typography (composition), shot on a 50mm lens at f/1.8 (camera), hyperreal 3D render with museum-quality material detail (texture/style), no text, no logos, no people, no flat illustration look (avoid)."

### Negative prompts to use almost always

```
no text, no logos, no watermark, no real brand names, no flat 2D
illustration, no AI-typical hands, no oversaturated gradient blobs,
no stock-photo composition, no awkward face anatomy, no inconsistent
perspective, no random typography overlays
```

### Reserve space for typography

The image prompt should explicitly say: *leave the [bottom third / right side / top left] empty for headline placement*. Image generators respect this if you say it.

### Prefer "no text" over "with text"

Most image models put garbage text on slides. Generate the visual without text, then layer typography in Figma / Canva / Paper.design.

The only exception: meme structures (Mode 8) where the visual concept *requires* a single readable word, and you accept that you'll likely retry several times.

---

## Layout instructions per slide

The image is half the slide. Layout is the other half. Standard layout instruction template:

```
- Image: [mode], rendered with [composition direction]
- Headline: [bottom-third / top-left / center], [serif / sans], [color]
- Accent: [accent color] used on [one element]
- Slide number: [top-right small / hidden]
- Brand mark: [bottom-left small / hidden]
- Margins: [16px / 32px / 48px from edges]
```

Consistency across slides is the deliverable. Pick once, repeat.

---

## Common visual failure modes

A debugging list. If the carousel looks "AI-y," check these:

1. **Centered text on top of subject's face.** Move it to bottom-third.
2. **Two accent colors.** Pick one, drop the other.
3. **Gradient blob backgrounds.** Replace with a real environment or a single dramatic color.
4. **Stock-photo composition.** Reframe to off-center, cropped, editorial.
5. **Inconsistent slide margins.** Pick one padding, apply to all.
6. **Tiny logo cropping a corner.** Either commit to a brand mark or drop it.
7. **Over-rendered everything.** Strip texture detail until the subject reads in 0.5 seconds.
8. **AI-typical hands or eyes.** Hide hands. Avoid frontal eye contact unless deliberate.
9. **Clichéd metaphors (lightbulb = idea, gears = system).** Reach further.
10. **Headline that fights the image.** Either move the headline or swap the image; don't compromise.

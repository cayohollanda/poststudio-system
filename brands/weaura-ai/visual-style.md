# Visual Style — `weaura-ai`

> Editorial-minimal with a distinctive **volt** accent (`#CCFF00`). Brand kit lives at `brands/weaura-ai/{mark,favicon,lockup,svg}/`. Aperture mark + Manrope wordmark.
>
> Source of truth for the asset pack: [`brands/weaura-ai/README.md`](README.md) (Logo Pack v2.0).

---

## ⚠️ NEVER redraw the Aperture mark

The Aperture mark is a precise, brand-defining geometry. **Claude must never redraw it via SVG primitives** (`<circle>`, `<polygon>`, `<path>`, etc.) or approximate its geometry. Always embed the actual file as base64.

Required assets and their canonical paths:

| When | Use this asset | Base64-embed in renderable code as |
|---|---|---|
| Dark background slide | `brands/weaura-ai/svg/mark-primary.svg` (cream + volt dot) | `data:image/svg+xml;base64,...` |
| Dark slide with full lockup | `brands/weaura-ai/svg/lockup-transparent-cream.svg` | same |
| Light background slide | `brands/weaura-ai/svg/mark-dark.svg` (dark + volt dot) | same |
| Light slide with full lockup | `brands/weaura-ai/svg/lockup-transparent-dark.svg` | same |
| Slide with VOLT background | `brands/weaura-ai/svg/mark-dark-on-volt.svg` (black dot, no volt in mark) | same |

If the binary SVG files cannot be read in the current runtime (some Project Knowledge configurations only expose markdown text), **stop and ask the user to upload the SVGs into the chat**. Do **not** fall back to redrawing the aperture from primitives — the result will be visibly wrong every time and erode brand consistency.

The most common failure mode in past sessions: Claude generated an SVG with `<circle>` primitives approximating the Aperture and shipped that as the mark. The audience notices. The brand pays. **Read the file or stop.**

---

## Visual mode `decoded-editorial` — the WeAura primary template

> Layout pattern extracted from `@brandsdecoded__` (Brazilian AI Content Agency, 287K seguidores). We borrow **layout** — composition, density, slide rhythm, hook formula, header chrome, progress bar — and adapt to WeAura's identity (VOLT accent, CREAM/DARK palette, Manrope typography, Aperture mark, no faces). We do NOT clone brandsdecoded's visual identity (orange accent, "POWERED BY CONTENT MACHINE" footer, condensed Druk-style display sans, photographic Hero).

**This is the default mode for all WeAura long-form carousels (10-14 slides).** Use `editorial-minimal` only for short-form (5-8 slides) where a bottom-third headline is sufficient. Use `technical-blueprint` only for engineering essays with architecture diagrams.

---

### Canvas + grid

- **Canvas:** 1080×1350 (4:5).
- **Outer margins:** 64px top, 64px bottom, 80px left, 80px right.
- **Header zone:** top 0-100px (header bar lives here).
- **Content zone:** 100-1280px (eyebrow + headline + body + mockups).
- **Footer zone:** bottom 1280-1350px (progress bar + slide number).

### Header chrome (every slide except none)

```
[ 4px VOLT line, full width, top edge ]
[ 64px gap ]
POWERED BY POSTSTUDIO                @poststudio.ai · MAY 2026 ®
^ left, 11px Manrope Regular UPPERCASE, letter-spacing 0.08em, opacity 50%
                                       ^ right, 11px Manrope Regular UPPERCASE, letter-spacing 0.08em, opacity 60%
```

- On dark slides: text is CREAM at 50%/60% opacity.
- On light slides: text is DARK at 45%/55% opacity.
- The 4px VOLT top-edge line stays VOLT regardless of background.
- Date (`MAY 2026`) auto-derived from publish date. Use English month-name in caps.
- The `®` is decorative — not a registered-trademark assertion.

### Slide types (the rhythm)

A typical WeAura `decoded-editorial` carousel has 10-14 slides alternating between **light** (CREAM/LIGHT) and **dark** (DARK) bodies, with a dedicated Hero (slide 1) and CTA (slide N).

| Slide # | Type | Background | Purpose |
|---|---|---|---|
| 1 | Hero | object/blueprint composition with dark overlay | scroll-stop hook |
| 2 | Body Light or Dark | sets the conventional belief / what they say | "you've heard this before…" |
| 3 | Body Dark (typically) | names the problem with ALL-CAPS headline | "CONTEXT ROT." moment |
| 4-N-2 | Alternating Body Light / Body Dark | one technique / argument per slide | the body of the argument |
| N-1 | Body Light | summary list ("AS 8 TÁTICAS." pattern) | the recap |
| N | CTA | LIGHT background | "FEITO NO WEAURA." pattern |

**Alternation rule:** never two adjacent dark slides; never two adjacent light slides without a reason. Rhythm matters more than economics.

---

### Type A — Hero (slide 1)

- **Background:** full-bleed object photography (no faces — per `constraints.md`). Examples: a single brass camera aperture in deep dusk light; a server rack with one VOLT-lit detail; a glass dome over a wooden desk; a labeled engineering blueprint floating in dark space.
- **Overlay:** dark gradient from 50% to 90% opacity covering bottom 60% of canvas, so headline reads.
- **Header chrome:** as defined above (CREAM tones at 50/60% opacity on the photo).
- **Profile chip** (centered horizontally, ~600px from top, left-aligned to text margin):
  - Dark rounded pill (~280×56px, radius ~28px), background `rgba(0,0,0,0.55)` with subtle border.
  - Inside: Aperture mark icon (40px, `mark-primary` from `brands/weaura-ai/svg/`) + "@weaura_ai" in 18px Manrope SemiBold CREAM + a small VOLT verified-style dot.
- **Headline** (lower 40% of canvas, left-aligned, 80px from left):
  - 76-92px Manrope SemiBold or Bold (the boldest weight available).
  - Sentence case OR ALL CAPS — pick per topic (ALL CAPS for declarative claims; sentence case for narrative hooks).
  - 2-4 lines max.
  - **One word/phrase in VOLT** — picked for emphasis (the noun that carries the tension).
  - Color (non-VOLT): CREAM `#F2EFE8`.
  - Letter-spacing tight (-0.5% to -1%).
  - Line-height 0.95-1.05 (tight, almost touching).
- **Tease arrow** (below headline, ~32px gap):
  - "→ AND HOW TO APPLY THIS IN PRACTICE" or PT equivalent.
  - 14px Manrope Regular UPPERCASE, letter-spacing 0.06em, CREAM at 65% opacity.
- **Slide number** (bottom-right, 64px from right, 80px from bottom):
  - "1/10" in 11px Manrope Regular CREAM at 50% opacity.
- **Bottom progress bar:** thin 2px VOLT line filling the proportional left portion, CREAM at 15% opacity for the rest.

### Type B — Body Light

- **Background:** solid CREAM `#F2EFE8` (preferred) or LIGHT `#FAF8F3`.
- **Header chrome:** as defined (DARK tones).
- **Empty zone** (top 30-40% of content area): intentional negative space. Do NOT fill it. This is the most distinctive feature of this template — slides BREATHE at the top.
- **Section eyebrow** (just above headline, ~480px from top):
  - "O PROBLEMA" / "TÉCNICA 5" / "COMANDO 2" / "O RESUMO" / "EM PRÁTICA" — uppercase narrative beat.
  - 11px Manrope Regular UPPERCASE, letter-spacing 0.08em, color DARK at 50% opacity.
- **Headline** (centered vertically around 60% from top, left-aligned, 80px from left):
  - 80-110px Manrope SemiBold or Bold.
  - Sentence case for narrative ("E foi assim que **construímos**:") or ALL CAPS for emphatic ("CONTEXT ROT.", "PLAN MODE.", "/BTW").
  - 1-3 lines max.
  - **One word/phrase in VOLT** — and at most one word inline-bold.
  - Color (non-VOLT): DARK `#0B0B0A`.
  - Letter-spacing tight (-0.5% to -2% for ALL CAPS).
  - Line-height 0.95-1.05.
- **Body text** (below headline, ~32px gap, full content width minus margins):
  - 22-26px Manrope Regular.
  - Color: DARK at 65% opacity (medium gray feel).
  - Selective bold inline (~1-3 phrases per body block) in Manrope SemiBold DARK at 100%.
  - Optional: ONE phrase highlighted in VOLT (use sparingly — body bold is preferred).
  - 2-4 short paragraphs, 80-200 words total.
  - Line-height 1.4-1.5 (open, readable).
- **Optional embedded mockup** (below body):
  - Rounded card (radius 16px), soft shadow.
  - 70% width, centered horizontally.
  - Used for: profile screenshots, engagement-metric pills floating over a post.
- **Bullet lists** (when used):
  - Use ✓ green-on-light-tint chip and ✗ red-on-light-tint chip for confirm/deny lists, OR
  - Use `→` arrow with VOLT color as bullet for action lists.
- **Slide number + progress bar:** as defined.

### Type C — Body Dark

Same as Type B but inverted:

- **Background:** solid DARK `#0B0B0A`.
- **Section eyebrow:** CREAM at 40% opacity.
- **Headline:** CREAM (non-VOLT portion). VOLT word stays VOLT.
- **Body text:** CREAM at 75% opacity.
- **Bold inline:** Manrope SemiBold CREAM at 100%.
- **Optional ghost number watermark** (a distinctive Type C feature):
  - Place a HUGE numeral (the technique number, e.g. "10") behind the headline + body, anchored bottom-right of the content area.
  - Font: 480-580px Manrope Bold.
  - Color: CREAM at 4% opacity (very subtle).
  - The text content reads OVER it.
- **Progress bar:** VOLT filled + CREAM at 15% unfilled.
- **Slide number:** CREAM at 50%.

### Type D — CTA (final slide)

- **Background:** solid LIGHT `#FAF8F3` (preferred) or CREAM `#F2EFE8`.
- **Header chrome:** as defined.
- **Two-line headline** (lower 50%, large):
  - First word(s) DARK + last word in VOLT.
  - Example: "FEITO NO" (DARK) / "WEAURA." (VOLT) — note: the brand name in the headline is acceptable HERE on the CTA slide (and only here).
  - 80-110px Manrope Bold, ALL CAPS, very tight letter-spacing.
- **Body explanation** (below headline, ~32px gap):
  - 22-26px Manrope Regular DARK at 65%.
  - 2-3 short sentences explaining what the carousel was made with / what to do next.
  - Bold inline: 1-2 phrases.
- **VOLT pill action bar** (full content-width):
  - VOLT background, DARK text (high contrast).
  - 22px Manrope SemiBold.
  - Single line, ~64px tall, radius 12px.
  - Example: "Demo gratuita por tempo limitado." OR "30 min com nossa equipe."
- **CTA card** (below pill, full content-width):
  - White/CREAM background, soft shadow, radius 16px.
  - Top-left label: "Comenta a palavra abaixo:" — 14px Manrope Regular DARK at 50%.
  - Center: HUGE TRIGGER WORD (e.g. "DEMO" or "WEAURA") in 76-92px Manrope Bold DARK or VOLT, letter-spacing tight.
  - Below trigger: subtitle in 16-18px Manrope Regular DARK at 60% (the value swap — "and I'll send you [the resource]").
- **Footer attribution row** (below card):
  - Tiny Aperture mark icon (~24px) + "@weaura_ai · Envio automático via DM" — 14px Manrope Regular DARK at 50%.
- **Slide number + progress bar:** N/N (full bar).

---

### Slide-1 Hero composition options for WeAura (no faces)

Per `constraints.md`, we don't use faces. Hero compositions for `decoded-editorial`:

1. **Brass aperture closing** — a camera aperture mechanism in macro, lit by a single VOLT highlight at the center. Cinematic dusk lighting.
2. **Single labeled artifact** — a notarial-sealed document with a brass seal, lit by warm rim light.
3. **Engineering blueprint** — an unfolded technical diagram on cream paper, with dramatic single-source lighting.
4. **Server-rack still life** — a single rack panel in deep blue-black light, with one detail lit in VOLT.
5. **Glass dome over wooden desk** — the existing `editorial-minimal` Hero pattern, adapted full-bleed with dark gradient overlay.

Always: deep cinematic depth, single dramatic light source, hyperreal materials, no people, no glowing brains, no AI clichés.

---

### Hook formula (slide 1)

Brandsdecoded uses claim-based hooks that build tension. WeAura adapts:

- **Pattern 1 — The Death/Disappearance claim:** "A MORTE DOS [THING]: por que [audience] [should X]?" → adapted to WeAura: "A MORTE DA IA GENÉRICA EM OPS: por que cada SRE deveria parar de adivinhar?"
- **Pattern 2 — The Question Hook:** "[NUMBER] [TÉCNICAS / TÁTICAS / VERDADES] PARA [OUTCOME]"
- **Pattern 3 — The Italicized Contrast (WeAura signature):** "A IA QUE _CONHECE_ SUA INFRAESTRUTURA — e por que tudo o resto está chutando."
- **Pattern 4 — The Threshold:** "Se você ainda [BEHAVIOR] em 2026, [STATEMENT]."

Always pair the hook with a tease arrow line below: "→ AND HOW [WeAura] DOES IT IN PRACTICE" or PT.

---

### Section eyebrow taxonomy (above headline on body slides)

Pick one per slide. These mirror brandsdecoded's narrative beats but use WeAura vocabulary:

- `O PROBLEMA` — for problem-naming slides (often paired with ALL CAPS headline).
- `O ERRO COMUM` — for the conventional-wisdom slide.
- `O REFRAME` — for the new-mental-model slide.
- `MECANISMO N` — for mechanism / technique slides (numbered).
- `COMANDO N` — for tool/command-introduction slides.
- `EVIDÊNCIA` / `PROVA` — for proof slides.
- `O RESUMO` — for the summary slide.
- `EM PRÁTICA` — for the application slide.

---

### Density rules (different from `editorial-minimal`)

`decoded-editorial` is text-richer than `editorial-minimal`:

- Body slides: 80-200 words.
- 2-4 short paragraphs.
- Headline + body always coexist (headline alone is rare).
- Empty space at the TOP of each body slide is intentional (creates editorial breathing).
- Small components (mockups, pills, eyebrows) are allowed in addition to headline + body.

---

### Embedded mockups (when used)

When a slide needs social proof or product mockup:

- **Profile screenshot card:** rounded 16px corners, soft shadow, ~70% width centered. Use a real WeAura profile screenshot or a styled placeholder. Keep it 1.5x scaled-down so the slide reads first, the screenshot supports.
- **Post mockup with engagement pill:** a smaller carousel-thumbnail card with a floating white pill chip "10,2 mil curtidas / há 3 dias" overlaid on top-right. The pill is white/CREAM with DARK text at 70%, soft shadow.
- **Code/terminal mockup:** dark rounded card with monospace text. Use IBM Plex Mono. CREAM text on `#1A1A1A` background.

Always reference real screenshots stored under `brands/weaura-ai/mockups/` if they exist; never generate fake screenshots from scratch.

---

### Comparison: `decoded-editorial` vs `editorial-minimal`

| Dimension | `decoded-editorial` (NEW default) | `editorial-minimal` (alternate) |
|---|---|---|
| Headline placement | lower-half (50-70% from top) | bottom-third |
| Empty space | intentional at TOP | intentional at TOP |
| Top header | thin VOLT line + label-left + handle-right | none |
| Body text | 80-200 words below headline | 0-25 words on slide |
| Section eyebrow | required above headline on body slides | not used |
| Slide number | bottom-right with progress bar | top-right |
| Slide count | 10-14 (long-form) | 7-10 (short-form) |
| Mockups | embedded inline frequently | rare |
| Best for | full-funnel essays, agency-style content | thought-leadership, founder essays |

---

### When to use `decoded-editorial`

- Long-form carousels (10-14 slides).
- Topics with multiple techniques / arguments / steps.
- "How we [DID X]" or "[N] táticas para [Y]" framings.
- Anything where the audience would benefit from text-rich body content per slide.
- The default for WeAura content carousels going forward, unless the brief explicitly requests another mode.

### When NOT to use it

- Short-form (≤8 slides) where the rhythm doesn't have room to breathe.
- Pure thought-leadership essays where one big sentence per slide is the better choice (use `editorial-minimal`).
- Engineering deep-dives with diagrams (use `technical-blueprint`).

---

### Reference (do not commit; for internal use)

The visual reference comes from 3 brandsdecoded__ carousels on `/tmp/brandsdecoded-ref/`:
- "A MORTE DOS REELS..." (10 slides, hero photo + body alternation)
- "A maioria das pessoas não entende como o Claude usa tokens..." (14 slides, all-typographic, technique-numbered)
- "Essa conta nasceu em janeiro de 2025 com zero seguidores..." (10 slides, growth narrative)

These screenshots are stored locally for layout reference. They are **NOT committed** to the repo (copyright, brandsdecoded's IP). The patterns are adapted into this template; the source images are not reproduced in WeAura output.

---

## Visual mode

- **Primary mode (long-form, 10-14 slides):** `decoded-editorial` — see the dedicated section below. Default for WeAura content carousels.
- **Alternate mode (short-form, 7-10 slides):** `editorial-minimal` — cream-on-dark or cream-on-light backgrounds, restrained typography in the bottom-third. Use when the topic is tight enough for one big sentence per slide.
- **Specialty mode (engineering diagrams):** `technical-blueprint` — deep dark backgrounds, thin volt linework for architecture / system-flow content.
- **Tertiary (Hero only):** `surreal-product-metaphor` — a single luminous object lit with a single volt highlight, for slide-1 heroes that demand drama. Often combined with `decoded-editorial` body slides.
- **Why these modes fit:** the audience is senior engineers who distrust over-styled tech marketing. `decoded-editorial` brings the magazine/agency feel that the user has explicitly anchored to (`@brandsdecoded__` template) while keeping WeAura's identity (VOLT/CREAM/DARK + Manrope + Aperture). `editorial-minimal` and `technical-blueprint` remain available for content types that don't fit the long-form rhythm.

---

## Color palette

> Locked from the official Logo Pack v2.0 (`brands/weaura-ai/README.md`).

| Role | Token | Hex | When to use |
|---|---|---|---|
| Primary background (dark) | `DARK` | `#0B0B0A` | dominant background for cinematic / blueprint slides |
| Cream (lâminas sobre dark) | `CREAM` | `#F2EFE8` | headline ink on dark; surface tone on cream slides |
| Light background (warm) | `LIGHT` | `#FAF8F3` | dominant background for editorial-minimal light slides |
| Accent | `VOLT` | `#CCFF00` | one element per slide max — single-word headline emphasis, mark dot, single graphic detail |
| Ink (on light) | `DARK` | `#0B0B0A` | headline / body ink on light backgrounds |

**Accent rule (VOLT):**
- Volt appears **once per slide**, on **one element** — a single word in the headline, the dot of the aperture mark, or a single graphic detail. Never two volt elements in one slide.
- Volt is **never used as a background or as a glow effect.** It's a confident accent, not a neon spread.
- Cream and dark do the heavy lifting; volt is the punch.

---

## Typography

> Locked: Manrope (Regular 400 + SemiBold 600) — open-source, Google Fonts.

- **Headline typeface:** **Manrope SemiBold (600)**
  - Case: sentence
  - Letter-spacing: -0.5% to -1% (tight, especially at large sizes)
  - Line-height: 1.05-1.10 for tight cinematic looks
- **Body typeface:** **Manrope Regular (400)**
  - For support text, captions, and slide-numbers fallback.
- **Mono typeface:** _(to be confirmed — propose IBM Plex Mono or JetBrains Mono for code identifiers like `payment-api`, `kubectl get pods`, slide-number labels)_

**Pairing rule:** Manrope SemiBold for headlines, Manrope Regular for body. Add one mono face for code only. No fourth typeface.

---

## Logo / brand mark

- **Mark name:** "Aperture" (per the official asset pack).
- **Two main forms:**
  - **`mark-*`** — detailed Aperture (variation of opacity) for sizes ≥ 64px.
  - **`favicon-*`** — flat Aperture (full opacity, larger central dot) for sizes ≤ 256px.
- **Lockup:** Aperture + "WeAura AI" wordmark in Manrope (proportion ~2.64:1).

### Variant selection

| Background | Mark variant | Lockup variant | File path |
|---|---|---|---|
| Dark | `mark-primary-*` (cream + volt dot) | `lockup-on-dark-*` | `brands/weaura-ai/mark/` and `brands/weaura-ai/lockup/` |
| Light | `mark-dark-*` (dark + volt dot) | `lockup-on-light-*` | same |
| Volt | `mark-dark-on-volt-*` | `lockup-on-volt-*` (black dot, no volt in mark) | same |
| Transparent | various transparent variants | `lockup-transparent-*` (cream or dark) | same |

### Placement rules

- **Slide 1:** lockup `mark + wordmark` bottom-right or bottom-center, ~14-16px height (or proportional).
- **Slides 2-N:** mark only (no wordmark) bottom-right small, OR hidden depending on slide density.
- **Final slide (CTA):** lockup again, slightly larger, paired with the CTA headline.
- **Color:** match background (cream-mark on dark, dark-mark on light, etc.) per the variant table above.

---

## Composition rules

- **Subject placement:** off-center upper-third for hero slides; subject occupies ~60% of canvas height. Bottom-third reserved for headline.
- **Headline placement:** bottom-third, left-aligned, 48px from left margin (on a 1080-wide canvas).
- **Slide-number style:** top-right, mono typeface, 14px, neutral gray (cream-derived). Format: `01 / 08`.
- **Margins:** 48px from each edge (default).
- **Aspect ratios per platform:**
  - LinkedIn (carousel doc): 4:5 (1080×1350)
  - Instagram: 4:5 (1080×1350) or 1:1
  - X (Twitter): 1:1 or 16:9
  - TikTok: 4:5 (1080×1440)

---

## Lighting & mood defaults

For `editorial-minimal` (default):
- Soft natural daylight from the left; minimal harsh shadows.
- Generous negative space; the subject breathes.
- Restrained styling.
- Mood: considered, technical, anti-marketing.

For `technical-blueprint` (Engineering essays):
- Deep dark background (`#0B0B0A`).
- Thin cream linework (`#F2EFE8` strokes).
- Subtle paper grain / texture for warmth.
- Two volt highlights max on critical nodes — never more than two volt accents on one slide.

For `surreal-product-metaphor` (hero only — rare):
- Single luminous object (aperture-like; a single brass dial; a labeled artifact) on dark backdrop.
- One volt rim highlight on the subject.
- Atmospheric haze; deep shadows.
- Used sparingly — slide 1 of a major launch carousel, not every post.

---

## Texture & finish defaults

- Editorial product / object photography with restrained micro-detail.
- Hyperreal materials when 3D is used (rare; reserved for hero slides).
- No flat 2D illustration.
- No neon glow / cyberpunk styling — volt is an accent, not a background field.
- No "AI brand" tropes (matrix code, glowing brain, abstract networks of dots, futuristic interfaces with floating numbers).

---

## Reusable image-prompt building blocks

> Modular fragments to combine into image prompts. Refine per generator (Midjourney / Flux / ChatGPT Images) using [`prompts/05-generate-image-prompts.md`](../../prompts/05-generate-image-prompts.md).

### Block 1 — environment (editorial-minimal, light)
> "soft warm cream backdrop (#FAF8F3), generous negative space, single soft natural daylight from the left, minimal styling"

### Block 2 — environment (editorial-minimal, dark)
> "deep warm dark backdrop (#0B0B0A), generous negative space, single soft cool light from the upper-left, atmospheric depth"

### Block 3 — environment (technical-blueprint)
> "deep warm dark backdrop with subtle paper grain, vintage engineering blueprint aesthetic, thin cream-colored linework"

### Block 4 — subject (object metaphor with volt highlight)
> "a [SUBJECT — e.g. a single brass aperture dial; a labeled mechanical artifact; an unfolded technical diagram] oversized and slightly cropped, with a single volt-yellow (#CCFF00) accent highlight on one detail"

### Block 5 — typography reservation
> "the bottom-third of the canvas is intentionally empty for headline placement"

### Block 6 — negative prompt (always include)
> "no text, no logos, no watermark, no real brand names, no flat 2D illustration look, no AI-typical hand anatomy, no oversaturated gradient blobs, no stock-photo composition, no cyberpunk RGB glow, no neon spread, no Matrix-style code, no glowing brain, no faces, no fake competitor UIs"

---

## Three example image prompts

### Example 1 — slide 1 hero (surreal-product-metaphor)
> "A single brass aperture dial floating in a deep warm dark space (#0B0B0A) with atmospheric haze, a single volt-yellow (#CCFF00) accent highlight on the central dot of the aperture, soft cool rim light from the upper-left, generous negative space at the bottom for headline placement, hyperreal material detail on the brass and the aperture mechanism, museum-quality 3D render, no text, no logos, no watermark, no real brand names, no flat 2D illustration, no AI-typical hand anatomy, no faces."

### Example 2 — mechanism slide (technical-blueprint)
> "A thin cream-colored line architecture diagram on a deep warm dark background (#0B0B0A) with subtle paper grain, vintage engineering blueprint aesthetic, two nodes highlighted in volt-yellow (#CCFF00), the diagram occupies the upper two-thirds of the frame, bottom-third intentionally empty for headline. No text inside boxes, no real product names, no flat illustration, no AI-typical hand anatomy."

### Example 3 — CTA slide (editorial-minimal, light)
> "A single small brass dial on a warm cream backdrop (#FAF8F3), soft natural daylight from the left, a single volt-yellow (#CCFF00) highlight on the dial's center dot, generous negative space at the bottom for the CTA headline, editorial product photography style, hyperreal material detail, no text, no logos, no watermark, no flat 2D illustration."

---

## Visual do-nots (off-limits)

- **No people / faces / hands** by default. Anonymous, object-led, diagram-led.
- **No fake screenshots of real competitor UIs.**
- **No neon / RGB / cyberpunk styling.** Volt is a *single accent on a single element*, never a glow spread or a saturated background.
- **No "AI brand" clichés:** glowing brains, Matrix code, abstract networks of dots, futuristic floating UIs with readable numbers.
- **No flat 2D illustration.** Always 3D-rendered or photographic for hero slides; thin-stroke linework for diagrams.
- **No two-accent palettes.** One volt accent per slide. Period.
- **No volt as a background.** Volt is punctuation; cream and dark are the surfaces.

---

## Visual decisions to revisit

- Confirm mono typeface (recommended IBM Plex Mono or JetBrains Mono).
- Decide whether to include the WeAura wordmark on every slide or only slide 1 + last (current proposal: lockup on slide 1 + slide N; mark-only on intermediate slides if used at all).
- Decide on a recurring "incident-response" visual signature (proposal: a brass aperture closing, lit by single volt highlight).
- Confirm whether `surreal-product-metaphor` is used for hero slides or reserved for major launches only.

---

## Refresh log

| Date | What changed |
|---|---|
| 2026-05-06 | Updated with confirmed brand colors (VOLT `#CCFF00`, CREAM `#F2EFE8`, DARK `#0B0B0A`, LIGHT `#FAF8F3`) and Manrope typography from the official Logo Pack v2.0 (`brands/weaura-ai/README.md`). Aperture mark identified as the brand symbol. |
| 2026-05-06 | Initial visual-style derived from public-site signal at https://weaura.ai. |

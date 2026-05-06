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

## Layout reference template — `brandsdecoded__` style

> ⚠️ Pending screenshot capture. The user has indicated WeAura's carousels should follow the layout pattern of `@brandsdecoded__` on Instagram (Brazilian AI Content Agency, 287K followers, "Decodificando o futuro do marketing com AI"). Until 5+ post screenshots are pasted into the brand-pack-update flow, this section captures only the ask, not the executed template.

**What to capture from screenshots when available:**
- Slide background pattern (solid dark / cream / split / framed?).
- Headline placement (top / center / bottom; alignment).
- Headline weight + case (heavy / medium / light; UPPERCASE / sentence / Title).
- Slide-number indicator style (top / corner / footer / hidden).
- Frame / border treatment (full-bleed / inset card with rounded corners / hard frame).
- Use of accent on the slide number, headline highlight word, or a single graphic.
- Hook formula on slide 1 (length, number prefix, italic emphasis).
- Slide-2 transition style (does the layout shift, or is each slide visually identical with only text changing?).
- Brand mark placement (every slide / first+last only / hidden).
- Photography vs object-still-life vs pure typography.
- Density of text per slide (one big sentence vs short paragraphs).

**Adapt rule (do not clone):**
We adopt **layout pattern** (composition, density, slide rhythm) from brandsdecoded — we do **not** adopt their visual identity (colors, fonts, logo). WeAura keeps its own VOLT/CREAM/DARK palette, Manrope typography, and Aperture mark. Cloning brandsdecoded's exact identity would be brand confusion and a trademark concern.

When the screenshots arrive, this section gets replaced with the extracted template + a new visual mode entry (e.g. `agency-decoded` or `editorial-frame`) added to the modes table above.

---

## Visual mode

- **Primary mode:** `editorial-minimal` (cream-on-dark or cream-on-light backgrounds, restrained typography in the bottom-third).
- **Secondary mode:** `technical-blueprint` (deep dark backgrounds, thin volt linework for engineering-essay diagrams).
- **Tertiary (hero only):** `surreal-product-metaphor` (a single luminous object — aperture-like, lit with a single volt highlight — for slide-1 heroes that demand drama).
- **Why these modes fit:** the audience is senior engineers who distrust over-styled tech marketing. Editorial-minimal communicates restraint; technical-blueprint gives engineering essays the right register; the volt accent gives the brand a recognizable signature without the cyberpunk-RGB trap.

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

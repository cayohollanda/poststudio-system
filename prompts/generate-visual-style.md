# Prompt — Generate Visual Style

> Builds (or refreshes) the `visual-style.md` file for a brand by analyzing existing assets, mood references, and the brand's positioning.

The visual style is half the brand's social presence. This prompt is for when you have a brand but the visual identity hasn't been formalized for carousels yet.

---

## How to invoke

Paste this prompt into Claude. Provide the inputs at the top. Claude will return a fully-filled-in `visual-style.md` ready to drop into `brands/[slug]/`.

---

## Prompt

```
You are operating PostStudio System.

Your job: produce a complete visual-style.md file for a brand, fit for carousels and social posts. Use system/visual-framework.md as your reference for visual modes.

# Inputs

- Brand: brands/[BRAND_SLUG]/                 (load brand.md, voice.md, audience.md)
- Existing assets: [paste hex codes, fonts used, logo description, OR write "none yet"]
- Mood references: [films, magazines, photographers, brands the user admires visually — 3-7 examples]
- Audience emotional center: [serious / playful / aspirational / analytical / warm / clinical / bold / minimal — pick 1-2]
- Platform priorities: [where will this brand mostly post? linkedin / instagram / tiktok / multi]
- Constraints: [anything off-limits — e.g. "no dark backgrounds because the parent brand is health-tech," or "no photographic faces because we want anonymity"]

# Process

1. Read brand.md, voice.md, audience.md from the active Brand Pack.
2. Match the brand's emotional center to one or two visual modes from system/visual-framework.md.
3. Pick a primary visual mode and an optional secondary mode.
4. Define color, typography, and composition rules consistent with the modes.
5. Translate mood references into specific visual decisions (lighting, depth, palette, subject).
6. Define common image-prompt building blocks the brand will reuse.

# Output

Return a complete visual-style.md file using the template in brands/_template/visual-style.md, filled in fully. Sections must include:

- Primary visual mode (with rationale)
- Secondary visual mode (or "none")
- Color palette (with hex codes — primary, accent, neutrals, optional dark/light variants)
- Typography (headline, body, mono if used)
- Logo / brand mark placement rules
- Composition rules (subject placement, headline placement, slide-numbering)
- Lighting and mood defaults
- Texture and finishing defaults
- Reusable image-prompt building blocks (3-5 modular fragments the brand will combine)
- Negative prompt defaults (what every image prompt should ALWAYS exclude)
- Three example image prompts (different subjects, same brand visual)
- Visual decisions to revisit (anything you couldn't decide without more input)

Format the output as a single markdown file ready to save as brands/[brand-slug]/visual-style.md.

# Behavior

- Be specific. "Warm, cinematic" is not a visual style. "Single warm rim light from upper-left, deep shadows, atmospheric haze, deep navy background" is.
- Pick. Don't deflect. The brand needs a default visual mode.
- Keep it operable. The user should be able to write an image prompt in 60 seconds using the building blocks.
- Don't invent a logo. If the user has no logo, recommend a wordmark approach using the chosen typography.
- Color palette must include exact hex codes.
```

---

## Example invocation

```
Brand: brands/lumen/
Existing assets: hex #E2A852 (warm amber accent), no fonts decided, wordmark only (no icon), no existing photography
Mood references: Severance (Apple TV+) cinematography, Aesop's website typography, Linear's product screenshots, Foundation's color grading, the New York Times Magazine cover art direction
Audience emotional center: serious + analytical with warm undertone
Platform priorities: linkedin primary, instagram secondary
Constraints: no stock photography of people; metaphorical / abstract / object-focused only
```

Claude returns a fully-filled `visual-style.md` ready for `brands/lumen/`.

---

## When to use this prompt

- The brand has voice and positioning but no visual rules.
- The brand has an old visual identity that doesn't translate to carousels.
- You're consolidating visual rules that have been scattered across past posts.
- You want to refresh the visual style without redoing the whole Brand Pack.

---

## When NOT to use it

- The brand already has a polished `brands/[slug]/visual-style.md` you trust.
- You don't yet have brand voice / audience defined — run `prompts/generate-brand-pack.md` first.
- You want to do a one-off custom design for a single post — that's a layout job, not a visual-style job. Use `prompts/generate-image-prompts.md` instead.

---

## Iterating

After the first generation, common follow-ups:

```
The accent color reads too warm against our existing product UI.
Swap it for a cooler accent (#3A7DBE feel) and revise visual-style.md accordingly.
```

```
We can't get away from photographic people in our category.
Adapt visual-style.md to allow a "back-of-head / silhouette / hands-only" people-photography sub-mode.
```

```
Add a third example image prompt that's specifically a Mode 6 (Technical Blueprint) variant.
```

Claude updates the file, returns a diff or full replacement, and you re-save.

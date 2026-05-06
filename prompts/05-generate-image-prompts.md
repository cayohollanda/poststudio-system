# Prompt — Generate Image Prompts

> Refines image prompts for a specific image generator (Midjourney, ChatGPT Images, Flux, Leonardo, Ideogram) using the brand's visual style.

The image prompts inside a generated carousel are *good*. This prompt makes them *generator-specific* and *better*.

---

## How to invoke

Paste this prompt into Claude after you have a carousel output. Specify the image generator. Claude returns a refined prompt per slide, ready to paste into your tool of choice.

---

## Prompt

```
You are operating PostStudio System.

Your job: take an existing carousel's image prompts and refine them for a specific image generator, using the active Brand Pack's visual style.

# Inputs

- Brand: brands/[BRAND_SLUG]/                 (load visual-style.md primarily)
- Carousel: [paste the carousel block from a previous generation, OR reference a saved file]
- Image generator: [midjourney | chatgpt-images | flux | leonardo | ideogram | sora-images]
- Aspect ratio: [4:5 | 1:1 | 16:9 | 9:16]
- Quality bias: [draft | balanced | hero]   # how much to optimize per prompt

# Process

1. Read brands/[slug]/visual-style.md to lock the visual mode, color palette, typography reservation, and negative prompts.
2. For each slide in the carousel, refine the image prompt to:
   - Match the visual mode and accent color exactly.
   - Use the syntax conventions of the chosen image generator.
   - Reserve typography space (state placement explicitly).
   - Negate generator-typical pitfalls (text artifacts, broken hands, generic stock composition).
   - Hit the aspect ratio in the prompt (e.g. --ar 4:5 for Midjourney; aspect ratio note for ChatGPT Images).
   - Match the requested quality bias (draft = simpler, balanced = standard, hero = maximally detailed for slide 1 only).
3. Add a "watch out for" note per slide identifying the most likely failure mode and how the user should iterate if the first generation is weak.

# Output

Return a single markdown block per slide, in this format:

## Slide N — [Role]

**Refined prompt:**
[the prompt, generator-specific syntax included]

**Generator settings:**
- Aspect ratio: [...]
- Style / model: [Midjourney style raw / DALL-E natural / Flux schnell / etc]
- Optional flags: [--style raw, --no logo, etc]

**Reserved typography space:**
[where the user should leave room for headline, e.g. "bottom third"]

**Watch out for:**
[1-2 sentences naming the most likely AI artifact or weak result, and how to iterate]

# Generator-specific syntax tips

When refining for Midjourney, use:
- `--ar [ratio]`
- `--style raw` for editorial / cinematic looks
- `--s [low value]` for less stylization on Mode 2 (Editorial Minimal) or Mode 6 (Technical Blueprint)
- `--no [list]` for negative prompts
- Aspect ratios: --ar 4:5 (1080x1350), --ar 1:1 (square), --ar 16:9

When refining for ChatGPT Images / DALL-E:
- Natural language; avoid "--" flags
- State aspect ratio in plain words ("portrait 4:5 composition")
- Negative prompts go in plain words ("no text, no logos, no watermarks")
- Be more explicit with composition direction

When refining for Flux:
- Long-form natural prompts work best
- Lighting and depth descriptions are well-handled
- Avoid asking for readable text

When refining for Leonardo / Ideogram:
- Ideogram is best for prompts that need readable typography (rare in PostStudio outputs)
- Leonardo handles photographic prompts well; weight description toward camera/lighting/subject

# Behavior

- Do not change the visual concept. Only refine the prompt.
- Do not mix generators in one output.
- For "hero" quality bias, lavish detail on slide 1 only; keep slides 2-N consistent but lighter.
- If the visual style file says "no people" and a slide's image prompt implies people, adapt the prompt to abstract or object-based subject and flag it.
```

---

## Example invocation

```
Brand: brands/lumen/
Carousel: [pasted from a generate-carousel.md output]
Image generator: midjourney
Aspect ratio: 4:5
Quality bias: balanced
```

Claude returns 8 refined prompts (one per slide) with Midjourney-specific syntax (`--ar 4:5 --style raw --no text logo watermark`), reserved typography zones, and a per-slide "watch out for" note.

---

## Recommended generator → mode pairings

| Visual mode | Best generator | Why |
|---|---|---|
| 1 — Cinematic Dark | Midjourney `--style raw`, Flux | Both handle dramatic lighting and atmospheric haze well |
| 2 — Editorial Minimal | Flux, ChatGPT Images | Soft natural light and clean composition |
| 3 — Futuristic Interface | Midjourney, Flux | 3D and bloom render well; avoid cartoon defaults |
| 4 — Surreal Product Metaphor | Midjourney, Flux | Hyperreal materials require strong base |
| 5 — Premium Founder/Operator | Midjourney `--style raw`, Flux | Cinematic 35mm look |
| 6 — Technical Blueprint | Midjourney with `--s low`, Flux | Restrained line art needs low stylization |
| 7 — Exploded Diagram | Midjourney, Flux | 3D render with material detail |
| 8 — Meme-but-Premium | Midjourney `--style raw`, Flux | Photoreal first, meme structure second |

If your generator is forced (e.g. "we only have ChatGPT Images access"), Claude will adapt — but expect 2-3x more iterations on Modes 1, 4, 7.

---

## Iteration patterns

After running the prompt and rendering images, common follow-ups:

```
Slide 1 came out too dark. The subject is invisible at thumbnail size.
Refine the prompt to brighten the subject's rim light and increase contrast at the silhouette level.
```

```
Slide 4's image is generating with garbage text on the surface.
Strengthen the negative prompt and rephrase to make the subject text-free by design.
```

```
The whole set has inconsistent perspective.
Lock perspective: every slide is a 50mm lens, eye-level, slight low angle.
```

Re-run with the modification and Claude returns updated prompts only for the affected slides.

---

## Hero slide treatment

For slide 1, you can request "hero" quality bias to get a more elaborate prompt:

- More specific lighting direction (number, temperature, angle of each light).
- More specific depth-of-field language.
- More specific texture detail.
- Camera reference (lens, film stock, aperture).

This typically buys 20-40% better slide 1 results — important because slide 1 is the thumbnail.

For slides 2-N, "balanced" is enough.

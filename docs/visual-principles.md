# Visual Principles

> The philosophy under PostStudio System's visual layer. The 8 visual modes in `system/visual-framework.md` are the *what*. This file is the *why*.

These principles are timeless. The modes will evolve; the principles won't.

---

## 1. The thumbnail is the slide

The first slide is seen as a thumbnail in a feed before it's seen full-screen. Design for the thumbnail.

A thumbnail-friendly slide:

- Has one dominant subject the eye lands on instantly.
- Reads at 5% of full size (open the slide, zoom out, squint).
- Has high contrast at the silhouette level.
- Is not a busy collage of small elements.

If your slide doesn't survive thumbnail size, redesign it.

---

## 2. Cinematic, not stocky

The visual instinct most carousels lean on is "stock photo with text on top." That's table-stakes. PostStudio aims higher: cinematic, editorial, surreal, or photographic in a way that *says* something.

The difference:

- **Stock photo:** smiling team at laptop, glass office, perfect daylight, dead-center subject.
- **Cinematic:** a single figure at a desk, late-light shadows raking across the wall, deep blacks, atmospheric haze.

Both can illustrate "remote work." Only one stops the scroll.

---

## 3. Depth over flatness

Flat illustrations are the cheapest visual default and look identical across thousands of brands.

Depth differentiates:

- Foreground / midground / background.
- Soft focus that falls off.
- Atmospheric haze on distant elements.
- Light that wraps around subjects.
- Subjects that recede into shadow.

Depth says "considered." Flatness says "template."

---

## 4. One accent, used selectively

Brand accents work when they're rare. Used everywhere, they vanish.

Apply the accent to:

- A single edge glow on the subject (Mode 3 / Futuristic Interface).
- A highlighted node in a diagram (Mode 6 / Technical Blueprint).
- One word in the headline.
- The slide number.
- A single contrast detail in a mostly-monochrome composition.

Don't apply the accent to: every slide's background, every headline, every stroke. Restraint is what gives the accent its power.

---

## 5. Visual metaphor over decoration

A pattern of dots is decoration. A glass dome over a city is metaphor.

Decoration says "here is some shape." Metaphor says "this is what the post is about."

Reach for metaphor first. Examples:

- "AI tools fragment your focus" → a fractured dome over a desk.
- "Most onboarding flows are too long" → a stretched, unraveling tape measure.
- "Build in public is a long compounding bet" → a tree growing slowly through cracked concrete.

If you can't think of a metaphor, you don't have a strong enough thesis. Sharpen the thesis first; the metaphor follows.

---

## 6. Headline lives in negative space

Never put the headline on top of the subject's face, the busiest part of an image, or a high-contrast edge.

Standard placements:

- Bottom-third — the default; the subject occupies the upper two-thirds.
- Top-left — works when the subject is on the right.
- Right-side — works when the subject is on the left.
- Center on monochrome — works when the visual is a clean texture.

Reserve the typography zone in the image prompt itself. State it explicitly: *"leave the bottom third empty for headline."*

---

## 7. Consistency is the deliverable

A 7-slide post is not 7 designs. It's one design, repeated 7 times with different content.

Consistent across slides:

- Visual mode.
- Accent color.
- Typography pairing.
- Margins and padding.
- Brand mark placement.
- Slide-number style.

When slide 4 looks like a different post, the carousel collapses. The reader feels something is wrong even if they can't name it.

---

## 8. Restraint over richness

Most weak carousels suffer from too much, not too little.

The instinct: add more elements, more textures, more colors, more icons.
The fix: subtract until only what's load-bearing remains.

Test: cover any element with your hand. If the slide is *worse*, the element earns its place. If the slide is the same, kill it.

---

## 9. AI-typical pitfalls are predictable

AI image generators have signature failures. Watch for:

- Six-fingered hands.
- Asymmetric eyes.
- Garbled text.
- "Plastic skin" lighting.
- Floating objects with no shadow grounding.
- Inconsistent perspective on architecture.
- Logos that look almost-but-not-quite real.

Avoid in prompts: explicitly negate ("no text, no logos, no garbled letters, no AI-typical hand anatomy"). Hide the failure: keep hands out of frame, prefer back-of-head shots, avoid frontal eye contact unless deliberate.

---

## 10. Brand mark is small or absent

A logo splash on every slide reads as advertising. A small brand mark — 12-16px in a corner, on slide 1 and the last slide only — reads as identity.

Default: brand mark on slide 1 and slide N. Optional on slide 2 if it carries continuation. Otherwise, hide it.

The brand is communicated through voice, accent, and visual mode — not through repeated logos.

---

## 11. Typography is brand

Carousels are read at high speed. The typeface tells the reader what brand they're looking at before the headline does.

Pick *one* primary headline typeface and *one* support typeface. Use them consistently across every carousel. Save them in `brands/[slug]/visual-style.md`.

Fight the urge to "be creative" with typography per post. The Brand Pack's typography is the brand's signature. Brand recognition compounds when typography stays the same.

---

## 12. Generate without text, layer text after

Most image generators put garbage text on slides. Even when they get the spelling right, the kerning, hierarchy, and color rarely match brand standards.

Default workflow:

1. Generate the image with **no text overlay**.
2. Bring the image into Figma / Canva / Paper.design / Photoshop.
3. Layer the headline, accent, slide number, and brand mark.

The exception: meme structures (Mode 8) where a single readable word carries the joke. Even then, expect to retry several times for clean text.

---

## 13. Aspect ratio matters

Different platforms reward different aspect ratios:

| Platform | Aspect ratio | Notes |
|---|---|---|
| LinkedIn (carousel doc) | 4:5 (1080×1350) | Tall format, fills more of the feed. |
| LinkedIn (image carousel) | 4:5 or 1:1 | Carousels via document upload look more polished. |
| Instagram | 4:5 or 1:1 | 4:5 owns more screen real estate. |
| TikTok carousels | 4:5 (1080×1350) | Vertical-leaning. |
| X (Twitter) | 16:9 or 1:1 | Native carousels limited; treat each tweet as a slide. |
| Threads | 4:5 or 1:1 | Mirror Instagram. |

Generate at the platform's preferred ratio from day one. Cropping a 16:9 image to 4:5 often kills the composition.

---

## 14. Slide 1 ≠ slide 2-N

Slide 1 is the most expensive slide to design. It's the only one that must work as a feed thumbnail, a swipe-stop, and a recognizable brand artifact simultaneously.

Treat slide 1 as a *poster*. Slides 2-N are *frames within the same film*.

Spend disproportionate time on slide 1. Iterate it 5-10 times if needed. The other slides will design themselves once slide 1 is right.

---

## 15. The visual must serve the argument

Visuals do three jobs:

1. **Stop the scroll** (slide 1).
2. **Carry the argument** (slides 2-N).
3. **Earn the trust to ask for action** (final slide).

A beautiful image that doesn't serve the argument is decoration. A serviceable image that crystallizes the argument is gold.

When in doubt, choose the image that *says something*.

---

## How to debug a weak visual

A debugging checklist for the moment a slide doesn't feel right:

- Is there a single dominant subject? If not, simplify.
- Is the subject in the center of the canvas? Try off-center, cropped, or rule-of-thirds.
- Is the headline competing with the image? Move the headline to negative space.
- Is the contrast strong enough at thumbnail size? If not, deepen blacks or brighten whites.
- Is the metaphor literal or surprising? If literal, push further.
- Is the lighting flat? Add a single dramatic key light.
- Is the perspective static? Try lower angle, higher angle, or off-axis.
- Is the texture overcooked? Strip detail until the subject reads.
- Does the visual mode match the post's emotional center? If not, switch modes.

When five of these are off, restart. A good visual often comes faster than a salvaged bad one.

# Playbook — Single Carousel Workflow

> Mode 1. From brief to published carousel. ~60-90 minutes once the Brand Pack exists.

---

## Pre-flight

- [ ] Brand Pack exists at `brands/[slug]/` and is current (≤ 90 days).
- [ ] Topic identified.
- [ ] Audience clarified.
- [ ] Goal set (save / comment / DM / etc.).
- [ ] Strongest signal identified (proof / opinion / story).

If any are missing, run [`brand-onboarding-workflow.md`](brand-onboarding-workflow.md) or refine the brief first.

---

## The 9 stages

```
Brief → Generate → Critique → Apply fix → Image prompts → Visuals → Layout → Caption + post → Capture proof
```

---

## Stage 1 — Brief (5-15 min)

Tight 4-line brief:

```
Topic:    [one sentence]
Audience: [role + situation]
Goal:     [save | comment | share | DM | click | follow | try]
Signal:   [strongest evidence — proof, opinion, story]
```

Optional:
```
Angle:        [from system/content-system.md, or auto]
Structure:    [from system/carousel-framework.md, or auto]
Visual mode:  [from system/visual-framework.md, or auto]
Slide count:  [7-10, default 8]
```

---

## Stage 2 — Generate (~5 min)

In the Claude Project:

```
Use brands/[slug]/ and prompts/04-generate-carousel.md.

Brand:       [slug]
Topic:       [...]
Audience:    [...]
Platform:    [...]
Language:    [...]
Goal:        [...]
Signal:      [...]
[optional fields]
```

Claude returns: Strategic Angle + slides + caption + CTA + first comment + 5 alt hooks + Quality Checklist + Missing Inputs.

---

## Stage 3 — Critique (~5 min)

In a *new chat* in the same Project (separate context):

```
Run prompts/08-critique-output.md on the carousel below. Be adversarial.

[paste carousel]
```

Verdict:
- **Ship-ready** → continue to Stage 5.
- **Ship with flags** → review flags; continue if acceptable.
- **Borderline / Rewrite required** → Stage 4.

---

## Stage 4 — Apply fix (~5-10 min)

In another new chat:

```
Use prompts/09-improve-output.md.

Brand:        [slug]
Module:       carousel-machine
Original:     [paste]
Fix to apply: [highest-leverage fix from the critique]
```

Re-run critique. Loop max 2 times.

---

## Stage 5 — Image prompts (~10 min)

Refine per-slide prompts for your generator:

```
Use prompts/05-generate-image-prompts.md.

Brand:           brands/[slug]/
Carousel:        [paste]
Image generator: midjourney
Aspect ratio:    4:5
Quality bias:    balanced
```

For slide 1, optionally re-run with `quality bias: hero`.

---

## Stage 6 — Render visuals (~30-60 min)

For each slide:
1. Run the prompt in your generator (Midjourney / Flux / ChatGPT Images / etc.).
2. Generate 3-4 variations; pick the strongest.
3. Iterate the prompt if needed (specificity, lighting, composition).
4. Generate WITHOUT text whenever possible.

Visual mode consistent across all slides.

---

## Stage 7 — Layout (~30-60 min)

In Figma / Canva / Paper.design / Photoshop:

1. Set canvas to platform spec (LinkedIn / IG: 1080×1350).
2. Drop in each rendered image.
3. Apply per-slide layout instruction (headline placement, accent, typography).
4. Use Brand Pack typography from `visual-style.md`.
5. Margins consistent.
6. Brand mark on slide 1 + slide N.
7. Slide numbers if Brand Pack uses them.

Open as flipbook; confirm it hangs together.

---

## Stage 8 — Caption + post (~10 min)

1. Read caption out loud. Stumble = rewrite.
2. Confirm CTA is one specific action.
3. Trim caption to platform sweet spot.
4. Schedule or post.
5. Within 5 min of posting, drop the **first comment** with the value-stacked content.

---

## Stage 9 — Capture proof (~5-10 min, next day)

Within 24-72 hours:
1. Save strong reactions to `brands/[slug]/proof-assets.md`.
2. Save audience-adopted vocabulary to `voice.md`.
3. Note what worked / didn't in `examples.md` / `rejected-examples.md`.
4. If a pattern emerges, update the relevant Brand Pack file.

---

## Time budget

| Stage | Time |
|---|---|
| 1. Brief | 5-15 min |
| 2. Generate | 5 min |
| 3. Critique | 5 min |
| 4. Apply fix | 5-10 min |
| 5. Image prompts | 10 min |
| 6. Visuals | 30-60 min |
| 7. Layout | 30-60 min |
| 8. Caption + post | 10 min |
| 9. Capture proof | 5-10 min (next day) |
| **Total** | **~2-3 hours** |

After 3-5 cycles, the time drops to 60-90 minutes (Brand Pack mature, visual templates locked).

---

## Common shortcuts

- **Skip Stage 4** if Stage 3 returned Ship-ready. Don't over-edit.
- **Skip Stage 5** if Stage 2's prompts are already good (sometimes they are).
- **Skip Stage 9** for low-stakes posts (capture only when meaningful).

Never skip Stages 1, 3, 8.

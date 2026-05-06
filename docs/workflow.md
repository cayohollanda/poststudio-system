# Workflow — End-to-End

> The full pipeline from brief to ship to learning. Use this as a checklist for any non-trivial post.

The workflow has 9 stages. Skipping stages is allowed. Skipping stages silently is how brands drift.

---

## Stage 0 — Inputs

Before you begin, confirm you have:

- **Topic** — what's the post about, in one sentence?
- **Audience** — who specifically should this hit?
- **Platform** — LinkedIn / Instagram / X / TikTok / multi-channel?
- **Language** — output language (and any localization notes)?
- **Goal** — saves / comments / DMs / clicks / awareness?
- **Time budget** — when does this need to ship?
- **Signal** — what real proof, data, story, or opinion will anchor the post?

If three or more are unclear, run **Stage 1** as a brief refinement before generating anything.

---

## Stage 1 — Brief

Two paths:

### 1a. Quick brief (you have clarity)

Write a 3-line brief in your own notes:

```
Topic: [one sentence]
Audience: [role + situation]
Goal: [one verb: save / comment / DM / click]
Signal: [the strongest hook material — number, opinion, story]
```

### 1b. Refined brief (you don't have clarity)

In Claude:

```
I'm planning a carousel about "[topic]" for brands/[slug]/.
Help me sharpen the brief. Ask me up to 5 questions to surface:
- the audience segment
- the goal
- the strongest available signal
- the angle from system/content-system.md
- the structure from system/carousel-framework.md
```

Claude will surface gaps. Answer them. Now your brief is ready.

---

## Stage 2 — Brand Pack check

Confirm the Brand Pack:

- Is active in this session — either supplied via `prompts/19-ephemeral-brand-intake.md` (runtime intake) OR uploaded as a private `brands/[slug]/` folder into your Claude Project Knowledge (12 files).
- Was updated in the last 90 days (Brand Packs decay).
- Includes recent proof (claims, customers, results) in `proof-assets.md`.
- Includes recent words-to-avoid (vocabulary you've discovered the audience hates) in `voice.md`.

If any are stale, refresh them before generating.

If the brand has no Brand Pack at all, run `prompts/19-ephemeral-brand-intake.md` (fast path) or `prompts/02-generate-brand-pack.md` (interview path). Don't skip this. A weak Brand Pack is the #1 cause of weak output. **Do not commit real brand packs into this framework repo** — they live in your private storage and are uploaded into Project Knowledge per session.

---

## Stage 3 — Generate

Send to Claude:

```
Use brands/[slug]/.
Topic: [topic from brief]
Audience: [audience from brief]
Platform: [platform]
Language: [language]
Goal: [goal]
Signal: [signal]
[optional] Angle: [angle name from system/content-system.md]
[optional] Structure: [default / launch / educational / comparison / benchmark / thought-leadership / product-led / project-announcement]
[optional] Visual mode: [mode from system/visual-framework.md]

Use prompts/generate-carousel.md.
```

Claude returns the full output contract.

**Checkpoint:** if Claude asks clarifying questions, answer them. Don't say "just go" — every answered question buys you 10% sharper output.

---

## Stage 4 — Critique

Run a separate reviewer pass:

```
Run prompts/critique-carousel.md on the carousel above. Be adversarial.
```

Claude returns:

- A pass/fail/risk grade per checklist item.
- The single highest-leverage fix.
- 2-3 concrete rewrites for the weakest slide.
- Any safety / claims violations.

**Decision point:**
- If 0-1 critical fails and ≥25/30 score → continue.
- If 2+ critical fails or <22/30 score → rewrite from Stage 3 with the critique applied.

---

## Stage 5 — Image prompts

Take the image prompts from the carousel. Improve them with:

```
Use prompts/generate-image-prompts.md.
For each slide, refine the image prompt to:
- match visual mode [mode]
- target image generator [Midjourney / Flux / ChatGPT Images / Leonardo / Ideogram]
- avoid AI-typical pitfalls (text artifacts, broken hands, generic stock composition)
- explicitly reserve typography space at [bottom-third / top-left / etc]
```

Save the refined prompts. You'll iterate on the image generator next.

---

## Stage 6 — Visuals

Render every slide:

1. Run the prompt in your image generator.
2. Generate 3-4 variations per slide.
3. Pick the strongest (look for: subject clarity, depth, lighting drama, no AI artifacts).
4. Iterate the prompt if the output is weak (more specific subject, more specific lighting, clearer composition direction).
5. Generate **without text** whenever possible. Add text in Stage 7.

**Checkpoint:** does the visual mode hold across all slides? If slide 5's image looks like it's from a different brand than slide 1, regenerate.

---

## Stage 7 — Layout

In Figma / Canva / Paper.design / Photoshop:

1. Set up a slide template at the platform's recommended size:
   - LinkedIn: 1080×1350 (4:5)
   - Instagram: 1080×1350 (4:5) or 1080×1080 (1:1)
   - TikTok carousels: 1080×1350 (4:5)
   - X (Twitter): 1600×900 (16:9) or 1080×1080 (1:1)
2. Drop in each rendered image.
3. Apply the slide's **layout instruction** verbatim (headline placement, accent color, typography rule).
4. Use the Brand Pack's typography (from `visual-style.md`).
5. Confirm consistent margins across all slides.
6. Add the brand mark (small) on slide 1 and slide N. Optional on others.
7. Add slide numbers in the top-right if your visual mode uses them.

**Checkpoint:** open the slides as a flipbook. Do they hang together as one unit? If slide 4 feels like a different post, fix the layout or the image.

---

## Stage 8 — Caption, CTA, first comment

Take the caption, CTA, and first comment from the carousel output.

Final pass:

1. Read the caption out loud. Stumble = rewrite.
2. Confirm the CTA is one specific action.
3. Trim the caption if it's > the platform's sweet spot (LinkedIn 250 / Instagram 180 / X 280-char threads / TikTok 140).
4. Schedule or post.
5. Within 5 minutes of posting, drop the **first comment** with the value-stacked content (link, bonus, contrarian footnote, receipt).

---

## Stage 9 — Capture proof

The post is live. The work isn't over.

Within 24-72 hours:

- **Capture wins.** Strong comments, DMs, customers reaching out → save the screenshot or quote into `brands/[slug]/proof-assets.md`.
- **Capture losses.** A claim that drew pushback, a slide that confused people, a CTA that landed flat → note it in `brands/[slug]/proof-assets.md` under "what didn't work."
- **Capture vocabulary.** Words the audience used in comments. Update `brands/[slug]/voice.md` with what to use and what to avoid.

Future posts get sharper because the system gets sharper. The Brand Pack is a living document.

---

## Time budget

A typical pipeline using PostStudio System:

| Stage | Time |
|---|---|
| 0. Inputs | 5 min |
| 1. Brief | 5-15 min |
| 2. Brand Pack check | 2-10 min |
| 3. Generate | 5 min |
| 4. Critique | 5 min |
| 5. Image prompts | 10 min |
| 6. Visuals | 30-90 min |
| 7. Layout | 30-60 min |
| 8. Caption + post | 10 min |
| 9. Capture proof | 5-10 min (next day) |

Total: **~2-3.5 hours** for a high-quality 8-slide carousel from cold start.

After 3-5 carousels per brand, the Brand Pack matures and the time drops to 60-90 minutes.

---

## Checkpoints summary

A condensed version you can paste at the top of any session:

```
0. Inputs ready: topic, audience, platform, language, goal, signal
1. Brief refined (or skip if you have clarity)
2. Brand Pack loaded and ≤90 days old
3. Generated → returned full output contract
4. Critique pass run → no critical fails, score ≥25/30
5. Image prompts refined for chosen image generator
6. Images rendered, visual mode consistent across slides
7. Layout applied, margins consistent, brand mark placed
8. Caption read out loud, CTA is one action, first comment ready
9. Proof captured within 72 hours into Brand Pack
```

If a checkpoint is skipped, mark it skipped and own the trade-off.

---

## When to break the workflow

Three legitimate reasons to skip stages:

1. **Time-sensitive reactions.** A breaking moment in your category — skip Stage 4 critique, ship faster.
2. **Series posts.** Once a series template is established, you can skip Stages 5-7 by reusing the visual template.
3. **Personal voice posts.** Founder essays where the voice *is* the brand and over-engineering removes warmth.

Otherwise, run the full workflow. The cost of one weak post is higher than the cost of an extra hour.

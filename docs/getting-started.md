# Getting Started

> A 10-minute on-ramp for using PostStudio System to produce your first carousel.

This guide assumes you know what a Claude Project is and have access to one. If you don't, you can use any LLM with a long context window — the workflow is the same; only the upload mechanism differs.

---

## Prerequisites

- Claude (or another long-context LLM)
- A topic you want a carousel about
- (Optional) a brand. If you don't have one, the system will help you build one.
- A design tool for layout (Figma, Canva, Paper.design, Photoshop)
- An image generator (Midjourney, ChatGPT Images, Flux, Leonardo, Ideogram)

---

## The 5-step on-ramp

### Step 1 — Get the repository

```bash
git clone https://github.com/yourname/poststudio-system.git
# or
gh repo clone yourname/poststudio-system
```

You don't need to run anything. The whole "OS" is text.

### Step 2 — Open a Claude Project

1. Create a new Claude Project (or any equivalent in your tool).
2. Upload the entire `poststudio-system/` folder as Project Knowledge.
3. In the Project's custom instructions, paste:

```
You are operating PostStudio System.

Before any task, read system/master-instructions.md.

When the user names a brand, treat brands/[brand-slug]/ as the active Brand Pack and read all 8 files.

Default to prompts/generate-carousel.md unless the user requests another prompt.

Always end output with the Quality Checklist and Missing Inputs / Proof Needed sections from the output contract.

Do not invent numbers, customers, awards, or quotes. Mark missing proof with [PROOF_NEEDED: short description] and surface it.
```

### Step 3 — Pick a brand path

You have three:

#### A) You already have a Brand Pack

Place it under `brands/your-brand-slug/`. It must include all 8 files. See `brands/_template/`.

#### B) You have brand context but no Brand Pack yet

Run [`prompts/generate-brand-pack.md`](../prompts/generate-brand-pack.md) inside the Claude Project. It runs an interview (10 sections). At the end, Claude returns the 8 Brand Pack files. Save them under `brands/your-brand-slug/`.

#### C) You're prototyping without a brand

Use the `examples/example-brand-pack/` (the fictional "Lumen" brand) as a stand-in. Don't ship anything from a fictional Brand Pack as if it were a real brand.

### Step 4 — Generate your first carousel

In the chat, say:

```
Use brands/[your-brand-slug]/ and generate a carousel about "[your topic]"
for [platform], in [language], goal: [save / comment / DM / click].
```

Claude will return:

- Strategic Angle
- Slide-by-slide carousel (7-10 slides)
- Caption
- CTA
- First comment
- 5 alternative hooks
- Quality checklist scorecard
- Missing inputs / proof needed

### Step 5 — Render visuals and lay out

For each slide:

1. Take the **image prompt** and run it in Midjourney / Flux / ChatGPT Images / Leonardo / Ideogram.
2. (Recommended) generate the image **without text overlay** and add typography in Figma / Canva / Paper.
3. Apply the slide's **layout instruction** (headline placement, accent color, typography rule).
4. Keep the same visual mode and accent across all slides.

Then post.

---

## A real example session

You: *"Use brands/lumen/ and generate a carousel about 'why most AI tools fail at deep work' for LinkedIn, English, goal: save."*

Claude (abridged):

> **Strategic Angle**
> Contrarian + Mechanism. The default belief is "AI saves time on deep work." The reality is most AI tools *interrupt* deep work because they're built around chat. We argue for an interruption-light AI workflow.
>
> **Carousel**
>
> ## Slide 1 — Hook
> - Role: hook
> - Headline: Most AI tools quietly destroy your deep work.
> - Support text: And the chat is why.
> - Visual concept: a fractured glass dome over a desk, late-light shadows
> - Image prompt: "A fractured glass dome over a wooden desk, ..."
> - Layout instruction: headline bottom-left third, white serif on dark
>
> [...continues for slides 2-8, caption, CTA, first comment, alts, checklist]

You take that, render the visuals, lay them out, and ship.

---

## Frequently overlooked steps

These small choices compound across many posts:

- **Save your alternative hooks.** Tomorrow's post might use today's #2 alt.
- **Capture proof.** When a customer says something useful, paste it into `brands/[slug]/proof-assets.md`. Future carousels will get sharper because the system can pull it.
- **Update the Brand Pack as you learn.** "We discovered our audience hates the word 'transform'" → add to `voice.md`. Next post automatically respects it.
- **Track which angles work.** Keep a small log per brand. "Mistake angles outperform Contrarian angles 2x for this audience." That insight feeds the next prompt.

---

## What to do when output is weak

If a generated carousel doesn't feel right:

1. **Run `prompts/critique-carousel.md` on it.** A reviewer pass often surfaces the issue.
2. **Run `prompts/improve-hook.md`** specifically on slide 1.
3. **Switch the angle.** From `system/content-system.md`'s 17 angles, pick a different one and regenerate.
4. **Sharpen the brief.** "Generate a carousel about leadership" is too broad. "Generate a carousel about a specific leadership mistake we made on the [PROJECT]" is workable.
5. **Add proof.** Most weak carousels are weak because the prompt didn't include a real story or number.

---

## When to skip Claude

PostStudio System is meant to *amplify* your taste, not replace it. Skip the system when:

- The post is a personal anecdote that needs your voice raw, not stylized.
- The post is a one-line announcement that doesn't justify a carousel.
- The post is highly time-sensitive (e.g. a real-time reaction) and the brand voice is already in your fingertips.

The system is best for: launches, evergreen educational content, framework posts, comparisons, founder essays, build-in-public updates, and recurring content that benefits from consistency.

---

## Next reading

- [`docs/how-to-use-with-claude-projects.md`](how-to-use-with-claude-projects.md) — deeper on the Claude Project setup.
- [`docs/workflow.md`](workflow.md) — full end-to-end workflow with checkpoints.
- [`docs/content-principles.md`](content-principles.md) — the philosophy behind the system.
- [`docs/visual-principles.md`](visual-principles.md) — the philosophy behind the visuals.
- [`docs/output-contract.md`](output-contract.md) — formal output specification.

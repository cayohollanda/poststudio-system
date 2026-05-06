# Prompt — Generate Carousel

> The primary, workhorse prompt of PostStudio System. Use it for ~80% of carousel generation tasks.

This prompt assumes you have:

1. Read `system/master-instructions.md` and the rest of `system/`.
2. An active Brand Pack for the session — supplied via runtime intake (website + handle + attached logos + voice samples, processed by `prompts/19-ephemeral-brand-intake.md`) OR loaded from a `brands/[slug]/` folder the user uploaded into Project Knowledge.
3. A topic and goal in mind.

If any of those are missing, route to:

- No system loaded → tell the user to upload `system/` first.
- No brand context (no website / handle / voice samples / uploaded brand folder in this session) → run `prompts/19-ephemeral-brand-intake.md` first (fast intake), or `prompts/02-generate-brand-pack.md` for a full interview.
- No topic → ask up to 3 clarifying questions.

> **Note on brand source.** The PostStudio System framework is brand-agnostic. Real brand instances do **not** live in this repo's `brands/` folder (only `_template/` does). Brand context is supplied per session.

---

## How to invoke

Paste this prompt into Claude (or run it inside a Claude Project that already has PostStudio System loaded), filling in the variables at the top.

---

## Prompt

```
You are operating PostStudio System.

# Inputs

- Brand: [BRAND_SLUG] — load from session context (ephemeral intake) OR from a brands/[BRAND_SLUG]/ folder the user uploaded as Project Knowledge (all 12 files). NEVER from the framework repo's brands/ folder, which contains only _template/.
- Topic: [ONE-SENTENCE TOPIC]
- Audience: [SPECIFIC AUDIENCE — role + situation]
- Platform: [linkedin | instagram | tiktok | x | threads | multi]
- Language: [en | pt | es | fr | de | ...]
- Goal: [save | comment | share | click | dm | follow | try]
- Signal: [the strongest evidence material — number, opinion, story, comparison, testimonial — or "none, work from first principles"]

# Optional inputs

- Angle: [one of the 17 angles in system/content-system.md, or "auto"]
- Structure: [default | launch | educational | comparison | benchmark | thought-leadership | product-led | project-announcement, or "auto"]
- Visual mode: [one of the 8 modes in system/visual-framework.md, or "auto"]
- Slide count: [7 | 8 | 9 | 10, or "auto" — default is 8]
- Tone override: [if you want to override the Brand Pack's voice for this single post, describe it; otherwise leave blank]

# Process

1. Read the active Brand Pack in full.
2. Pick or honor the angle from system/content-system.md.
3. Pick or honor the structure from system/carousel-framework.md.
4. Pick or honor the visual mode from system/visual-framework.md.
5. Apply system/copywriting-rules.md to all copy.
6. Apply system/safety-and-claims-rules.md — never invent.
7. Self-review against system/quality-checklist.md.

# Output

Return the standard output contract from docs/output-contract.md:

- Strategic Angle
- Carousel (slides 1 through N)
- Caption
- CTA
- First Comment
- 5 Alternative Hooks
- Quality Checklist (with score N/30)
- Missing Inputs / Proof Needed

If you're missing critical inputs (signal, brand voice, audience), ask up to 3 questions before generating. Otherwise, proceed.

If a slide needs evidence you don't have, write [PROOF_NEEDED: short description] and surface it.

Do not invent numbers, customers, awards, or quotes.
Do not pad output. Do not add commentary outside the contract.
```

---

## Example invocations

### Example 1 — Specific brief

```
Brand: brands/lumen/
Topic: why most AI tools fail at deep work
Audience: senior engineers and PMs at 50-500 person SaaS companies who want fewer interruptions
Platform: linkedin
Language: en
Goal: save
Signal: opinion — most AI tools are chat-shaped, and chat is the opposite of deep work
Angle: auto
Structure: default
Visual mode: cinematic-dark
Slide count: 8
```

### Example 2 — Sparse brief, will trigger questions

```
Brand: brands/lumen/
Topic: how we ship faster
Audience: founders
Platform: linkedin
Language: en
Goal: save
Signal: none
```

(Claude will ask up to 3 clarifying questions before generating.)

### Example 3 — Launch carousel

```
Brand: brands/lumen/
Topic: launching the Focus Mode in v3.4 — replaces meeting reminders with a single morning brief
Audience: existing Lumen users + product-led growth ICPs
Platform: linkedin
Language: en
Goal: try
Signal: feature ships Monday; we tested it internally and the team's interrupted-deep-work-time dropped notably (no specific number cleared for use yet)
Angle: hidden feature + product-led
Structure: launch
Visual mode: futuristic-interface
Slide count: 10
```

### Example 4 — Multi-language

```
Brand: brands/lumen/
Topic: 5 mistakes founders make when adopting AI tools
Audience: early-stage founders, technical or non-technical
Platform: linkedin
Language: pt
Goal: save
Signal: founder-perspective opinion based on shipping Lumen for 2 years
Angle: mistake
Structure: default
Visual mode: editorial-minimal
Slide count: 8
Tone override: same Brand Pack voice but adapted to Portuguese (Brazilian); keep direct, lowercase-friendly, no LinkedIn-bro cadence.
```

---

## Behavior expectations

When you run this prompt, Claude should:

- **Honor every input.** If you specify `visual mode: editorial-minimal`, every slide's image prompt should reflect that mode.
- **Surface gaps.** If a slide needs proof you didn't provide, mark `[PROOF_NEEDED]` and list it in Missing Inputs.
- **Score honestly.** The Quality Checklist score should reflect actual quality, not be inflated to 30/30 reflexively.
- **Stay inside the contract.** No "Here's a fun carousel for you!" preamble. No closing pleasantries.

If Claude returns output that violates these expectations, run `prompts/critique-carousel.md` and ask it to revise.

---

## Common modifications

You can extend the prompt with:

- **A series anchor** — "This is post 2 of a 5-post series on [theme]. Reference series consistency."
- **A reuse note** — "Reuse the visual mode and accent from carousel-001."
- **An audience-specific frame** — "The hook should land specifically for [sub-segment]."
- **A constraint** — "Avoid the word [WORD] entirely in this post."
- **A precedent** — "Match the voice of carousel-001 in examples/carousel-archive/."

Add modifications under the `# Optional inputs` section of the prompt.

---

## When to use a different prompt

| If you want… | Use… |
|---|---|
| To improve a hook only | `prompts/improve-hook.md` |
| To critique a finished carousel | `prompts/critique-carousel.md` |
| To refine image prompts | `prompts/generate-image-prompts.md` |
| JSON output | `prompts/export-carousel-json.md` |
| To bootstrap a brand | `prompts/generate-brand-pack.md` |
| To define visual style | `prompts/generate-visual-style.md` |

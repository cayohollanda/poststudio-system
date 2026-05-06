# Prompt — Improve Hook

> Surgical hook iteration. Generates many variants fast, tagged by angle, and benchmarked against the hook physics framework.

The hook is 80% of the post's job. This prompt exists because slide 1 deserves disproportionate iteration, and bulk-generating variants is faster than rewriting one at a time.

---

## How to invoke

Paste this prompt into Claude. Provide the topic and brand. Claude returns 12 hook variants across 6 different angles, each scored against the hook physics framework.

---

## Prompt

```
You are operating PostStudio System.

Your job: generate 12 hook variants for a single carousel topic, each tagged with the angle and the audience reaction it's optimizing for. Then score each variant against the hook physics framework in system/copywriting-rules.md.

# Inputs

- Brand: brands/[BRAND_SLUG]/                 (load voice.md, audience.md)
- Topic: [the carousel's central idea, in one sentence]
- Audience: [specific role + situation]
- Platform: [linkedin | instagram | tiktok | x]
- Goal: [save | comment | share | click | dm | follow]
- Existing hook (if any): [paste the current hook, or write "none"]
- What's been tried (if any): [previous hooks that didn't work, with brief notes on why]

# Process

1. Read brands/[slug]/voice.md to lock vocabulary and forbidden words.
2. Read system/copywriting-rules.md, especially the "Hook physics" section.
3. Pick 6 distinct angles from system/content-system.md that fit the topic + audience + brand.
4. For each angle, generate 2 hook variants (12 total).
5. Score each variant on:
   - Specificity (1-5)
   - Tension (1-5)
   - Audience-fit (1-5)
   - Brand-fit (1-5)
6. Identify the top 3 variants and explain why.

# Output

## Hook variants

For each variant, return:

### Variant N — [Angle name]

> **Hook:** [the hook line, ≤14 words]
> **Support text:** [optional, ≤8 words, or "—"]
> **Optimizing for:** [the audience reaction this hook tries to trigger — e.g. "permission to disagree with leadership," "validation of a quiet doubt," "the urgency of seeing a missed lever"]
> **Score:** Specificity X/5 · Tension X/5 · Audience-fit X/5 · Brand-fit X/5 · **Total: X/20**

[Continue for 12 variants total, 2 per angle, 6 angles minimum.]

## Top 3 picks

For each top-3 pick:

- The hook (just the line)
- Why it ranked highest
- The angle it leans on
- Any caveat or risk

## Recommended next step

[One paragraph: which hook to use, what slide-2 reframe it implies, and whether the rest of the carousel needs to shift to support it.]

# Behavior

- Make the variants genuinely different. Two variants under the same angle should attack the topic from different audience entries, not paraphrase each other.
- Use the brand's vocabulary. Never use a word from the brand's words-to-avoid list.
- Avoid stale patterns: "Are you tired of...?", "Did you know...?", "Hot take:", "Buckle up.", emoji walls, all-caps.
- Stay specific. If you're tempted to write "10 lessons from building," replace with the specific lessons or the specific tension behind them.
- If the existing hook is strong, say so and propose iterations rather than replacements.
```

---

## Example invocation

```
Brand: brands/lumen/
Topic: most AI tools fail at deep work because they're shaped like chat
Audience: senior PMs and engineering leads at 50-500 person SaaS companies who feel productive AI tools have made their week worse, not better
Platform: linkedin
Goal: save
Existing hook: "5 ways AI is hurting your focus"
What's been tried: previous "AI productivity tips" hooks underperformed; audience seems tired of generic AI takes
```

Claude returns 12 variants like:

> **Variant 1 — Contrarian**
> Hook: Most AI tools quietly destroy your deep work.
> Support text: And the chat is why.
> Optimizing for: permission to suspect what they already feel
> Score: Specificity 4/5 · Tension 5/5 · Audience-fit 5/5 · Brand-fit 4/5 · Total: 18/20

> **Variant 2 — Mistake**
> Hook: I installed 14 AI tools in a quarter. My deep work collapsed.
> Support text: Here's what I removed first.
> Optimizing for: confession + curiosity about which tools survived
> Score: Specificity 5/5 · Tension 4/5 · Audience-fit 5/5 · Brand-fit 5/5 · Total: 19/20

[...10 more variants across 4 more angles...]

## Top 3 picks

1. **"I installed 14 AI tools in a quarter. My deep work collapsed."** — strongest because the specific number and the confession lower the audience's defenses. Implies a list-style payoff. Aligns with mistake angle, which historically performs for this audience.
2. **"Most AI tools quietly destroy your deep work."** — strong contrarian punch. Better for save than for comment. Pairs with a mechanism-heavy slide 2.
3. **"Stop installing AI tools that interrupt you."** — sharp prescriptive frame. Lower specificity but higher action-orientation. Best if the carousel is workflow-heavy.

## Recommended next step

Lead with Variant 2 (the confession). It implies a workflow reveal in slide 5-6 ("the AI tools I kept and why") and a CTA inviting the reader to share their own removals. The current carousel structure works; only slide 1 needs replacement.

---

## When to use this prompt

- The hook on a generated carousel is the weak link.
- You're testing 2-3 variants of a planned post for a series.
- You want to find the single sharpest line in your draft.
- You're stuck and want to see the topic from 6 angles fast.

---

## When NOT to use it

- The carousel hasn't been drafted at all — use `prompts/generate-carousel.md`, which already includes 5 alternative hooks.
- You're iterating on slide 4 or slide 5 — those are mechanism / proof slides; this prompt is about slide 1 only.
- The whole carousel is weak. A new hook won't save a structure problem; run `prompts/critique-carousel.md` instead.

---

## Iterating after picking a hook

```
Use Variant 2 as the new slide 1.
Adjust slides 2-3 to set up the "deep work collapsed" promise — show the cost before resolving it.
Return only the updated slides 1, 2, and 3.
```

Claude updates only the affected slides without regenerating the rest.

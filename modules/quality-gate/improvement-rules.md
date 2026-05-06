# Improvement Rules

> How to translate a critique into the *single highest-leverage fix*. The Quality Gate identifies one fix per pass — not a list of nitpicks.

---

## The principle

A weak post usually has 4-5 things wrong with it. Fix all 5 and the post is over-edited and dies. Fix the *one* that matters and the rest fall in line.

The reviewer's job is to find that one.

---

## How to identify the highest-leverage fix

Score each problem on three axes:

1. **Audience impact** — how much does this hurt the reader's experience?
2. **Cascade effect** — fixing this also fixes other problems.
3. **Cost to fix** — can it be repaired in 5 minutes or 5 hours?

The highest-leverage fix maximizes audience impact + cascade effect at the lowest cost.

---

## Common highest-leverage fixes

### "The hook is generic."

If slide 1 is generic, nothing else matters. The carousel won't be opened.

**Fix:** rewrite slide 1 with a specific subject, tension, and implied payoff. Use [`prompts/improve-hook.md`](../../prompts/improve-hook.md) to generate variants.

### "There's no mechanism."

If the carousel asserts a conclusion without showing why, the audience won't believe it.

**Fix:** insert or rewrite the mechanism slide (typically slide 4-5). Show the underlying *why*.

### "Two slides argue the same point."

Redundancy kills momentum.

**Fix:** combine the two slides into one. Often slide 2 and slide 3 are the same problem said differently.

### "The CTA doesn't match the post energy."

A serious essay can't end with "Hope this helps!"

**Fix:** rewrite the CTA to match the angle. See [`copywriting-rules.md`'s CTA matching table](../../system/copywriting-rules.md).

### "Voice doesn't match the Brand Pack."

The post sounds like a different brand.

**Fix:** rewrite captions and headlines through the lens of `brands/[slug]/voice.md`. Search for banned words first.

### "An invented claim."

A specific number, customer, or quote that wasn't in the brief or `proof-assets.md`.

**Fix:** replace with `[PROOF_NEEDED: short description]` or rephrase to remove the specificity.

### "The visual is decoration, not metaphor."

If the image is interchangeable with any post in the category, it's failing.

**Fix:** specify a visual metaphor that connects to the carousel's argument. See [`visual-framework.md`](../../system/visual-framework.md).

---

## How to propose rewrites for the weakest slide

Pick the *single weakest* slide. Don't critique every slide. Don't propose 8 rewrites.

For the chosen slide:

1. Quote the current headline + support text.
2. Explain in 1-2 sentences what's wrong.
3. Propose **2-3 rewrites**, each from a different angle/framing.
4. Tag each rewrite with the angle it leans on.

Example:

```
**Slide 4 — Reframe**

Current: "AI tools are bad."
Why it's weak: vague, no audience, no mechanism, banned-word energy ("bad").

Rewrite A (Mechanism): "The problem isn't the AI. It's that chat is shaped wrong for focus."
Rewrite B (Identity): "Senior operators don't ban AI. They ban interruptions."
Rewrite C (Threshold): "Stop installing AI. Start installing fewer interruption surfaces."
```

The writer picks one. The Quality Gate doesn't.

---

## Fix-once vs rewrite-all decision

If the critique reveals **one critical issue**:
- Apply the single fix.
- Re-run Quality Gate.

If the critique reveals **3+ critical issues**:
- The post has structural problems.
- Don't repair piecemeal.
- Recommend regenerating from a sharper brief.

---

## Repair budgets

Each output should be improvable in **2 iterations max**:

- Iteration 1: apply the highest-leverage fix.
- Iteration 2: apply the next highest-leverage fix from the new critique.

If the score doesn't pass after iteration 2, the original brief was the problem. Escalate to brief refinement, not more repair.

---

## What the writer should *not* do with a critique

- Apply every flagged item simultaneously. Over-editing kills voice.
- Argue with the critique. The writer's job is to ship; the reviewer's job is to make the ship more likely.
- Re-run the whole carousel from scratch when one slide is the problem.
- Ignore safety/claims violations because "the audience won't notice." They notice. The brand pays.

---

## What the reviewer should *not* do

- List 12 things to fix. Pick one.
- Suggest a rewrite that contradicts the Brand Pack.
- Score harshly without specifying the gap.
- Approve work the writer made up just because it "reads well."
- Soft-pedal critical fails to be polite.

---

## When the highest-leverage fix is "regenerate"

Sometimes the critique reveals the carousel is salvageable but barely. If 4+ critical items fail, recommend regenerating from a sharper brief instead of patching.

In that case, the Quality Gate output's `highest_leverage_fix` field should say:

> "Regenerate from brief. Specifically: clarify [TOPIC/AUDIENCE/SIGNAL] before re-running prompts/04-generate-carousel.md. Patching this draft will produce diminishing returns."

# Claims and Proof

> The single most important rule in PostStudio System. Read this once. Internalize it.

The reference implementation is in [`system/safety-and-claims-rules.md`](../system/safety-and-claims-rules.md). This file is the philosophical layer.

---

## The first principle

> If you didn't see the number, the customer, the award, or the result, do not write it as fact.

This is non-negotiable. Confidence ≠ fabrication.

---

## What the system never invents

- Numbers ("47% faster," "10x improvement," "1,200 customers").
- Customer names or anonymized stories.
- Quotes / testimonials.
- Awards / press / certifications.
- Comparisons against named competitors.
- Industry / category statistics.
- Time-on-product results.
- Conversion / ROI metrics.
- Sample sizes.
- Founder credentials.

If the brief or `proof-assets.md` doesn't contain it, the system marks `[PROOF_NEEDED: short description]` and surfaces it.

---

## Why this matters more than anything else

Audiences for the brand's best content are the most skeptical. They read for credibility signals. One fabricated number gets caught. The cost of getting caught:

- Brand trust collapses faster than it built.
- Future content gets pre-discounted.
- The audience who liked you most leaves first.
- Legal / compliance exposure (depending on category).
- The team's own confidence in shipping erodes (everyone wonders which numbers are real).

The cost of marking `[PROOF_NEEDED]`:

- The user supplies the number. Or doesn't, and the slide gets reworded. That's it.

---

## The four claim levels

| Level | Example | Required evidence |
|---|---|---|
| **Universal** | "Every SaaS company should have onboarding telemetry." | Strong, broad evidence. Easy to disprove. Avoid. |
| **Categorical** | "Most B2B SaaS onboarding flows lose 40% in the first session." | Directional evidence at minimum. |
| **Experiential** | "In my experience, every founder underestimates onboarding." | Personal credibility. |
| **Specific evidentiary** | "[Customer] cut onboarding from 14 days to 2." | Verbatim, attributable evidence. |

Most strong content lives at the *experiential* + *specific evidentiary* levels. Avoid universal claims.

---

## Opinion vs fact, on the slide

A slide can carry both — but they must be **separated**.

Bad:
> "Most onboarding flows are too long. They lose 40% in the first session and that's why they fail."

Good:
> "Most onboarding flows are too long. [Cite or PROOF_NEEDED for the 40% figure.] My take: every step beyond 5 is a tax."

The user reading the slide must be able to tell what's *opinion* (the brand's interpretation) and what's *fact* (a sourced claim).

---

## How `[PROOF_NEEDED]` works

When a slide needs evidence the system doesn't have:

1. Write the slide structure with `[PROOF_NEEDED: short description]` in the headline or support text.
2. Add the gap to the **Missing Inputs / Proof Needed** section.
3. Continue. Don't stall the carousel.

The user fills in the proof in 30 seconds, or reframes the slide. The system never silently fills.

Example:

```
## Slide 6 — Proof
- Role: proof
- Headline: We cut [PROCESS] from [BEFORE_TIME] to [AFTER_TIME]
- Support text: [PROOF_NEEDED: real before/after times from a real customer]
- Visual concept: a side-by-side timeline visualization
- Image prompt: ...
- Layout instruction: ...
```

And in `Missing Inputs`:

```
- Slide 6 needs the actual before/after time numbers and the customer to attribute (anonymized or named per permission).
```

---

## Forbidden moves (absolute, no exceptions)

- Never use a real company's name as a customer / partner / comparison without explicit Brand Pack permission.
- Never quote a real person unless the quote is verbatim and the user provided it.
- Never invent a "study" or "research finding."
- Never claim industry leadership without proof.
- Never imply regulatory approval (HIPAA-compliant, SOC 2 certified, etc.) unless `constraints.md` confirms.
- Never imply scale ("thousands of customers") without numbers in the Brand Pack.
- Never make health, financial, or safety claims without verified credentials and disclaimers.
- Never disparage a competitor by name unless explicitly authorized.

---

## Allowed moves

- Make opinionated framing.
- Argue from first principles.
- Cite the user's provided data verbatim.
- Generalize from the user's experience when invited.
- Use placeholder ranges ("teams of 10-50") if the user provides them.
- Quote published research with citations the user provides.
- Tell stories framed as anecdotes (not benchmarks).

---

## When pushed to invent

If the user says "just make up a number, it'll sell better":

1. Don't.
2. Offer alternatives:
   - Use a placeholder the user fills in.
   - Reframe the claim ("noticeably faster" → "we ship reviews same-day").
   - Switch angles to one that doesn't require numbers.
3. Hold the line politely. The Brand Pack's reputation is the long game.

---

## Brand-pack-level proof discipline

Each brand's `proof-assets.md`:

- Lists every citable piece of evidence (case stories, quotes, metrics, press, awards).
- Marks each as `permission_to_cite_publicly: true | false | yes-but-anonymized`.
- Includes `last_reviewed` dates.
- Maintains a separate "things we have but cannot cite publicly" section.

The system uses *only* what's listed. Not what's implied.

---

## Compliance overlay

Brands in regulated categories (health, finance, legal, food, supplements):

- `constraints.md` lists trigger phrases for legal review.
- Specific health / financial / legal claims need disclaimers in captions (not on slides).
- Some categories (medical aesthetics, supplements) ban before/after imagery entirely.

The system flags any draft that contains a trigger phrase. The CRUD platform routes it to legal review before publish.

---

## TL;DR

- Don't invent.
- Mark gaps with `[PROOF_NEEDED]`.
- Surface gaps loudly.
- Hold the line when pushed.
- Compliance flags trigger pause, not silent compliance.
- The Brand Pack's `proof-assets.md` is the system's source of truth.

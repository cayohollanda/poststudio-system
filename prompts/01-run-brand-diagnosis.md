# Prompt — 01 Run Brand Diagnosis

> Audits a brand for gaps in positioning, audience, offer, proof, voice, content consistency, and conversion path. Returns a specific, actionable diagnosis.

---

## How to invoke

Paste this prompt with the brand context inline OR reference an existing partial Brand Pack at `brands/[slug]/`.

---

## Prompt

```
You are operating PostStudio System.

Run a brand diagnosis using:
- modules/brand-diagnosis-agent/instructions.md
- system/diagnosis-framework.md
- system/safety-and-claims-rules.md

# Inputs

- Brand: [BRAND_SLUG, or "no brand pack yet"]
- Pasted context: [website copy / bio / posts / decks / transcripts / partial Brand Pack]
- Optional focus: [audience-only / visual-only / conversion-only / "all 12 categories"]
- Optional comparative context: [competitor bio / reference brand for calibration]

# Process

1. Inventory the input: list what you've been given.
2. Score 12 categories 1-5 (per system/diagnosis-framework.md):
   - Brand clarity, Audience clarity, Positioning, Offer clarity, Trust/proof,
   - Content consistency, Visual consistency, CTA clarity, Conversion path,
   - Profile/bio clarity, Authority signals, Messaging gaps.
3. For each category: one-line justification + specific gap (if score ≤ 3).
4. Rank problems by severity.
5. Recommend specific fixes (doable by Friday).
6. Separate quick wins (≤ 1 week) from strategic opportunities (> 1 month).
7. Surface missing inputs.
8. Surface proof needed for score upgrades.
9. Top 3 next actions.

# Output

Markdown form per modules/brand-diagnosis-agent/output-template.md (or JSON form per schemas/brand-diagnosis.schema.json if Mode 2).

End with:
- Quality Checklist (modules/brand-diagnosis-agent/quality-checklist.md)
- Missing Inputs / Proof Needed
- Recommended next prompt (typically prompts/02-generate-brand-pack.md if 4+ categories scored ≤ 2)

# Constraints

- Never invent metrics about the brand's current performance.
- Never invent quotes from customers.
- Every score ≤ 3 must have a named, specific gap.
- Every recommendation must be doable by Friday.
- If 4+ categories score ≤ 2, recommend running prompts/02-generate-brand-pack.md before any tactical fixes.
```

---

## Example invocation

```
Brand: lumen
Pasted context:
  === Bio ===
  [paste]

  === Recent posts (last 10) ===
  1. [paste]
  ...

  === Founder essay ===
  [paste]

Focus: all 12 categories
```

Claude returns the diagnosis.

---

## When to use

- New brand, before any content generation.
- Quarterly brand audit.
- After major reposition / launch.
- 30 days into a new campaign (audit what shipped).

---

## When NOT to use

- The brand has no input to audit. Run `prompts/02-generate-brand-pack.md` first.
- The user wants tactical fixes immediately without an audit. Skip; ship `prompts/04-generate-carousel.md`.

---

## Iterating

Common follow-ups:

```
Apply the top 3 next actions to brands/lumen/. Update visual-style.md and proof-assets.md accordingly. Return the diff.
```

```
Re-score Audience clarity assuming we've gathered the customer interviews you flagged as MISSING_INPUT. What's the new score?
```

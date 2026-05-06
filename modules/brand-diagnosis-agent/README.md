# Module — Brand Diagnosis Agent

> Audits any brand for gaps in positioning, audience clarity, offer, proof, voice, content consistency, conversion path, and authority. Outputs a specific, actionable diagnosis — not "things you could improve."

---

## What it does

Reads pasted brand context (website copy, bio, posts, decks) or an existing partial Brand Pack. Scores 12 categories 1-5. Surfaces specific gaps. Recommends concrete fixes ordered by leverage.

## When to use it

- Before any content generation for a new brand.
- Quarterly for active brands.
- After a major reposition / launch.
- After 30 days of posting (a "30-day audit").

## When NOT to use it

- The brand is fresh enough that there's nothing to audit. Run [`brand-pack-builder`](../brand-pack-builder/) first.

---

## Required inputs

One or more of:
- Pasted website copy.
- Pasted bio text.
- Bullet list of recent posts.
- Decks, sales pages, product pages.
- Customer interview transcripts.
- Screenshot descriptions.
- An existing partial Brand Pack at `brands/[slug]/`.

## Optional inputs

- Audience scope ("focus on LinkedIn audit only").
- Comparative context ("here's a competitor's bio for reference").
- Time horizon ("audit since the last reposition").

---

## Output

A diagnosis document with:

1. Executive summary (3-5 sentences naming the 1-2 biggest unlocks).
2. Category scores (12 categories, 1-5 each).
3. Problems found (ranked).
4. Why each problem matters.
5. Recommended fixes (specific, doable by Friday).
6. Quick wins (≤1 week).
7. Strategic opportunities (>1 month).
8. Missing inputs (what couldn't be audited).
9. Proof needed (what would unlock score upgrades).
10. Top 3 next best actions.

See [`output-template.md`](output-template.md) and [`schemas/brand-diagnosis.schema.json`](../../schemas/brand-diagnosis.schema.json).

---

## Files in this module

- `README.md` — this file
- `instructions.md` — the module system prompt
- `input-template.md` — what to paste / provide
- `output-template.md` — output structure
- `quality-checklist.md` — module-specific checks

---

## Related

- Prompt: [`prompts/01-run-brand-diagnosis.md`](../../prompts/01-run-brand-diagnosis.md)
- System: [`system/diagnosis-framework.md`](../../system/diagnosis-framework.md)
- Schema: [`schemas/brand-diagnosis.schema.json`](../../schemas/brand-diagnosis.schema.json)
- Often paired with: [`brand-pack-builder`](../brand-pack-builder/) for follow-up.

## Mode support

| Mode | Supported? |
|---|---|
| Claude Project Manual | Yes |
| API Production | Yes |
| Renderable Creative Worker | n/a |

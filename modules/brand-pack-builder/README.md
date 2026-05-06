# Module — Brand Pack Builder

> Builds (or refreshes) the 12-file Brand Pack that grounds every future generation. Two ingestion modes: **interview** (conversational) and **context** (paste-and-extract).

---

## What it does

- Walks the user through a structured interview, or extracts a Brand Pack from pasted website copy / decks / transcripts.
- Generates 12 Brand Pack files: `brand`, `audience`, `offer`, `positioning`, `voice`, `visual-style`, `proof-assets`, `constraints`, `content-pillars`, `competitors`, `examples`, `rejected-examples`.
- Marks unknowns as `_(to be added)_` and proof gaps as `[PROOF_NEEDED]`.

## When to use it

- New brand with no existing Brand Pack.
- Existing brand with stale or partial pack (refresh).
- Multi-brand onboarding (run once per brand).

## When NOT to use it

- A pack already exists at `brands/[slug]/` and is current. Use [`brand-diagnosis-agent`](../brand-diagnosis-agent/) to identify which fields need refresh, then update only those.

---

## Required inputs

- Brand identifier or chosen slug.
- Either (a) a willing user to interview, or (b) pasted brand context (website copy, bio, decks, customer interview transcripts, founder writing samples).

## Optional inputs

- Existing partial Brand Pack to merge / refresh.
- Reference brand voice samples ("we admire X's writing").
- Constraint list (compliance frameworks, banned phrases).

---

## Output

12 Markdown files matching `brands/_template/`:

1. `brand.md` — identity, founders, elevator pitch, why we exist.
2. `audience.md` — primary segment, pains, desires, non-audience, personas.
3. `offer.md` — what we sell, mechanism, alternatives, positioning statement.
4. `positioning.md` — one-liner, differentiation matrix, category framing.
5. `voice.md` — adjectives, words to use / avoid, examples.
6. `visual-style.md` — visual mode, palette, typography, composition rules.
7. `proof-assets.md` — case studies, testimonials, metrics (verbatim).
8. `constraints.md` — claims allowed / forbidden, compliance, trigger phrases.
9. `content-pillars.md` — 3-5 pillars with topic clusters per pillar.
10. `competitors.md` — direct, adjacent, do-nothing, with win/lose analysis.
11. `examples.md` — past content that worked + reference-anchor posts.
12. `rejected-examples.md` — patterns banned because they hurt the brand.

JSON form (Mode 2): conforms to `schemas/brand-pack.schema.json`.

---

## Files in this module

- `README.md` — this file
- `interview-flow.md` — the 10-section interview script
- `brand-pack-template.md` — template references
- `output-template.md` — full Brand Pack output reference
- `quality-checklist.md` — module-specific checks

---

## Related

- Prompt: [`prompts/02-generate-brand-pack.md`](../../prompts/02-generate-brand-pack.md)
- Schema: [`schemas/brand-pack.schema.json`](../../schemas/brand-pack.schema.json)
- Templates: [`brands/_template/`](../../brands/_template/)
- Example: [`examples/example-brand-pack/`](../../examples/example-brand-pack/) (fictional Lumen)

## Mode support

| Mode | Supported? | Notes |
|---|---|---|
| Claude Project Manual | Yes | interview mode runs in chat |
| API Production | Yes | context mode preferred (no interactive interview) |
| Renderable Creative Worker | n/a | not a generation target |

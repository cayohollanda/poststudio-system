# Module — Quality Gate

> Adversarial reviewer pass. Grades any output (carousel, reels script, campaign, brand pack, renderable carousel) against a structured rubric, flags safety violations, identifies the single highest-leverage fix, and proposes concrete rewrites.

The Quality Gate runs *separately* from generation. The same model can play both roles, but never in the same context. Most quality-gate failures come from self-approval bias.

---

## What it does

- Scores outputs against the relevant rubric (content quality OR renderability OR both).
- Surfaces safety/claims violations (`safety-and-claims-rules.md`).
- Identifies the **single** highest-leverage fix.
- Proposes 2-3 concrete rewrites for the weakest slide / beat / post.
- Returns `pass / fail / risk` per checklist item.
- Issues a verdict: `Ship-ready` / `Ship with flags` / `Rewrite required`.

## When to use it

- After every carousel, reels script, or campaign generation (Mode 1, 2, 3).
- Before rendering (Mode 3) — runs renderability checks too.
- Before client delivery (Agency Delivery Kit).
- During quarterly Brand Pack refreshes (audits the pack itself).

## When NOT to use it

- Before a draft exists.
- For lightweight outputs like single-line copy variations (overkill).

---

## Two passes

### Content Quality Gate

Grades the *content* of an output:

- Hook physics, slide structure, mechanism, voice match, claim safety, CTA strength.
- Uses [`critique-rubric.md`](critique-rubric.md) and [`scoring-system.md`](scoring-system.md).
- Output schema: `schemas/quality-review.schema.json`.

### Renderability Quality Gate

Grades the *renderable* form of an output (Mode 3 only):

- Schema validity, canvas correctness, no remote assets / JS / fake logos, fallback fonts, single accent, overflow estimate.
- Uses [`renderability-checks.md`](renderability-checks.md).
- Runs *before* dispatching a render job.

Both passes can run on the same output. The renderable Quality Gate fails fast — if the output isn't safe to render, the content review is moot.

---

## Required inputs

- The output to critique (carousel JSON / Markdown / renderable carousel JSON / etc.).
- The active Brand Pack at `brands/[slug]/`.
- The original brief (so the reviewer can grade against intent).
- The renderability target if applicable (canvas, available fonts, available assets).

## Optional inputs

- Prior critique (when running iteration 2+ on the same output).
- Specific items to focus on (e.g. "review only the hook").

---

## Output

A structured `quality-review` document with:

- Verdict.
- Critical items (10) graded `pass / fail / risk` with one-line justification each.
- Quality items (20) graded `pass / fail / risk`.
- Renderability items (10) graded `pass / fail / risk` (renderable outputs only).
- Score `N/30` (or `N/40` if renderability is included).
- The single highest-leverage fix.
- Weakest slide + 2-3 concrete rewrites.
- Safety / claims violations flagged.
- Slides to combine or kill.
- Platform fit assessment.
- Missing inputs.

See [`schemas/quality-review.schema.json`](../../schemas/quality-review.schema.json).

---

## Files in this module

- `README.md` — this file
- `critique-rubric.md` — the 30-point rubric (content) + 10-point rubric (renderability)
- `scoring-system.md` — how to grade and how to combine scores
- `improvement-rules.md` — how to translate a critique into a single fix
- `renderability-checks.md` — the 10-point renderability rubric
- `red-flags.md` — patterns that auto-fail
- `output-template.md` — the standard quality-review output shape

---

## Mode support

| Runtime mode | Supported? | Notes |
|---|---|---|
| Claude Project Manual | Yes | run as a separate prompt in a new chat |
| API Production | Yes | run as a separate Claude API call after generation |
| Renderable Creative Worker | Yes | run renderability + content passes before render |

---

## Reviewer integrity

If the writer and reviewer are the same model in the same session, the model can drift toward defending its own draft. Mitigations:

- **Mode 1:** run the critique in a new chat within the Project. Paste the carousel only; don't carry context from the writing session.
- **Mode 2 / 3:** the CRUD platform makes a *separate* Claude API call for the critique. Different system prompt: this module's `instructions.md`.
- Use a different LLM for the critique pass if quality is paramount.

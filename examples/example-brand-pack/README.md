# Example Brand Pack — "Lumen" (fictional)

> **This is a fictional reference brand**, used inside the PostStudio System repo to demonstrate the structure and quality bar of a complete 12-file Brand Pack. It is not a real company. Any resemblance is coincidental.

---

## Purpose

Lumen exists to:

1. **Show the shape** of a finished Brand Pack — voice, claims, audience, offer, visual style, proof, constraints, pillars, competitors, examples, rejected examples.
2. **Be safe to ship in the repo** — because nothing in `examples/` is a real client.
3. **Power the worked examples** under `examples/example-carousel-output.md`, `examples/example-content-strategy.md`, etc. — every output references the Lumen brand pack so readers can trace the system end-to-end.

---

## What this is NOT

- **Not a real brand.** Don't ship anything from this folder as if "Lumen" were a real company. Don't reference it in client deliverables.
- **Not a template.** The scaffold for new brand packs is `brands/_template/`, not this folder.
- **Not a runtime brand pack.** When you onboard a real brand, **never copy this folder** into a real `brands/[slug]/` directory. Use [`prompts/19-ephemeral-brand-intake.md`](../../prompts/19-ephemeral-brand-intake.md) for runtime intake or [`prompts/02-generate-brand-pack.md`](../../prompts/02-generate-brand-pack.md) for a full interview.

---

## Why fictional

PostStudio System is brand-agnostic. The framework repo deliberately contains zero real brand instances — only:

- `brands/_template/` — the scaffold for the 12 brand pack files.
- `examples/example-brand-pack/` — this fictional Lumen reference.

Real brand packs live **outside** this repo (private folders, your CRUD platform, your Notion, your Drive). The `.gitignore` blocks accidental commits of real brand directories.

---

## How to use Lumen for testing

### Path A — Testing the framework with no real brand

In a Claude Project that has the repo loaded:

> Use `examples/example-brand-pack/` (the fictional Lumen brand) and generate a carousel about "[topic]" for LinkedIn, English, goal: save.

Claude treats Lumen as the active Brand Pack and produces a carousel. Useful for evaluating the system's output quality without onboarding your real brand.

### Path B — Comparing your output structure against the worked example

Read [`examples/example-carousel-output.md`](../example-carousel-output.md) (built against Lumen) to see what a finished, schema-compliant carousel looks like in Markdown.

---

## The 12 files

This folder demonstrates the full Brand Pack contract:

1. `brand.md` — name, mission, category, story
2. `audience.md` — primary + secondary personas
3. `offer.md` — what's sold and how it's packaged
4. `positioning.md` — category, frame-of-reference, differentiation
5. `voice.md` — tone, vocabulary, words-to-avoid
6. `visual-style.md` — palette, typography, visual mode, mark variants
7. `proof-assets.md` — verified claims and evidence
8. `constraints.md` — claims-allowed / forbidden, compliance
9. `content-pillars.md` — 3-5 recurring themes
10. `competitors.md` — 3-5 alternatives + how Lumen differs
11. `examples.md` — exemplar posts in Lumen's voice
12. `rejected-examples.md` — anti-patterns Lumen would never publish

---

## Lumen at a glance

- **Category:** AI workflow tooling for knowledge workers.
- **Audience:** Senior engineers and PMs at 50-500 person SaaS companies.
- **Voice:** Direct, opinionated, lowercase-friendly, no LinkedIn-bro cadence.
- **Visual mode:** Cinematic Dark (Mode 1) for founder takes; Editorial Minimal (Mode 2) for educational.
- **Accent:** A warm amber.
- **Stance:** Most AI tools are chat-shaped, and chat is the opposite of deep work.

This is enough specificity for the framework to produce coherent output across any prompt — but **none of it is real**. Treat it as a benchmark, not a brand.

---

## Next reading

- [`brands/_template/`](../../brands/_template/) — the scaffold for a real brand pack.
- [`prompts/19-ephemeral-brand-intake.md`](../../prompts/19-ephemeral-brand-intake.md) — the canonical default for runtime brand intake.
- [`prompts/02-generate-brand-pack.md`](../../prompts/02-generate-brand-pack.md) — interview-driven brand pack creation.
- [`docs/getting-started.md`](../../docs/getting-started.md) — onboarding flow.

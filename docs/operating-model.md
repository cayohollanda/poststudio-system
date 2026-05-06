# Operating Model

> The conceptual model behind PostStudio System. Read this once to understand *why* the repo is shaped the way it is.

The reference implementation of the lifecycle lives in [`system/operating-system.md`](../system/operating-system.md). This file is the philosophical layer.

---

## The three runtime modes

PostStudio System is one repository serving three different runtimes:

```
┌────────────────────────────────────┐
│ Mode 1 — Claude Project Manual     │
│ Human in chat. Markdown out.       │
│ For: founders, agencies, teams.    │
└────────────────────────────────────┘

┌────────────────────────────────────┐
│ Mode 2 — API Production            │
│ External CRUD platform → Claude API│
│ JSON in, JSON out, schemas enforce.│
│ For: SaaS, internal engines.       │
└────────────────────────────────────┘

┌────────────────────────────────────┐
│ Mode 3 — Renderable Creative Worker│
│ Claude → renderable JSON → worker. │
│ Final output: PNG/JPG slides.      │
│ For: production pipelines.         │
└────────────────────────────────────┘
```

The same repo. The same rules. Different runtimes.

---

## Why three modes (not one)

A content engine has different consumers:

- A **founder** wants to ship a post tonight from a Claude Project. Manual is right.
- A **SaaS platform** wants its users to spin up brands and generate content at scale. API is right.
- A **production pipeline** wants finished image assets, not specs for a designer. Renderable is right.

Forcing all three into one runtime breaks every use case. Splitting them lets each runtime be ergonomic.

---

## Why one repo

The temptation is to build a SaaS app, an API service, and a worker — three repos, three teams.

We did the opposite. One repo. Documentation-first. Why:

1. **The hard part is the prompt.** Not the orchestration. The same prompts produce great content in any runtime. Don't fork them.
2. **Schemas are the contract.** All three runtimes consume the same schemas. Forking them produces drift.
3. **Brand Packs travel.** A brand built in Mode 1 should work in Mode 2 / 3 unchanged.
4. **Quality Gate is universal.** Same rubric. Same red flags. Same scoring.
5. **Repo = team memory.** When a brand learns "listicle hooks don't work for our audience," that lesson lives in the brand pack — not in three separate codebases.

---

## The role split

```
External CRUD Platform (the system of record)
  - stores brands, briefings, jobs, outputs, assets
  - validates schemas
  - orchestrates Claude API calls and worker calls
  - provides the user-facing UI

Claude API (the content engine)
  - generates strategy, copy, visual direction, renderable code
  - runs Quality Gate
  - repairs broken renderable

Rendering Worker (the asset producer)
  - validates and sanitizes
  - resolves assets and fonts
  - renders HTML/SVG to PNG
  - optimizes and uploads
```

Each role has clear boundaries. None of them does the others' job. See [`system/integration-contract.md`](../system/integration-contract.md).

---

## The lifecycle (12 stages)

```
[0] Input → [1] Diagnose → [2] Brand Pack → [3] Strategy → [4] Brief
       → [5] Generate → [6] Quality Gate → [7] Render
       → [8] Deliver → [9] Iterate
```

Same stages in every mode. Different actors per stage. See [`system/operating-system.md`](../system/operating-system.md).

---

## What's stable, what's swappable

**Stable** (the OS kernel — `system/`):
- Master instructions.
- Operating system lifecycle.
- 17 carousel angles.
- 8 carousel structures.
- 12 visual modes.
- Renderable creative framework.
- Hook physics + copywriting rules.
- Safety + claims rules.
- Quality checklist.
- Output rules (Markdown + JSON).
- Integration contract.

**Swappable** (per brand — `brands/`):
- Brand identity, voice, audience, offer, positioning.
- Visual style.
- Proof assets, constraints.
- Content pillars, competitors, examples, rejected examples.

**Swappable** (per task — `prompts/`):
- 18 prompts for different operations.

**Swappable** (per consumer — `integration/`, `rendering/`):
- API contracts.
- Worker implementation.
- CRUD platform integration.

The system layer barely changes between brands. The brand layer changes per tenant. The prompt layer changes per task. The integration / rendering layers change per consumer.

---

## The compounding flywheel

```
Brand Pack v1
  → generates 5 carousels
  → runs Quality Gate on each
  → captures what worked / didn't (examples.md, rejected-examples.md)
  → updates voice.md with audience-adopted vocabulary
Brand Pack v2 (sharper)
  → generates 5 better carousels
  → ... and so on
```

The Brand Pack compounds. Every cycle should make the next post easier and sharper. If the pack isn't compounding, the team isn't capturing the cycle's lessons.

---

## The cost model

| Layer | Cost | Cadence |
|---|---|---|
| System layer | Big up-front. Stable thereafter. | Once per major version. |
| Brand layer | Big up-front per brand. Quarterly refreshes. | Per brand × quarters. |
| Prompt layer | Light. | Per task. |
| Integration layer | Big up-front per CRUD platform. | Once per platform. |
| Rendering layer | Big up-front per worker. | Once per worker. |

Mode 1 (manual) has near-zero infrastructure cost. Mode 2 + 3 have meaningful platform/worker cost amortized over many brands and many jobs.

---

## What this is not

- **Not a SaaS.** No vendor lock-in. Use any LLM provider. Swap workers. Run Mode 1 forever if Mode 2/3 don't fit.
- **Not a marketplace.** No template store. Templates per brand are local to the worker.
- **Not a black box.** Every prompt is human-readable. Every schema is documented. Every decision is in `system/`.
- **Not framework-bound.** No language preference. No framework dependency. Anyone can implement Mode 2 / 3.

---

## What you give up

- Plug-and-play UX. PostStudio System is documentation; you bring the runtime.
- Pre-built integrations. The integration layer documents the contract; you implement.
- A unified vendor relationship. You compose the stack from open or commercial pieces.

In exchange, you get a system that works for one brand or a thousand, manual or automated, with consistent quality across every runtime.

# Operating System — End-to-End Lifecycle

> The master lifecycle of PostStudio System. Every job — manual, API, or render-pipeline — follows this spine. Read this once; reference it forever.

---

## The 12 stages

```
┌────────────┐   ┌──────────┐   ┌────────────┐   ┌──────────┐   ┌────────┐
│  0. Input  │──▶│ 1. Diag  │──▶│ 2. Brand   │──▶│ 3. Strat │──▶│ 4. Brf │
└────────────┘   └──────────┘   │    Pack    │   │   egy    │   └────────┘
                                └────────────┘   └──────────┘        │
                                                                     ▼
┌────────────┐   ┌────────────┐   ┌──────────┐   ┌──────────┐   ┌────────┐
│ 9. Iterate │◀──│ 8. Deliver │◀──│ 7. Render│◀──│ 6. QGate │◀──│ 5. Gen │
└────────────┘   └────────────┘   │  (Mode 3)│   │          │   │  Copy  │
                                  └──────────┘   └──────────┘   │  + Vis │
                                                                └────────┘
```

The lifecycle is the same in every mode. **What changes is who/what executes each stage.**

| Stage | Manual (Mode 1) | API Production (Mode 2) | Renderable Worker (Mode 3) |
|---|---|---|---|
| 0 Input | Human types | CRUD platform stores | n/a |
| 1 Diagnose | Claude in chat | Claude API call | n/a |
| 2 Brand Pack | Claude interview | Claude API + form ingestion | n/a |
| 3 Strategy | Claude in chat | Claude API call | n/a |
| 4 Brief | Human writes | CRUD platform composes | n/a |
| 5 Generate | Claude in chat | Claude API call | n/a |
| 6 Quality Gate | Claude in chat | Claude API (separate call) | n/a |
| 7 Render | Manual layout (Figma) | n/a (returns spec) | Worker renders PNG |
| 8 Deliver | Human posts | CRUD returns to user | CRUD returns final URLs |
| 9 Iterate | Claude in chat | Claude API call | n/a |

---

## Stage 0 — Input

**Objective.** Capture the raw inputs needed to start a job: brand identifier (or context), topic, audience, platform, language, goal, and any provided proof.

**Required inputs.**
- Brand slug (or pasted brand context if no Brand Pack yet).
- Topic (one sentence).
- Audience (specific role + situation).
- Platform (linkedin / instagram / tiktok / x / threads / multi).
- Language (BCP-47 tag).
- Goal (save / comment / share / click / dm / follow / try / awareness).

**Optional inputs.**
- Angle (from `system/content-system.md`).
- Structure (from `system/carousel-framework.md`).
- Visual mode (from `system/visual-framework.md`).
- Render mode (`html-css` | `svg` | `template` | `none`).
- Slide count.
- Tone override.
- Reference posts / anchors.

**Output.** A normalized `generation-request` object (see `schemas/generation-request.schema.json`).

**Mode notes.**
- **Mode 1:** the user types a brief in chat; Claude can ask up to 3 clarifying questions.
- **Mode 2:** the CRUD platform validates the request against the schema before calling Claude API.
- **Mode 3:** input is upstream — comes from a Mode 2 result tagged `render: true`.

**Next:** Stage 1 if the brand has no diagnosis, Stage 2 if no Brand Pack, otherwise Stage 4.

---

## Stage 1 — Brand Diagnosis (optional but recommended)

**Objective.** Identify gaps in positioning, audience clarity, offer clarity, proof, voice, content consistency, conversion path, and authority.

**Module:** `modules/brand-diagnosis-agent/`.
**Prompt:** [`prompts/01-run-brand-diagnosis.md`](../prompts/01-run-brand-diagnosis.md).
**Output schema:** `schemas/brand-diagnosis.schema.json`.

**Required inputs.**
- Pasted brand context (website copy, bio, posts, decks, notes, screenshots).
- Or an existing partial Brand Pack.

**Output.** A diagnosis with: executive summary, scored categories, problems found, recommended fixes, quick wins, strategic opportunities, missing inputs.

**Mode notes.**
- **Mode 1:** Claude returns a Markdown diagnosis the user reviews.
- **Mode 2:** diagnosis returned as JSON; CRUD platform stores it as a brand artifact.

**Next:** Stage 2.

---

## Stage 2 — Brand Pack

**Objective.** Build (or refresh) the 12-file Brand Pack that grounds every future generation.

**Module:** `modules/brand-pack-builder/`.
**Prompt:** [`prompts/02-generate-brand-pack.md`](../prompts/02-generate-brand-pack.md).
**Output schema:** `schemas/brand-pack.schema.json`.

**Two ingestion modes.**
1. **Interview mode** — Claude walks 10 sections one at a time. Best for new brands.
2. **Context mode** — User pastes website copy, decks, transcripts. Claude extracts the Brand Pack and flags `[MISSING_INPUT]` for gaps. Best for existing brands.

**Output.** 12 Markdown files saved under `brands/[slug]/`:
`brand`, `audience`, `offer`, `positioning`, `voice`, `visual-style`, `proof-assets`, `constraints`, `content-pillars`, `competitors`, `examples`, `rejected-examples`.

**Mode notes.**
- **Mode 1:** files are returned in chat; user copy-pastes into the brand folder.
- **Mode 2:** the CRUD platform stores the pack as 12 separate documents (or one composite) keyed by brand_id.

**Next:** Stage 3 (or skip to Stage 4 if you already have a strategy).

---

## Stage 3 — Content Strategy

**Objective.** Define content pillars, recurring series, channel strategy, and a 30/90-day plan that ties every post to the brand's offer.

**Module:** `modules/content-strategy-os/`.
**Prompt:** [`prompts/03-generate-content-strategy.md`](../prompts/03-generate-content-strategy.md).
**Output schema:** `schemas/content-strategy.schema.json`.

**Output.** A strategy doc with: 3-5 pillars, 2-4 recurring series, 30-day plan with topics tagged by pillar, content-to-offer mapping, distribution notes.

**Next:** Stage 4.

---

## Stage 4 — Brief

**Objective.** Translate a single post intent into a concrete brief Claude can act on.

**This stage is lightweight.** It's the moment between strategy and generation.

**Inputs:**
- Topic (from the content plan, or ad hoc).
- Angle (from `system/content-system.md`, or "auto").
- Structure (from `system/carousel-framework.md`, or "auto").
- Signal (the strongest evidence material — number, opinion, story).
- Render mode (`html-css` | `svg` | `template` | `none`).

**Output:** A normalized brief object that feeds Stage 5.

**Next:** Stage 5.

---

## Stage 5 — Generate (Copy + Visual Direction)

**Objective.** Produce the carousel itself — strategic angle, slide-by-slide copy, visual concepts, image prompts, caption, CTA, alt hooks.

**Module:** `modules/carousel-machine/` (and `modules/visual-prompt-system/` for image-prompt depth).
**Prompt:** [`prompts/04-generate-carousel.md`](../prompts/04-generate-carousel.md).
**Output schema:** `schemas/carousel-output.schema.json`.

**Output.** Markdown (Mode 1) or JSON (Mode 2/3) following the standard output contract.

**Mode notes.**
- **Mode 1:** Markdown is returned; user designs slides manually.
- **Mode 2:** JSON returned; CRUD platform stores it.
- **Mode 3:** instead of (or in addition to) the standard carousel, a renderable carousel is produced — see Stage 5b.

**Next:** Stage 6.

### Stage 5b — Renderable Creative (Mode 3 only)

**Objective.** Produce HTML/CSS, SVG, or template-driven JSON suitable for a worker to render into 1080×1350 PNG/JPG slides.

**Module:** `modules/renderable-creative-system/`.
**Prompt:** [`prompts/13-generate-renderable-carousel.md`](../prompts/13-generate-renderable-carousel.md).
**Output schema:** `schemas/renderable-carousel.schema.json`.

Three sub-modes:
- **Freeform HTML/CSS** — Claude writes a self-contained HTML/CSS document per slide. Best for MVP and creative exploration.
- **SVG** — Claude writes valid SVG per slide. Best for vector/editorial layouts.
- **Template-driven** — Claude picks a template ID and fills its slots with content + render metadata. Best for stable production.

Constraints: no JS, no remote assets, no copyrighted logos, deterministic layout, safe margins, fallback fonts. Full rules in `rendering/html-css-generation-rules.md`, `rendering/svg-generation-rules.md`, `modules/renderable-creative-system/template-json-rules.md`.

**Next:** Stage 6.

---

## Stage 6 — Quality Gate

**Objective.** Adversarially grade the output against the 30-point checklist, surface safety/claims violations, propose the single highest-leverage fix.

**Module:** `modules/quality-gate/`.
**Prompt:** [`prompts/08-critique-output.md`](../prompts/08-critique-output.md).
**Output schema:** `schemas/quality-review.schema.json`.

**Two passes:**
- **Content Quality Gate** — runs against `system/quality-checklist.md` (30 items + claim safety).
- **Renderability Quality Gate** (Mode 3 only) — runs against `modules/quality-gate/renderability-checks.md` (HTML/CSS/SVG safety, overflow risk, font availability, asset references).

**If output fails any critical item:** auto-route to Stage 6b (improvement) before Stage 7.

**Mode notes.**
- **Mode 1:** Claude returns a critique; user decides to apply or skip.
- **Mode 2:** the CRUD platform runs Quality Gate as a *separate Claude API call* (writer + reviewer never share context).
- **Mode 3:** Quality Gate runs *before* the render job is dispatched.

### Stage 6b — Improve (if needed)

**Prompt:** [`prompts/09-improve-output.md`](../prompts/09-improve-output.md) or [`prompts/16-repair-renderable-output.md`](../prompts/16-repair-renderable-output.md) for renderable specifically.

Apply only the single highest-leverage fix from Stage 6. Re-run Quality Gate. Loop max 2 times before flagging the job for human review.

**Next:** Stage 7.

---

## Stage 7 — Render (Mode 3 only)

**Objective.** Convert renderable output into final PNG/JPG slides at the target dimensions.

**Layer:** `rendering/`.
**Prompt (for repair, not render):** [`prompts/16-repair-renderable-output.md`](../prompts/16-repair-renderable-output.md).
**Output schema:** `schemas/render-result.schema.json`.

**Worker responsibilities** (see `rendering/architecture.md`):
1. Validate the renderable carousel against `schemas/renderable-carousel.schema.json`.
2. Sanitize HTML/CSS/SVG per `rendering/safety-and-sanitization.md`.
3. Inject approved fonts and assets per `rendering/font-and-asset-handling.md`.
4. Render each slide at canvas dimensions (default 1080×1350).
5. Optimize PNGs.
6. Upload to object storage.
7. Return `render-result` with asset URLs and metadata.
8. On failure → return structured error → CRUD platform optionally calls `prompts/16-repair-renderable-output.md` and retries (max 2).

**Mode notes.**
- **Mode 1:** skip this stage; lay out manually.
- **Mode 2 (without render):** skip this stage; deliver Markdown/JSON spec only.
- **Mode 3:** this stage is the whole point.

**Next:** Stage 8.

---

## Stage 8 — Deliver

**Objective.** Hand the finished output to the consumer.

**Mode notes.**
- **Mode 1:** the user posts manually; the system suggests caption, first comment, alt hooks. Optional: log results into `brands/[slug]/proof-assets.md`.
- **Mode 2:** CRUD platform returns the JSON to the consuming app (dashboard, scheduler, internal CMS).
- **Mode 3:** CRUD platform returns final asset URLs (PNGs) plus caption / CTA / first-comment metadata.

**Output.** A `delivery` package: assets (or specs), caption, CTA, first comment, alt hooks, quality scorecard, missing inputs list.

**Next:** Stage 9.

---

## Stage 9 — Iterate

**Objective.** Capture results, refresh the Brand Pack, sharpen the next post.

**What to capture:**
- What landed (saves, comments, DMs, conversions). Update `brands/[slug]/proof-assets.md` and `examples.md`.
- What didn't (what underperformed and why). Update `brands/[slug]/rejected-examples.md`.
- New vocabulary the audience used. Update `brands/[slug]/voice.md`.

**Cadence:**
- **Mode 1:** human captures within 72 hours.
- **Mode 2 / 3:** the CRUD platform can record analytics and feed them back as Brand Pack refresh suggestions on a schedule.

**Next:** Stage 0 of the next job. The Brand Pack gets sharper every cycle.

---

## Mode-specific lifecycle diagrams

### Mode 1 — Claude Project Manual

```
[Human types brief in Claude]
  → Claude reads system/ + brands/[slug]/
  → Claude generates carousel (Markdown)
  → Human runs critique prompt in same Project (new chat)
  → Claude returns adversarial review
  → Human applies fix or ships
  → Human renders visuals manually (Midjourney + Figma)
  → Human posts
  → Human updates Brand Pack
```

### Mode 2 — API Production

```
[CRUD platform validates request against generation-request.schema.json]
  → CRUD assembles prompt: system/master-instructions.md + module instructions + brand pack + brief + output schema
  → Claude API call → returns generation-response.schema.json
  → CRUD validates response against schema
  → CRUD makes a separate Claude API call for Quality Gate (prompts/08-critique-output.md)
  → If fails: CRUD makes a third call for prompts/09-improve-output.md
  → CRUD stores final output keyed by brand_id and job_id
  → CRUD returns to consumer (or fires webhook)
```

### Mode 3 — Renderable Creative Worker

```
[Mode 2 pipeline runs end-to-end with render: true flag]
  → Claude API returns renderable-carousel.schema.json
  → CRUD validates renderable schema
  → CRUD posts a render-job to the worker
  → Worker validates + sanitizes + renders + optimizes + uploads
  → Worker returns render-result.schema.json (PNG URLs)
  → CRUD stores assets, fires webhook
```

---

## Where to start reading

Depending on what you're building:

- **Manual content engine:** `docs/getting-started.md` → `docs/how-to-use-with-claude-projects.md` → `prompts/00-start-here.md`.
- **API production engine:** `docs/api-production-mode.md` → `integration/claude-api-mode.md` → `integration/external-crud-platform-contract.md`.
- **Renderable pipeline:** `docs/renderable-output-mode.md` → `rendering/architecture.md` → `rendering/playbooks/mvp-html-to-png-workflow.md`.

---

## Versioning

Every output (in any mode) carries a `prompt_version` field tied to a tagged release of this repo. Pin it in production. When you upgrade the repo, run a small validation suite before flipping the flag — the schemas are stable across minor versions; major versions may change them.

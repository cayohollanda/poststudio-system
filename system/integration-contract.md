# Integration Contract

> The contract between PostStudio System (Claude-side) and an external CRUD platform (consumer-side). Defines who owns what across the pipeline.

This is the authoritative definition. The longer integration docs in `integration/` derive from this file.

---

## Three roles

```
┌─────────────────────────────┐    ┌─────────────────────────────┐    ┌─────────────────────────────┐
│  External CRUD Platform     │    │  Claude API (PostStudio)    │    │  Rendering Worker            │
│  - the system of record     │    │  - the content engine       │    │  - the asset producer        │
│  - storage + jobs + UI      │    │  - the brand voice          │    │  - HTML/SVG/JSON → PNG       │
└─────────────────────────────┘    └─────────────────────────────┘    └─────────────────────────────┘
```

Each role has fixed responsibilities. Boundaries are non-negotiable.

---

## CRUD Platform — what it owns

**Storage:**
- Brands (one record per brand, with brand-pack data either as 12 documents or one composite blob).
- Briefings (per-job inputs).
- Campaigns (multi-post sequences).
- Jobs (each generation is a job with status, inputs, outputs, prompt_version, costs).
- Outputs (Markdown, JSON, renderable JSON, render results).
- Final assets (PNG/JPG URLs from the worker).
- Approvals, retries, revisions, audit logs.

**Job orchestration:**
- Validate the request against `integration/schemas/generation-request.schema.json` before sending.
- Assemble the prompt (system + module + brand pack + brief + schema reference).
- Call Claude API.
- Validate the response against the schema.
- Optionally run a Quality Gate pass (a separate Claude API call).
- For renderable jobs: post the renderable carousel to the worker.
- Receive worker results (synchronously or via webhook).
- Store everything keyed by `brand_id`, `job_id`, `prompt_version`.
- Surface to the user / consuming app.

**UI:**
- Brand management (create, edit, refresh Brand Pack).
- Briefing creation (topic, audience, angle, render mode).
- Job tracking (status, output preview, approve / retry / revise).
- Asset library (final renders).

**What the CRUD platform does NOT own:**
- Designing slides.
- Placing text over images.
- Choosing visual modes.
- Composing renderable HTML/CSS/SVG.
- Rendering image assets.
- Generating final PNG/JPG.

---

## Claude API — what it owns

**Generation:**
- Strategic angle selection.
- Slide-by-slide copy.
- Visual direction (concept + image prompts).
- Caption, CTA, first comment, alt hooks.
- Renderable HTML/CSS/SVG/template-driven JSON when requested.
- Quality Gate scoring and critique.
- Repair of broken renderable output.
- Brand diagnosis, content strategy, campaign design, reels scripts.
- Surface gaps as `[PROOF_NEEDED]` and `missing_inputs[]`.

**What Claude does NOT own:**
- Storage.
- Job orchestration.
- Validation enforcement (Claude *self-validates* but doesn't enforce — the CRUD does).
- Asset rendering.
- Asset upload.
- User-facing UI.
- Analytics or post-publish results capture.

---

## Rendering Worker — what it owns

**Validation:**
- Renderable carousel against `schemas/renderable-carousel.schema.json`.
- Renderable slides against `schemas/renderable-slide.schema.json`.
- HTML/CSS sanitization per `rendering/safety-and-sanitization.md`.
- SVG validation.
- Template ID resolution (template-driven mode).

**Rendering:**
- Inject approved fonts and assets.
- Render each slide at the requested canvas (default 1080×1350).
- Optimize PNG/JPG output.
- Upload to object storage.
- Return `render-result` with asset URLs and metadata.

**Failure handling:**
- Return structured errors with slide number, error type, suggested repair.
- Optional: invoke `prompts/16-repair-renderable-output.md` directly via Claude API for auto-repair (or hand back to CRUD platform for retry orchestration — both are valid).

**What the worker does NOT own:**
- Generating content.
- Choosing brand voice or visual mode.
- Storing brand context (it's stateless per job).

---

## Job lifecycle (Mode 2 + 3)

```
1. CRUD platform receives a brief from the user.
2. CRUD validates against generation-request.schema.json.
3. CRUD assembles the prompt:
     system/master-instructions.md
     + module instructions (e.g. modules/carousel-machine/instructions.md)
     + brands/[slug]/* (the active Brand Pack)
     + the brief
     + the schema reference (e.g. schemas/carousel-output.schema.json)
4. CRUD calls Claude API. Receives generation-response (JSON).
5. CRUD validates response against schema. If invalid → retry once → mark job failed if still invalid.
6. CRUD optionally calls Claude API again for Quality Gate (prompts/08-critique-output.md).
   - If Quality Gate critical fails → call prompts/09-improve-output.md → re-run Quality Gate (max 2 loops).
7. If render: true and quality passes:
     CRUD calls Claude API for prompts/13-generate-renderable-carousel.md.
     CRUD validates against renderable-carousel.schema.json.
     CRUD posts a render-job to the worker.
     Worker returns render-result with asset URLs.
8. CRUD stores all artifacts. Fires webhook if subscribed.
9. User reviews in CRUD UI. Approves / retries / requests revision.
```

---

## Prompt assembly

The CRUD platform assembles the prompt for each Claude API call from these layers:

1. **System layer:** `system/master-instructions.md` + relevant `system/*.md` files for the module.
2. **Module layer:** `modules/<module>/instructions.md`.
3. **Brand layer:** all 12 files under `brands/<slug>/`. Concatenated or referenced.
4. **Brief layer:** the request payload from `generation-request.schema.json`.
5. **Schema layer:** include the JSON Schema for the expected output as a reference (Claude needs to know what shape to return).
6. **Quality layer:** `system/quality-checklist.md` + `system/safety-and-claims-rules.md`.

Pin the prompt version. Cache aggressively. Each layer is a stable file with a hash; only the brief changes per job.

---

## Multi-tenancy and brand isolation

- Each brand is a separate folder + key in the CRUD database.
- Brand Pack data **never crosses tenants**. The prompt assembled for brand A must contain only A's pack.
- Brand-pack-level secrets (e.g. compliance constraints, internal-only proof) tagged in `constraints.md` and `proof-assets.md` must not leak across brands.
- Logs and analytics partitioned by `brand_id`.

If two brands share an account holder, they're still separate brands. No "merged context."

---

## Versioning

- The repo is tagged (e.g. `v2.0.0`). Each tag is a stable contract.
- Every job records `prompt_version` in its metadata.
- The CRUD platform may pin specific versions per brand or per environment (staging vs prod).
- Schema breaking changes happen only on major versions. Minor / patch releases are backward-compatible.

---

## Failure and retry strategy

| Failure | Strategy |
|---|---|
| Invalid JSON from Claude | Retry once with a stricter system prompt. Then fail with `SCHEMA_VIOLATION`. |
| Schema validation failure | Same as above. |
| Quality Gate critical fail | Auto-repair via `prompts/09-improve-output.md`, max 2 loops. |
| Renderable validation failure | Auto-repair via `prompts/16-repair-renderable-output.md`, max 2 loops. |
| Worker render failure | Return structured error to CRUD; allow CRUD to invoke repair or hand to human. |
| Asset upload failure | Worker retries 3× with backoff before failing the job. |
| Brand Pack missing | Return `MISSING_BRAND_PACK` error; do not generate. |
| Unsafe claim demanded by brief | Return `UNSAFE_CLAIM`; CRUD surfaces to user. |

All failures are structured errors (see `system/api-output-rules.md`).

---

## Webhook events

The CRUD platform may emit webhooks to consumers:

- `job.created`
- `job.generation_complete`
- `job.quality_gate_failed`
- `job.improved`
- `job.renderable_complete`
- `job.render_started`
- `job.render_complete`
- `job.render_failed`
- `job.approved`
- `job.revision_requested`
- `job.failed`

Schema: `integration/schemas/webhook-event.schema.json`.

---

## Why this contract matters

The contract makes three things possible:

1. **Multiple consumers can use the same Claude-side prompts** without forking the repo.
2. **The rendering worker can be swapped** (Playwright today, headless WebKit tomorrow) without touching Claude.
3. **The CRUD platform can swap LLM providers** without re-architecting (the contract is the same; only the model changes).

Keep the boundaries clean. Resist the temptation to put rendering logic in the prompt, or content logic in the worker.

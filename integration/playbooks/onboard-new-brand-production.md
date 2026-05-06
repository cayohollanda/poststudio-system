# Playbook — Onboard a New Brand to API Production

> The first-week sequence to set up a new brand in Mode 2 / 3.

---

## Pre-conditions

- Tenant already exists in CRUD.
- Tenant has a valid API key.
- Tenant has agreed to scope.
- (For Mode 3) Worker is operational; brand-specific assets and fonts will be registered.

---

## Day 0 — Provisioning

1. Create brand record: `POST /brands` with `{ "slug": "...", "name": "..." }`.
2. Generate API key for the brand (or reuse the tenant key, scoped to brand).
3. Create object-storage paths: `s3://crud-bucket/tenants/<id>/brands/<slug>/`.
4. Subscribe webhooks if the consumer wants real-time events.

---

## Day 1 — Brand Pack ingestion

Two paths depending on input quality:

### Path A — Existing pack
The tenant uploads a Brand Pack (12 files) directly via `PUT /brands/<slug>/pack`.

Validate each section against `schemas/brand-pack.schema.json`. Reject sections that fail.

### Path B — Generate from context

If the tenant has only website copy / decks / interview transcripts:

1. Submit a `brand-pack-builder` job with the pasted context as the brief.
2. The result: 12 sections returned in the response.
3. Surface gaps as `[MISSING_INPUT]`; collect from the tenant.
4. Persist the pack (versioned).

### Path C — Interview mode (manual)

If the tenant wants the interview, run it inside Claude Project (Mode 1) — Mode 2 is not interactive. Save the resulting pack and ingest via Path A.

---

## Day 2 — Diagnosis

Submit a `brand-diagnosis-agent` job. Returns scored 12-category audit.

Surface to the tenant. Decide together:

- Which gaps to fix before generating content.
- Which to defer.
- Whether the Brand Pack needs another round of refinement.

If diagnosis surfaces 4+ critical gaps in the Brand Pack → loop back to Day 1 Path B.

---

## Day 3 — Asset registration (Mode 3 only)

If the tenant is using Mode 3:

1. Register the brand's fonts in the worker's font library.
2. Register brand assets (background images, logo wordmarks, illustration sets) in the worker's asset registry.
3. Test render a "hello world" slide: hardcoded HTML + font + asset. Confirm output matches expectation.
4. (Template mode) Register brand-specific templates in the worker.

Until this passes, Mode 3 jobs will fail on font / asset / template lookups.

---

## Day 4 — Content strategy

Submit a `content-strategy-os` job.

Returns:
- 3-5 pillars.
- 2-4 series.
- 30-day plan.

Tenant reviews. Approves. Lock the strategy in the brand record (versioned).

---

## Day 5 — First carousel job

Submit the first `carousel-machine` job for the brand:

- Topic from the strategy (Day 1 of the 30-day plan).
- Run Quality Gate.
- Surface to the tenant for approval.

Mark the brand `production_ready` when this first job completes successfully.

---

## Day 6-7 — First Mode 3 render (if applicable)

Submit the first renderable carousel job. Confirm:

- Renderable schema validates.
- Renderability QG passes.
- Worker renders all slides successfully.
- Asset URLs accessible to the tenant.

If any stage fails, debug per `failure-and-retry-strategy.md`. Document gaps.

---

## Day 8+ — Steady state

Brand is production-ready. Subsequent jobs follow `generate-carousel-end-to-end.md` or `run-quality-gate-before-render.md`.

---

## Onboarding completion checklist

- [ ] Brand record created.
- [ ] Brand Pack ingested (12 sections).
- [ ] Diagnosis run; gaps either fixed or deferred.
- [ ] Content strategy locked.
- [ ] (Mode 3) Fonts registered.
- [ ] (Mode 3) Assets registered.
- [ ] (Mode 3 template mode) Templates registered.
- [ ] First carousel generated and approved.
- [ ] (Mode 3) First render successful.
- [ ] Webhooks subscribed and verified.
- [ ] Tenant trained on the consumer-app flow.

When all boxes check, the brand is `production_ready`.

---

## Onboarding red flags

- Tenant refuses to provide a Brand Pack → engagement should run Mode 1 first, not Mode 2.
- Brand Pack diagnosis returns < 30/60 → fix the foundation before scaling content.
- Worker render fails repeatedly on day 3 → font / asset registry gaps; resolve before steady-state.
- Tenant wants to skip Quality Gate → never agree.

---

## Brand Pack versioning during onboarding

Onboarding typically produces multiple Brand Pack versions:

- v1: from initial ingestion.
- v2: after diagnosis-driven fixes.
- v3: after first cycle of feedback.

Pin jobs to the version that was current at submission time. Don't retro-apply newer versions to in-flight jobs.

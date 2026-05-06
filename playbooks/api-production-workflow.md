# Playbook — API Production Workflow

> Mode 2. The end-to-end production cycle for an external CRUD platform calling Claude API.

The integration-side details live in [`integration/playbooks/`](../integration/playbooks/). This top-level playbook is the operational summary.

---

## Architecture recap

```
Consumer App → CRUD Platform → Claude API (gen) → Claude API (QG) → CRUD storage → consumer
                              ↘ Worker (Mode 3 only)  ↗
```

See [`docs/api-production-mode.md`](../docs/api-production-mode.md).

---

## Daily operational loop

### Job intake
- Consumer app submits a generation job via `POST /jobs/generation`.
- CRUD validates against `integration/schemas/generation-request.schema.json`.
- CRUD enqueues the job; returns `202 + job_id`.

### Generation
- Worker pool picks up the job.
- Assembles prompt per `integration/prompt-assembly-strategy.md`.
- Calls Claude API.
- Validates response against the requested module's schema.
- Stores the response.
- Fires `job.generation_complete` webhook.

### Quality Gate (separate Claude API call)
- Worker calls Claude API with `prompts/08-critique-output.md` + the response as input.
- Different system prompt: the quality-gate module's instructions.
- Validates `quality-review` JSON.
- Stores the review.
- Fires `job.quality_gate_complete` webhook.

### Decision
- `Ship-ready` → mark `approved_for_delivery`; deliver.
- `Ship with flags` → deliver with flags surfaced.
- `Borderline` / `Rewrite required` → invoke improve loop.

### Improve loop (max 2 iterations)
- Worker calls Claude API with `prompts/09-improve-output.md`.
- Re-runs Quality Gate.
- If 2 iterations exhausted → mark `failed_pending_human_review`.

### (Mode 3) Render
- Worker calls Claude API with `prompts/13-generate-renderable-carousel.md`.
- Validates renderable schema.
- Renderability QG.
- Repair loop if needed (max 2).
- Posts render-job to the rendering worker.
- Receives render-result.
- Stores assets.
- Fires `job.render_complete` webhook.

### Delivery
- CRUD marks job `delivered`.
- Fires `job.delivered` webhook.
- Consumer app receives final artifacts.

---

## Common operational patterns

### Onboard a new brand
1. `POST /brands` — create brand record.
2. `PUT /brands/<slug>/pack` — upsert the 12-section Brand Pack (or run `prompts/02-generate-brand-pack.md` to generate).
3. `POST /jobs/generation` with module=`brand-diagnosis-agent` → review.
4. `POST /jobs/generation` with module=`content-strategy-os` → review.
5. Brand is `production_ready`.

### Ship a single carousel
1. `POST /jobs/generation` with module=`carousel-machine` + brief.
2. Subscribe to `job.delivered` webhook.
3. Consume artifact URL when webhook fires.

### Run a 14-day campaign
1. `POST /jobs/generation` with module=`campaign-builder` → returns plan.
2. For each day's post: schedule a `carousel-machine` (or `reels-script-machine`) job.
3. Consume artifacts as webhooks fire.

### Mode 3 render
1. As above, but with `render: true` and `render_mode: "html-css"` (or `template`).
2. Subscribe to `job.render_complete`.
3. Consume PNG URLs.

---

## Operational metrics to track

Per tenant:
- Jobs per hour / day / week.
- Success rate by module.
- Quality Gate critical-fail rate.
- Render fail rate (Mode 3).
- Mean / p95 / p99 latency.
- Cost per job.
- Webhook delivery success rate.

Per brand:
- Total active jobs.
- `prompt_version` distribution (alerts on stale versions).
- Average iterations per job.
- Most-failed checklist items.

See [`integration/observability-and-logging.md`](../integration/observability-and-logging.md).

---

## Alert thresholds

| Trigger | Severity |
|---|---|
| Cross-tenant prompt detected | CRITICAL |
| Webhook signature failures > 1% | CRITICAL |
| Auth bypass attempt | CRITICAL |
| Quality Gate critical-fail rate > 10% | WARN |
| Render fail rate > 5% | WARN |
| p95 latency > 60s | WARN |
| Cost spike > 2x rolling average | WARN |
| Storage usage > 80% | WARN |

---

## Versioning discipline

- Pin `prompt_version` per environment.
- Roll out new versions to staging, validate against the regression corpus, then promote to prod.
- Keep prior version available for 6 months for backward compatibility.
- Track which version each tenant is using; let high-touch tenants opt in to upgrades.

---

## Failure handling

Per [`integration/failure-and-retry-strategy.md`](../integration/failure-and-retry-strategy.md):

- Schema violations: retry once → SCHEMA_VIOLATION.
- Quality Gate critical fails: improve loop max 2.
- Render failures: repair loop max 2.
- Beyond budget: `failed_pending_human_review`.

Persistent failures are *signals*, not just exceptions. Investigate patterns.

---

## Cost discipline

- Cache stable prompt layers (system + module + schema) per repo version.
- Cache Brand Pack per brand.
- Use Anthropic's prompt caching where supported.
- Track cost per job; alert on anomalies.
- Allow tenants to opt out of Quality Gate for low-stakes content (with explicit consent).

---

## Audit trail

Every job logs:
- `tenant_id`, `brand_id`, `job_id`, `request_id`.
- `prompt_version`, `brand_pack_version`.
- Prompt hash (so the assembly is reproducible).
- Model + tokens + duration + cost.
- Result hash.
- All webhook deliveries.

Retained per the platform's audit policy (typically 12+ months).

See [`integration/observability-and-logging.md`](../integration/observability-and-logging.md).

---

## Capacity planning

Per worker pool:
- Concurrent generation jobs (Claude API).
- Concurrent QG jobs (separate calls).
- Concurrent render jobs (worker capacity).

Scale horizontally. Each layer is stateless per job.

---

## On-call playbook

When alerts fire:

1. Read the metric. What's anomalous?
2. Check error rates by tenant / brand / module.
3. Pull recent failed jobs; reproduce locally.
4. If Claude API issue: check Anthropic status; consider failover model.
5. If worker issue: check worker logs; restart pool if needed.
6. If schema issue: check recent prompt_version changes; consider rollback.
7. Post-incident: document, update regression corpus.

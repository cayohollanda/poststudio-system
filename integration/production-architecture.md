# Production Architecture

> The full Mode 2 + Mode 3 architecture. Implementation-level reference for the CRUD platform.

---

## High-level flow

```
┌──────────────┐
│ Consumer App │  (dashboard, internal tool, scheduler, embed)
└──────┬───────┘
       │ HTTP
       ▼
┌──────────────────────────────────────────────────────────┐
│ CRUD Platform                                             │
│  - Brand store        - Job orchestration                │
│  - Briefing store     - Schema validation                │
│  - Output store       - Quality Gate dispatcher          │
│  - Asset store        - Render dispatcher                │
│  - Webhook fanout     - Audit log                        │
└──┬──────────────────┬───────────────────┬────────────────┘
   │                  │                   │
   │ Claude API       │ Quality Gate API  │ Worker API
   ▼                  ▼                   ▼
┌─────────────┐  ┌─────────────┐  ┌────────────────┐
│ Claude      │  │ Claude      │  │ Rendering      │
│ (Generation)│  │ (Reviewer)  │  │ Worker         │
└─────────────┘  └─────────────┘  └────┬───────────┘
                                       │
                                       ▼
                                 ┌──────────────┐
                                 │ Object Store │
                                 │ (S3/R2/GCS)  │
                                 └──────────────┘
```

---

## Components

### Consumer App
- The user-facing UI. Could be a SaaS dashboard, an internal tool, or an embed inside another product.
- Talks to the CRUD platform via HTTPS.
- Does not call Claude or the worker directly.

### CRUD Platform
- The system of record.
- Stores brands, Brand Packs, briefings, jobs, outputs, render results.
- Orchestrates Claude API calls and worker calls.
- Validates everything against schemas.
- Owns the audit log.

### Claude (Generation)
- Stateless per call.
- Receives assembled prompt + brief.
- Returns JSON conforming to the requested module's schema.
- Returns `prompt_version`, `request_id`, `missing_inputs`, `warnings`, `errors`.

### Claude (Reviewer)
- Same model, called separately.
- Receives the generation output + brand pack + critique prompt.
- Returns `quality-review` JSON.
- Never the same context as the generation call.

### Rendering Worker
- Stateless per render job.
- Receives renderable-carousel JSON.
- Validates, sanitizes, renders, optimizes, uploads, returns asset URLs.
- May invoke Claude (via the CRUD platform) for repair if needed.

### Object Store
- Stores rendered PNGs and any generated image assets.
- Versioned by `job_id`.
- Accessible to the consumer app via signed URLs.

---

## Sequence diagram (Mode 2 + 3)

```
Consumer       CRUD              Claude(Gen)       Claude(QGate)     Worker          Object Store
   │            │                    │                  │              │                │
   │ POST job   │                    │                  │              │                │
   │───────────▶│                    │                  │              │                │
   │            │ validate request   │                  │              │                │
   │            │ assemble prompt    │                  │              │                │
   │            │ POST API ─────────▶│                  │              │                │
   │            │◀─────────── JSON   │                  │              │                │
   │            │ validate schema    │                  │              │                │
   │            │ POST API ────────────────────────────▶│              │                │
   │            │◀──────────────────── quality-review   │              │                │
   │            │ if pass: continue                     │              │                │
   │            │ if fail: improve loop                 │              │                │
   │            │                    │                  │              │                │
   │            │ if render: true:                      │              │                │
   │            │ POST renderable ──▶│                  │              │                │
   │            │◀──── renderable    │                  │              │                │
   │            │ POST render ─────────────────────────────────────────▶                │
   │            │                                                       │ render        │
   │            │                                                       │ upload ──────▶│
   │            │◀────── render-result + asset URLs ────────────────────│                │
   │            │ store everything   │                  │              │                │
   │◀───── 200  │                    │                  │              │                │
   │ + webhook  │                    │                  │              │                │
   │            │                    │                  │              │                │
```

---

## Latency expectations

| Stage | Typical latency |
|---|---|
| Schema validation | < 50ms |
| Prompt assembly | < 100ms |
| Claude generation call | 8-25s (carousel); 15-45s (renderable) |
| Quality Gate call | 5-15s |
| Improve loop (if needed) | +8-25s per iteration |
| Render dispatch | < 200ms |
| Worker render (per slide) | 1-3s |
| Worker upload | 0.5-2s per asset |
| Webhook fanout | < 500ms |

A full Mode 3 carousel job: typically 60-180 seconds end-to-end.

---

## Async vs sync

Generation should be async by default. The CRUD platform:

- Accepts the request, returns `202 Accepted` with `job_id`.
- Processes in a worker pool.
- Notifies via webhook on completion.

Sync API is acceptable for short jobs (< 30s) and direct user-facing flows, but async is the safer default.

---

## Scaling considerations

- **Brand isolation:** assemble prompt per brand; cache stable layers (system, module, schema). Only the brief layer changes per request.
- **Multi-tenant safety:** never share brand context across tenants. See [`multi-tenant-boundaries.md`](multi-tenant-boundaries.md).
- **Rate limits:** respect Claude API rate limits per tenant; queue overflow.
- **Worker capacity:** render workers are stateless; scale horizontally.
- **Cost control:** Quality Gate is a separate API call — track cost per job; for low-stakes content, allow opt-out.

---

## Failure boundaries

The architecture isolates failures:

- A bad Brand Pack fails the assembly, not the API.
- A bad Claude response fails the schema validation, not the storage.
- A bad render fails the render-job, not the generation.
- A bad worker upload fails the asset, not the metadata.

Each layer fails loud with structured errors. See [`failure-and-retry-strategy.md`](failure-and-retry-strategy.md).

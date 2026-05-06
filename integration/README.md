# Integration Layer

> The contract between PostStudio System and an external CRUD platform that orchestrates Claude API calls and (optionally) a rendering worker.

This layer makes Mode 2 (API Production) and Mode 3 (Renderable Creative Worker) possible without putting any orchestration logic in the prompt files themselves.

---

## When to read this

- You're building (or wiring) an external CRUD platform that calls Claude API.
- You're scaling a multi-brand content engine.
- You want webhooks instead of polling.
- You need to integrate a rendering worker.

If you're using PostStudio System inside a Claude Project (Mode 1) only — you don't need this layer.

---

## What's in this folder

```
integration/
├── README.md                              # this file
├── production-architecture.md             # the high-level architecture
├── claude-api-mode.md                     # how to call Claude API with this repo
├── context-loading-strategy.md            # which files to load per request
├── prompt-assembly-strategy.md            # how to assemble the prompt
├── async-job-lifecycle.md                 # job states and transitions
├── external-crud-platform-contract.md     # the API the CRUD platform exposes
├── partner-creative-api.md                # how the CRUD platform talks to the rendering worker
├── webhook-contract.md                    # event shapes
├── failure-and-retry-strategy.md          # when to retry, when to fail
├── multi-tenant-boundaries.md             # how to isolate brands
├── storage-model.md                       # what to store and where
├── observability-and-logging.md           # what to log + alert on
├── security-and-secrets.md                # secrets handling
├── schemas/                               # JSON schemas for the contract
│   ├── generation-request.schema.json
│   ├── generation-response.schema.json
│   ├── creative-render-request.schema.json
│   ├── creative-render-response.schema.json
│   ├── job-status.schema.json
│   └── webhook-event.schema.json
├── examples/                              # canonical request/response examples
│   ├── generate-carousel-request.json
│   ├── generate-carousel-response.json
│   ├── creative-render-request.json
│   ├── creative-render-response.json
│   └── webhook-completed-event.json
└── playbooks/                             # runbook-style end-to-end procedures
    ├── generate-carousel-end-to-end.md
    ├── run-quality-gate-before-render.md
    ├── handle-missing-proof.md
    ├── retry-low-quality-output.md
    └── onboard-new-brand-production.md
```

---

## Three roles, three responsibilities

```
┌─────────────────────────────┐    ┌─────────────────────────────┐    ┌─────────────────────────────┐
│  External CRUD Platform     │    │  Claude API (PostStudio)    │    │  Rendering Worker            │
│  - the system of record     │    │  - the content engine       │    │  - the asset producer        │
│  - storage + jobs + UI      │    │  - the brand voice          │    │  - HTML/SVG/JSON → PNG       │
└─────────────────────────────┘    └─────────────────────────────┘    └─────────────────────────────┘
```

See [`system/integration-contract.md`](../system/integration-contract.md) for the full role definition. This `integration/` folder is the *implementation guide* for the CRUD platform side.

---

## The minimum viable integration

For a basic Mode 2 setup:

1. CRUD stores brands and Brand Packs.
2. CRUD validates each request against [`schemas/generation-request.schema.json`](schemas/generation-request.schema.json).
3. CRUD assembles the prompt per [`prompt-assembly-strategy.md`](prompt-assembly-strategy.md).
4. CRUD calls Claude API. Validates response against the appropriate schema.
5. CRUD optionally calls Quality Gate (separate API call).
6. CRUD stores final output keyed by `brand_id` + `job_id`.

For Mode 3 (rendering), add:

7. CRUD calls Claude API for renderable carousel.
8. CRUD posts a render-job to the worker (per [`partner-creative-api.md`](partner-creative-api.md)).
9. Worker returns render-result; CRUD stores final asset URLs.
10. CRUD fires `job.render_complete` webhook.

---

## The 5 minimum API operations

The CRUD platform must implement at least:

1. `POST /jobs/generation` — create a generation job.
2. `GET /jobs/{id}` — get job status.
3. `POST /jobs/{id}/quality-gate` — run Quality Gate as a separate pass.
4. `POST /jobs/{id}/render` — dispatch a render job (Mode 3).
5. `POST /jobs/{id}/approve` — approve / reject final output.

See [`external-crud-platform-contract.md`](external-crud-platform-contract.md).

---

## Versioning

- This integration layer is versioned with the repo (`v2.0.0`).
- Every job records `prompt_version`.
- Schemas are stable across minor versions; major versions may break.

---

## Related

- System contract: [`system/integration-contract.md`](../system/integration-contract.md)
- API output rules (for Claude): [`system/api-output-rules.md`](../system/api-output-rules.md)
- Operating system overview: [`system/operating-system.md`](../system/operating-system.md)
- Rendering layer: [`rendering/architecture.md`](../rendering/architecture.md)

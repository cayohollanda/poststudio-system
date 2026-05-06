# Rendering Architecture

> The high-level architecture of the rendering worker. Stateless. Horizontally scalable. Network-isolated.

---

## Components

```
┌─────────────────────────────────────────────────────────────────┐
│ Worker process                                                  │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────┐ │
│  │  Job intake      │→ │  Validator       │→ │  Sanitizer   │ │
│  │  (HTTP / queue)  │  │  (schema)        │  │  (HTML/SVG)  │ │
│  └──────────────────┘  └──────────────────┘  └──────┬───────┘ │
│                                                     ▼          │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────┐ │
│  │  Asset resolver  │← │  Renderer        │← │  Asset/font  │ │
│  │  (registry)      │  │  (Playwright/SVG)│  │  registry    │ │
│  └─────────┬────────┘  └─────────┬────────┘  └──────────────┘ │
│            ▼                     ▼                             │
│  ┌──────────────────┐  ┌──────────────────┐                    │
│  │  Optimizer       │→ │  Uploader        │→ Object Storage    │
│  │  (sharp)         │  │  (S3/R2/GCS)     │                    │
│  └──────────────────┘  └──────────────────┘                    │
└─────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
                       ┌──────────────────┐
                       │  Callback / queue │
                       │  → CRUD platform  │
                       └──────────────────┘
```

---

## Stages

### 1. Job intake

Receives a render job via HTTP `POST /render-jobs` or via a queue (SQS, RabbitMQ, etc.). Conforms to `integration/schemas/creative-render-request.schema.json`.

### 2. Validator

Validates the `renderable_carousel` against `schemas/renderable-carousel.schema.json`. Rejects malformed input.

### 3. Sanitizer

Per [`safety-and-sanitization.md`](safety-and-sanitization.md):
- Strip `<script>` tags.
- Strip `on*=` handlers.
- Strip `javascript:` URIs.
- Strip remote URLs.
- Validate inline `data:` URI sizes.

### 4. Asset resolver

Replaces `{{asset:<id>}}` placeholders with real URLs from the worker's asset registry. If an asset doesn't resolve → error per slide.

### 5. Renderer

Three implementations based on sub-mode:
- **Freeform HTML/CSS** → headless Chromium / Playwright.
- **SVG** → SVG renderer (`resvg`, browser-based, etc.).
- **Template-driven** → template engine + slot data → renderer of the template's choice.

### 6. Optimizer

`sharp` (Node) or equivalent. Compresses PNG/JPG without visible loss.

### 7. Uploader

Uploads to object storage with deterministic paths:

```
s3://crud-bucket/tenants/<id>/brands/<slug>/jobs/<job_id>/assets/slide-<n>.png
```

Returns signed URLs (or public URLs depending on policy).

### 8. Callback

Returns `creative-render-response` to the CRUD platform's callback URL or via a polling endpoint.

---

## Concurrency

- Worker handles N jobs concurrently (configurable per worker process).
- Each job renders M slides concurrently (configurable per job).
- Per-slide rendering is CPU/IO-heavy; tune to host capacity.

Recommended starting point:

| Host size | Concurrent jobs | Concurrent slides per job |
|---|---|---|
| 4 vCPU, 8GB | 2 | 2 |
| 8 vCPU, 16GB | 4 | 4 |
| 16 vCPU, 32GB | 8 | 4 |

Scale horizontally beyond that.

---

## Network isolation

The renderer process should run in a sandbox:

- No outbound network (or restricted to known asset CDNs).
- No filesystem access outside the job's working directory.
- Memory limits per job.
- CPU limits per job.

Headless Chromium specifically:

- `--no-sandbox` only if the host OS provides equivalent isolation (containers, microVMs).
- Disable JavaScript globally: `--disable-javascript` (we don't allow JS in renderable output anyway).
- Disable plugins, extensions, GPU.

---

## Observability

Per render job, log:
- Job ID, tenant, brand.
- Sub-mode.
- Per-slide duration.
- Total duration.
- Asset upload sizes.
- Errors.
- Worker version.

---

## Deployment

- Containerized worker (Docker / OCI).
- Stateless — easy to autoscale.
- Health endpoint for orchestration.
- Graceful shutdown: drain in-flight jobs before termination.

---

## Failure boundaries

- A bad slide fails *that slide*, not the whole job.
- A worker crash on slide 4 → CRUD platform retries the slide (or the whole job, depending on policy).
- An asset 404 → slide fails with `ASSET_NOT_FOUND`.
- A template lookup miss → slide fails with `TEMPLATE_NOT_FOUND`.

See [`failure-and-retry-strategy.md`](failure-and-retry-strategy.md).

---

## Versioning

The worker has its own version tag (`renderer-v1.4.2`). The version is included in every render result so the CRUD platform can correlate with worker behavior.

Worker upgrades are backward-compatible within the renderable schema's major version.

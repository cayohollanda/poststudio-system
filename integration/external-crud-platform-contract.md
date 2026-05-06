# External CRUD Platform Contract

> The minimum API surface the CRUD platform exposes (to the consumer app and to internal services).

---

## The 5 minimum operations

### 1. `POST /jobs/generation`

Create a generation job.

**Body:** conforms to `integration/schemas/generation-request.schema.json`.

**Response:** `202 Accepted` + `{ "job_id": "...", "status": "queued" }`.

### 2. `GET /jobs/{id}`

Get job status + artifacts.

**Response:** `{ "job_id": "...", "status": "...", "artifacts": [...], ... }` per `integration/schemas/job-status.schema.json`.

### 3. `POST /jobs/{id}/quality-gate`

Run Quality Gate as a separate pass on an existing job's output.

**Body:** `{ "iteration": 1 }`.

**Response:** quality-review JSON conforming to `schemas/quality-review.schema.json`.

### 4. `POST /jobs/{id}/render`

Dispatch a render job for an existing renderable carousel.

**Body:** `{ "canvas": { "width": 1080, "height": 1350, "format": "png" } }`.

**Response:** `202 Accepted`. Final URLs delivered via webhook.

### 5. `POST /jobs/{id}/approve`

Approve / reject final output.

**Body:** `{ "decision": "approved" | "rejected", "notes": "..." }`.

**Response:** updated status.

---

## Extended operations (recommended)

| Op | Purpose |
|---|---|
| `POST /brands` | Create a brand record. |
| `PUT /brands/{slug}/pack` | Upsert Brand Pack (12 sections). |
| `GET /brands/{slug}/pack` | Get current Brand Pack. |
| `POST /brands/{slug}/diagnose` | Run brand diagnosis. |
| `POST /jobs/{id}/improve` | Run improve loop. |
| `POST /jobs/{id}/cancel` | Cancel an in-flight job. |
| `POST /jobs/{id}/revisions` | Create a revision request. |
| `GET /jobs/{id}/artifacts/{name}` | Fetch a specific artifact. |

---

## Authentication

- API keys per tenant (the consumer app).
- API keys per brand (optional; for fine-grained scoping).
- All requests over HTTPS.
- Webhook deliveries signed (HMAC-SHA256 with shared secret).

---

## Rate limits (suggested)

- Per tenant: 60 jobs/hour, burst 10/min.
- Per brand: 30 jobs/hour.
- Per user (if multi-user tenants): configurable.

Higher limits negotiated per engagement.

---

## Job inspection

`GET /jobs/{id}` returns a job object:

```json
{
  "job_id": "...",
  "tenant_id": "...",
  "brand_id": "...",
  "module": "carousel-machine",
  "status": "render_complete",
  "created_at": "...",
  "updated_at": "...",
  "prompt_version": "v2.0.0",
  "request": { /* echoed */ },
  "artifacts": [
    { "name": "carousel.json", "url": "...", "type": "json" },
    { "name": "carousel.md", "url": "...", "type": "markdown" },
    { "name": "renderable.json", "url": "...", "type": "json" },
    { "name": "quality-review.json", "url": "...", "type": "json" },
    { "name": "slide-1.png", "url": "...", "type": "image" },
    { "name": "slide-2.png", "url": "...", "type": "image" }
  ],
  "errors": [],
  "warnings": []
}
```

---

## Webhooks

The platform supports webhooks on job state changes. See [`webhook-contract.md`](webhook-contract.md).

Webhook subscription:

```
POST /webhooks
{
  "url": "https://consumer.example/webhook",
  "events": ["job.render_complete", "job.failed"],
  "secret": "<shared secret for signing>"
}
```

---

## Error responses

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "...",
    "field": "brand_id",
    "request_id": "..."
  }
}
```

Standard codes:
- `400 VALIDATION_ERROR`
- `401 UNAUTHENTICATED`
- `403 FORBIDDEN`
- `404 NOT_FOUND`
- `409 CONFLICT` (e.g. duplicate job_id)
- `422 SCHEMA_VIOLATION`
- `429 RATE_LIMITED`
- `500 INTERNAL_ERROR`
- `503 UPSTREAM_UNAVAILABLE` (Claude API or worker)

---

## Idempotency

Clients send `Idempotency-Key` header. The platform de-duplicates within a 24h window.

---

## API versioning

URL prefix per major version: `/v1/jobs/...`.

Breaking changes → new prefix. Backward compatibility maintained for 6 months on the prior version.

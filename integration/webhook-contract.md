# Webhook Contract

> Event shapes the CRUD platform fires to subscribed consumers.

---

## Events

| Event | When |
|---|---|
| `job.created` | Job created and validated. |
| `job.generation_complete` | Generation response received. |
| `job.quality_gate_complete` | Quality Gate pass complete (any verdict). |
| `job.quality_gate_failed` | Quality Gate critical fail; improve loop triggered. |
| `job.improved` | Improve loop completed an iteration. |
| `job.renderable_complete` | Renderable carousel JSON ready. |
| `job.render_started` | Worker accepted the render job. |
| `job.render_complete` | Worker returned final assets. |
| `job.render_failed` | Worker render failed; repair loop triggered. |
| `job.approved` | User approved the output. |
| `job.revision_requested` | User requested revisions. |
| `job.delivered` | Final asset delivered to consumer (last event). |
| `job.failed` | Terminal failure. |
| `job.cancelled` | Cancelled. |

---

## Envelope

Every webhook payload:

```json
{
  "event": "job.render_complete",
  "delivery_id": "<uuid>",
  "delivered_at": "2026-05-06T12:34:56Z",
  "tenant_id": "<id>",
  "brand_id": "<id>",
  "job_id": "<uuid>",
  "prompt_version": "v2.0.0",
  "data": { /* event-specific */ }
}
```

---

## Per-event `data` shapes

### `job.created`
```json
{ "module": "carousel-machine", "request": { /* echoed */ } }
```

### `job.generation_complete`
```json
{ "module": "carousel-machine", "artifact_url": "...", "missing_inputs": [] }
```

### `job.quality_gate_complete`
```json
{ "verdict": "Ship-ready", "score": "29/30", "iteration": 1, "artifact_url": "..." }
```

### `job.renderable_complete`
```json
{ "render_mode": "html-css", "slide_count": 8, "renderability_validated": true, "artifact_url": "..." }
```

### `job.render_complete`
```json
{
  "slide_count": 8,
  "duration_ms": 8421,
  "slides": [
    { "slide_number": 1, "asset_url": "...", "size_bytes": 412988 },
    { "slide_number": 2, "asset_url": "...", "size_bytes": 398124 }
  ]
}
```

### `job.failed`
```json
{ "stage": "renderable_generating", "error_code": "UNSAFE_RENDERABLE", "message": "..." }
```

### `job.delivered`
```json
{ "delivery_method": "webhook" | "api_polling", "final_artifact_urls": [...] }
```

---

## Signing

Each webhook delivery signed with HMAC-SHA256 using the subscription's shared secret:

```
Header: X-PostStudio-Signature: sha256=<hex>
```

Compute: `HMAC-SHA256(secret, "<delivery_id>.<timestamp>.<body>")`.

Consumers verify before acting.

---

## Delivery semantics

- **At-least-once delivery.** Consumers must be idempotent (use `delivery_id`).
- **Retries:** exponential backoff on 5xx / timeouts. Max 5 retries over 24h.
- **Order:** not guaranteed. Use `delivered_at` and event types to reconstruct.

---

## Subscription management

```
POST /webhooks
{
  "url": "https://consumer.example/webhook",
  "events": ["job.render_complete", "job.failed"],
  "secret": "<shared secret>"
}

GET /webhooks
DELETE /webhooks/{id}
```

A consumer can have multiple subscriptions filtered by event types.

---

## Best practices for consumers

- Verify signature on every delivery.
- Use `delivery_id` for idempotency.
- Respond `2xx` quickly; do heavy processing async.
- Store the raw payload for audit.
- Re-fetch `GET /jobs/{id}` for the authoritative state if the webhook is stale.

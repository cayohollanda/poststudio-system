# Partner Creative API — CRUD ↔ Rendering Worker

> The contract between the CRUD platform and the rendering worker. Stateless. Idempotent. Versioned.

---

## Purpose

The CRUD platform owns content + brand context. The rendering worker owns asset production. They communicate through a small, well-defined API.

The worker should know nothing about brands, briefs, or campaigns. It receives a *renderable carousel* and returns *rendered assets*.

---

## Endpoints

### `POST /render-jobs`

Submit a render job.

**Body:** conforms to `integration/schemas/creative-render-request.schema.json`.

```json
{
  "render_job_id": "<uuid>",
  "callback_url": "https://crud.example/render-jobs/<id>/callback",
  "renderable_carousel": { /* the renderable-carousel.schema.json blob */ },
  "options": {
    "format": "png",
    "quality": "high",
    "concurrency": 4
  }
}
```

**Response:** `202 Accepted` + `{ "render_job_id": "...", "status": "queued" }`.

### `GET /render-jobs/{id}`

Get render job status + asset URLs.

**Response:** conforms to `integration/schemas/creative-render-response.schema.json`.

```json
{
  "render_job_id": "...",
  "status": "complete",
  "rendered_at": "...",
  "duration_ms": 8421,
  "slides": [
    { "slide_number": 1, "asset_url": "...", "width": 1080, "height": 1350, "size_bytes": 412988 },
    { "slide_number": 2, "asset_url": "...", "width": 1080, "height": 1350, "size_bytes": 398124 }
  ],
  "errors": [],
  "warnings": []
}
```

### `POST /render-jobs/{id}/cancel`

Cancel a render in flight.

---

## Worker behavior

On receiving a render job, the worker:

1. **Validates** the renderable_carousel against `schemas/renderable-carousel.schema.json`.
2. **Sanitizes** HTML/CSS/SVG per `rendering/safety-and-sanitization.md`.
3. **Resolves** `{{asset:<id>}}` placeholders to real URLs from the worker's asset registry.
4. **Resolves** font names from the worker's font registry.
5. **Renders** each slide:
   - HTML/CSS → headless Chromium screenshot.
   - SVG → SVG-to-PNG converter.
   - Template → template engine + slot data → screenshot.
6. **Optimizes** PNGs (e.g. `sharp`).
7. **Uploads** to object storage.
8. **Returns** `creative-render-response` to the callback URL or polling endpoint.

If any step fails, the worker returns a structured error per slide.

---

## Failure handling

```json
{
  "render_job_id": "...",
  "status": "partial_failure",
  "slides": [
    { "slide_number": 1, "status": "rendered", "asset_url": "..." },
    { "slide_number": 2, "status": "rendered", "asset_url": "..." },
    {
      "slide_number": 3,
      "status": "failed",
      "errors": [
        {
          "code": "TEXT_OVERFLOW",
          "message": "Headline at 76px overflows canvas by 28px right.",
          "details": { "rendered_width_px": 1108, "canvas_width_px": 1080 }
        }
      ]
    }
  ],
  "warnings": []
}
```

The CRUD platform decides whether to:
- Accept partial success (mark job partially complete).
- Trigger repair via `prompts/16-repair-renderable-output.md`.
- Mark the job failed.

---

## Authentication

- mTLS or shared API key.
- Callback URL signed (HMAC-SHA256 with worker-CRUD shared secret).

---

## Idempotency

`render_job_id` is idempotent. Re-posting with the same ID returns the existing job's state.

---

## Worker rate limits

Worker capacity is finite. Suggested limits:

- 50 concurrent renders.
- Burst of 20 jobs/min.
- Backpressure: if queue depth > 100, return `503` and a `Retry-After` header.

---

## Worker stateless guarantee

The worker is **stateless per job**. It does not:

- Remember previous renders.
- Store brand context.
- Maintain a queue of "user preferences."

If the worker needs context (e.g. fonts, asset library), it pulls from its own registry — independent of the CRUD platform.

---

## What the CRUD platform must NOT do

- Modify the renderable_carousel before sending (must match what Claude returned and what was QGate-approved).
- Add fields not in the schema.
- Pass real customer data through the worker (the worker only needs renderable code + asset IDs).

---

## What the worker must NOT do

- Generate content.
- Choose visual mode or accent color.
- Modify slot values in template mode.
- Cache renders across jobs (each render is fresh).
- Retain renderable_carousel JSON beyond the job's life cycle.

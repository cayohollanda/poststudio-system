# Rendering Failure and Retry Strategy

> Worker-side failure handling. Per-slide granularity. Structured errors.

The integration-layer failure strategy lives in [`integration/failure-and-retry-strategy.md`](../integration/failure-and-retry-strategy.md). This file covers the *worker's* retries and recovery patterns.

---

## Failure granularity

A render job has N slides. Each slide can:

- Render successfully.
- Fail with a structured error.
- Render with warnings.

The worker reports per-slide status. The CRUD platform decides whether to:

- Accept partial success.
- Trigger repair via Claude API (`prompts/16-repair-renderable-output.md`).
- Mark the whole job failed.

---

## Failure codes

| Code | Cause | Worker action |
|---|---|---|
| `TEXT_OVERFLOW` | Headline / body overflows canvas at chosen font-size | Render anyway; emit error with rendered_width_px detail |
| `FONT_NOT_AVAILABLE` | Font referenced not in registry | Substitute via fallback chain; emit warning |
| `ASSET_NOT_FOUND` | `{{asset:<id>}}` doesn't resolve | Strip asset; flat color fallback; emit warning |
| `ASSET_FETCH_FAILED` | Resolved URL returned 4xx/5xx | Retry once; then strip; emit error |
| `UNSAFE_HTML_FEATURE` | Sanitizer stripped a load-bearing feature | Render anyway; emit warning. If un-renderable → emit error |
| `INVALID_SVG` | SVG fails to parse after sanitization | Slide fails; emit error |
| `INVALID_HTML` | HTML fails to render after sanitization | Slide fails; emit error |
| `TEMPLATE_NOT_FOUND` | template_id not in worker's library | Slide fails; emit error |
| `SLOT_VIOLATION` | Slot value violates type/length | Slide fails; emit error |
| `RENDER_TIMEOUT` | Render exceeded per-slide timeout (default 30s) | Slide fails; emit error |
| `UPLOAD_FAILED` | Optimized PNG failed to upload | Retry 3× with backoff; then slide fails |
| `IMAGE_LOAD_FAILED` | Inline image (data: URI) is malformed | Strip; fallback; emit warning |

---

## Retry budgets

| Failure | Worker retry |
|---|---|
| `ASSET_FETCH_FAILED` | 1 retry with backoff |
| `RENDER_TIMEOUT` | 1 retry (some flakiness in headless Chromium) |
| `UPLOAD_FAILED` | 3 retries with exponential backoff |
| All other failures | 0 retries — fail the slide loud |

Beyond worker retries, repair loops happen via Claude API (orchestrated by the CRUD platform).

---

## Per-slide isolation

A failed slide does NOT fail the whole job:

```json
{
  "render_job_id": "...",
  "status": "partial_failure",
  "slides": [
    { "slide_number": 1, "status": "rendered", ... },
    { "slide_number": 2, "status": "rendered", ... },
    {
      "slide_number": 3,
      "status": "failed",
      "errors": [{ "code": "TEXT_OVERFLOW", "message": "...", "details": { "rendered_width_px": 1108 } }]
    },
    { "slide_number": 4, "status": "rendered", ... },
    ...
  ]
}
```

The CRUD platform sees `partial_failure` and decides:

- Accept (rare; most consumers want full carousels).
- Repair via Claude API (use `prompts/16-repair-renderable-output.md` on the broken slide).
- Re-render after repair.
- Or mark job failed and surface to user.

---

## Worker-side resource limits

Per slide:

- Memory: 1GB.
- CPU: 1 core (configurable).
- Timeout: 30s (configurable).

Per job:

- Memory: 4GB total.
- Timeout: 5min total.

If a job exceeds these → kill processes; emit `RENDER_TIMEOUT` per affected slide.

---

## Worker process restart

Headless Chromium can leak memory. Worker behavior:

- Restart browser every N renders (default: 100).
- Restart immediately on segfault / OOM.
- Drain in-flight jobs before shutdown.

Don't fail in-flight jobs on restart; resume.

---

## Asset CDN failures

If the asset CDN is partially down:

- Worker retries 1× per asset.
- Worker tries cache (if it has one).
- Falls back to flat color.
- Emits warning (not error if a fallback is acceptable).

If too many assets fail in one job (> 50%), mark the job `failed` with `IMAGE_LOAD_FAILED` and let the CRUD platform decide.

---

## Repair loop coordination

When a slide fails:

```
Worker → CRUD platform: render-result with partial_failure
CRUD platform → Claude API: prompts/16-repair-renderable-output.md
Claude API → CRUD platform: repaired renderable JSON for affected slides
CRUD platform → Worker: new render-job for repaired slides only
```

Worker accepts partial render-jobs (only some slides). The output uses the same `job_id` so the final result merges with the prior partial.

Max repair iterations: 2. Beyond that → `failed_pending_human_review`.

---

## Error reporting discipline

Every error includes:

- `code` (from the table above).
- `message` (human-readable).
- `details` (machine-readable: rendered_width_px, asset_id, font_family, etc.).

Don't log raw HTML/CSS payloads in errors (sanitization-stripped versions OK).

---

## Audit log

Every render job logs:

- Total duration.
- Per-slide duration.
- Sanitizer actions taken.
- Asset resolutions.
- Font substitutions.
- Errors and warnings.
- Output asset checksums.

Retained per the CRUD platform's audit policy.

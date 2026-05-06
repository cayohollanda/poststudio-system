# Playbook — End-to-End Rendering Workflow

> Worker-side runbook. From "received render job" to "asset URLs returned."

---

## Trigger

`POST /render-jobs` arrives at the worker with a `creative-render-request` payload.

---

## Stage 1 — Validate

1. Authenticate the request (mTLS or shared secret).
2. Validate against `schemas/render-job.schema.json`.
3. Confirm tenant + brand are recognized; reject if not.
4. Extract `renderable_carousel`. Validate against `schemas/renderable-carousel.schema.json`.
5. Confirm `renderability_validated === true`.

If any fail → return `400` with structured error. Don't queue.

---

## Stage 2 — Sanitize

For each slide payload:

1. Run HTML / SVG / template-driven sanitizer per [`safety-and-sanitization.md`](../safety-and-sanitization.md).
2. Strip banned features. Log strips.
3. Validate the sanitized payload still renders.
4. If sanitization breaks the slide → mark slide failed with `UNSAFE_HTML_FEATURE`.

---

## Stage 3 — Resolve

For each slide:

1. Resolve `{{asset:<id>}}` placeholders against the brand's asset registry.
2. Validate asset URLs (allowed domains).
3. Pre-fetch assets if using inline-data strategy.
4. Resolve font references against the worker's font registry.
5. Inject `@font-face` rules for HTML/CSS; embed font files for SVG renderer.
6. (Template mode) resolve `template_id` against the brand's template library.

---

## Stage 4 — Render

For each slide (concurrent, bounded):

### HTML/CSS path
1. Launch / reuse a `chromium` page.
2. Set viewport to canvas dimensions.
3. Set HTML content.
4. Wait for fonts to load.
5. Screenshot at canvas size.
6. Close page.

### SVG path
1. Pass SVG to renderer (resvg / browser).
2. Render at canvas dimensions.
3. Output PNG.

### Template path
1. Look up template_id.
2. Fill slot data.
3. Render via the template engine's path (which will be one of the above).

Capture per-slide duration.

---

## Stage 5 — Optimize

For each rendered slide:

1. Run `sharp` (or equivalent) per [`sharp-optimization.md`](../sharp-optimization.md).
2. Apply quality settings per request's `options.quality`.
3. Verify final dimensions.
4. Compute SHA-256 checksum.

---

## Stage 6 — Upload

For each optimized slide:

1. Upload to object storage at deterministic path:
   ```
   s3://crud-bucket/tenants/<id>/brands/<slug>/jobs/<job_id>/assets/slide-<n>.png
   ```
2. Set appropriate ACL (private with signed URL access).
3. Capture asset URL.
4. Retry 3× with backoff on upload failure.

---

## Stage 7 — Build response

1. Aggregate per-slide results.
2. Compute total duration.
3. Compute metrics (substitutions, failures, total bytes).
4. Determine status:
   - `complete` — all slides rendered.
   - `partial_failure` — some failed.
   - `failed` — none rendered.
5. Build `render-result` payload.

---

## Stage 8 — Callback

1. POST to `callback_url` with the `render-result` payload.
2. Sign the request body with HMAC-SHA256.
3. On 4xx/5xx response from CRUD, retry per webhook policy.

---

## Stage 9 — Audit

Log:
- `render_job_id`, `tenant_id`, `brand_id`.
- Total duration, per-slide durations.
- Sanitizer actions.
- Asset / font substitutions.
- Errors and warnings.
- Final asset checksums.

Retain per audit policy.

---

## Total latency budget

| Stage | Typical |
|---|---|
| Validate | < 100ms |
| Sanitize (per slide) | 5-50ms |
| Resolve (per slide) | 100-500ms (if pre-fetching assets) |
| Render (per slide) | 1-3s |
| Optimize (per slide) | 100-400ms |
| Upload (per slide) | 0.5-2s |
| Build + callback | < 500ms |

Per-job (8 slides, concurrent 4): typically 30-90 seconds.

---

## Failure paths

| Stage | Failure | Action |
|---|---|---|
| 1 | Schema invalid | 400 to CRUD; no queue |
| 2 | Sanitizer breaks slide | Slide fails with `UNSAFE_HTML_FEATURE` |
| 3 | Asset 404 | Slide fails with `ASSET_FETCH_FAILED` (after 1 retry) |
| 3 | Font missing | Substitute via fallback; warning |
| 4 | Render timeout | Retry once; then slide fails with `RENDER_TIMEOUT` |
| 5 | Optimize fails | Skip optimization; ship raw render with warning |
| 6 | Upload fails | Retry 3× with backoff; then slide fails |
| 8 | Callback fails | Retry per webhook policy; alert ops |

---

## Stage skip logic

For repair jobs (re-render of specific slides):

- Stage 1 validates the partial job.
- Stages 2-6 run only on the affected slides.
- Stage 7 merges with the prior partial render-result.
- Stage 8 sends an updated callback.

The CRUD platform tracks merge state; worker is stateless.

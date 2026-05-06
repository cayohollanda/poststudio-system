# Playbook — Debug Render Failures

> When a render fails, where to look and what to do.

---

## Triage steps

1. **Read the error code.** Per [`failure-and-retry-strategy.md`](../failure-and-retry-strategy.md). Each code has a known cause and fix.
2. **Reproduce locally.** Pull the renderable_carousel from artifacts; render against the worker locally.
3. **Compare to a working baseline.** Use the regression corpus.
4. **Isolate the variable.** Same brand, different topic? Same topic, different brand? Same job, retry?

---

## Per-error-code fixes

### `TEXT_OVERFLOW`

**Cause:** Headline / body overflowed canvas at chosen `font-size`.

**Fix:**
1. Inspect the rendered PNG at the affected slide.
2. Measure rendered_width_px from the error report.
3. Apply via `prompts/16-repair-renderable-output.md`: shorten the headline OR adjust line break OR reduce `font-size` (last resort).
4. Re-render the slide.

**Prevention:** tighten Brand Pack's `voice.md` headline length guidance.

### `FONT_NOT_AVAILABLE`

**Cause:** Font referenced not in worker's font registry.

**Fix:**
1. Check the worker's font registry. Is the font actually missing?
2. If yes: register the font (admin path).
3. If no: check tenant allowance — was the font licensed for this tenant?
4. Re-render after registration / allowance update.

**Prevention:** validate `available_fonts[]` against the registry before submitting.

### `ASSET_NOT_FOUND` / `ASSET_FETCH_FAILED`

**Cause:** Asset placeholder didn't resolve OR asset URL returned 4xx/5xx.

**Fix:**
1. Check asset registry — is the asset_id known?
2. Check the resolved URL — does it return content?
3. If it's a transient CDN issue: retry.
4. If asset is genuinely missing: register it OR remove from the renderable.

**Prevention:** validate `assets_required[]` against the registry before submitting.

### `UNSAFE_HTML_FEATURE`

**Cause:** Sanitizer stripped a load-bearing feature.

**Fix:**
1. Inspect what was stripped (sanitizer logs).
2. If Claude generated something non-essential (e.g. comment, decorative animation): re-render is OK without it.
3. If Claude relied on stripped feature: invoke `prompts/16-repair-renderable-output.md` to substitute a worker-supported equivalent.

**Prevention:** keep `html-css-rules.md` aligned with the worker's actual sanitizer config.

### `INVALID_SVG` / `INVALID_HTML`

**Cause:** Markup didn't parse after sanitization.

**Fix:**
1. Pull the sanitized output from logs.
2. Parse manually — find the syntax error.
3. Invoke `prompts/16-repair-renderable-output.md` with the parser error.
4. Re-render.

**Prevention:** improve Claude-side validation (mental parsing) before returning.

### `TEMPLATE_NOT_FOUND`

**Cause:** Claude picked a template_id not in the brand's library.

**Fix:**
1. Check the request's `available_templates[]` — was it complete?
2. If yes: Claude hallucinated. Re-run with stricter prompting (refer Claude back to the available list).
3. If no: register the template OR add it to the available list.

**Prevention:** ensure the request always includes the canonical `available_templates[]`.

### `SLOT_VIOLATION`

**Cause:** Slot value violated type / length / enum constraint.

**Fix:**
1. Check the violating slot.
2. Truncate / rephrase via `prompts/16-repair-renderable-output.md`.
3. Re-render.

**Prevention:** tighten Claude-side awareness of slot constraints.

### `RENDER_TIMEOUT`

**Cause:** Render exceeded per-slide timeout.

**Fix:**
1. Check what the slide was doing — heavy filters? Large inline images? Complex animations stripped to nothing?
2. If load-shedding caused: retry; should pass.
3. If Claude generated genuinely complex markup: simplify via repair.

**Prevention:** cap inline image sizes; restrict filter complexity.

### `UPLOAD_FAILED`

**Cause:** Asset failed to upload after retries.

**Fix:**
1. Check object storage status.
2. Check IAM / credentials.
3. Retry the slide independently.

**Prevention:** monitor object storage health; use multi-region failover.

---

## When to escalate to human

After 2 repair iterations without success → mark `failed_pending_human_review` and surface in CRUD UI.

A human reviews:

- The renderable JSON.
- The render error report.
- The most recent rendered output (if any).

The human can:

- Manually edit the renderable.
- Provide additional context to Claude.
- Mark "ship anyway" if the issue is cosmetic.
- Reject and request a fresh generation.

---

## Common patterns by brand

After running for a brand for a month:

- Track most-frequent error codes.
- Identify which slides / templates / fonts cause repeat failures.
- Update Brand Pack / templates to remove the patterns.

This is the feedback loop that keeps Mode 3 stable.

---

## Worker logs to check first

When a render fails:

1. Sanitizer log (what was stripped).
2. Asset resolution log (which assets failed).
3. Font injection log (which fonts substituted).
4. Render log (timeout, parser error, OOM).
5. Upload log (which slide upload failed).
6. Callback log (did the result post correctly?).

Most failures show in one of these.

---

## Reproducing locally

```bash
# Pull the renderable JSON from artifacts
curl -O https://crud.example/jobs/<id>/artifacts/renderable.json

# Run the worker locally with debug logging
WORKER_LOG_LEVEL=debug node src/worker.js < renderable.json

# Inspect the local render
open out/slide-<n>.png
```

Most issues reproduce locally on the first try.

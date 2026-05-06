# Playbook — Generate Carousel End-to-End (Mode 2)

> The full Mode 2 pipeline: brief → carousel → quality gate → delivery. No rendering in this playbook.

---

## Pre-flight

- Brand exists in CRUD; Brand Pack version current.
- Tenant API key valid.
- Anthropic API key valid.
- Schemas pinned to current `prompt_version`.

---

## Stage 1 — Receive request

```http
POST /v1/jobs/generation
Content-Type: application/json
Authorization: Bearer <tenant_api_key>
Idempotency-Key: <uuid>

{
  "request_id": "...",
  "tenant_id": "...",
  "brand_id": "lumen",
  "module": "carousel-machine",
  "prompt_version": "v2.0.0",
  "output_format": "json",
  "render": false,
  "brief": { ... }
}
```

Validate against `integration/schemas/generation-request.schema.json`.

If invalid → `400 VALIDATION_ERROR`.
If brand_id missing in DB → `404 BRAND_NOT_FOUND`.

Return `202 Accepted` + `{ "job_id": "...", "status": "queued" }`.

---

## Stage 2 — Assemble prompt

Server-side worker:

1. Fetch Brand Pack from DB, keyed by `tenant_id + brand_id`.
2. Validate it loads. If `null` → mark job `failed`, error `MISSING_BRAND_PACK`.
3. Assemble per [`prompt-assembly-strategy.md`](../prompt-assembly-strategy.md):
   - System core layers.
   - Module layer (carousel-machine).
   - Brand Pack layer.
   - Schema reference (carousel-output.schema.json).
4. Hash the assembled prompt; log it.

---

## Stage 3 — Call Claude API

```python
response = anthropic.messages.create(
    model="claude-opus-4-7",
    system=assembled_system,
    messages=[{ "role": "user", "content": brief }],
    max_tokens=8000,
    metadata={
      "request_id": request.request_id,
      "tenant_id": request.tenant_id,
      "brand_id": request.brand_id,
      "prompt_version": "v2.0.0"
    }
)
```

Mark job `generating` → `generated` on success.

Validate response against `schemas/carousel-output.schema.json`.

If invalid → retry once with stricter system. Then `SCHEMA_VIOLATION` failure.

Store response as `artifacts/carousel.json`. Fire `job.generation_complete` webhook.

---

## Stage 4 — Quality Gate (separate Claude API call)

```python
qg_response = anthropic.messages.create(
    model="claude-opus-4-7",
    system=quality_gate_system,
    messages=[{
      "role": "user",
      "content": "Critique this output: " + json.dumps(generation_response_data)
    }],
    max_tokens=4000
)
```

Different system prompt (the quality-gate module's instructions). Different context.

Validate against `schemas/quality-review.schema.json`.

Store as `artifacts/quality-review.json`. Fire `job.quality_gate_complete`.

If `verdict == "Rewrite required"` → Stage 4b. Else → Stage 5.

### Stage 4b — Improve loop

If iteration < 2:

```python
improve_response = anthropic.messages.create(
    system=carousel_machine_system,  # same as generation
    messages=[{ "role": "user", "content": "Apply this fix to the previous output: " + fix }],
    max_tokens=8000
)
```

Re-validate, re-store. Re-run Quality Gate (Stage 4).

If iteration >= 2 and still failing → mark job `failed_pending_human_review`. Surface in CRUD UI.

---

## Stage 5 — Approve / deliver

Mark job `approved_for_delivery` → `delivered`.

Fire `job.delivered` webhook.

Consumer can fetch artifacts via `GET /jobs/{id}`.

---

## Total latency budget

- Stage 1-2: < 200ms.
- Stage 3 (gen): 8-25s.
- Stage 4 (QG): 5-15s.
- Stage 4b (improve, if needed): +8-25s.
- Stage 5: < 500ms.

Typical end-to-end: 15-50s.

---

## Failure paths

| Stage | Failure | Action |
|---|---|---|
| 1 | Schema invalid | 400 to consumer |
| 2 | Brand Pack missing | mark failed, MISSING_BRAND_PACK |
| 3 | Claude timeout | retry once; then failed |
| 3 | Schema violation | retry once with stricter system; then failed |
| 4 | QG critical fail | improve loop (max 2) |
| 4b | Improve max iterations | failed_pending_human_review |

---

## Audit log entries

Every transition logged with `job_id`, `tenant_id`, `brand_id`, prompt hash, model, duration, cost. See [`observability-and-logging.md`](../observability-and-logging.md).

# Observability and Logging

> What to log, what to alert on, and how to investigate incidents.

---

## What to log

### Per Claude API call

- `tenant_id`, `brand_id`, `job_id`, `request_id`.
- `prompt_version`, `model`, `module`.
- Prompt size (tokens, characters).
- Response size (tokens, characters).
- Latency.
- Cost (USD).
- Result status: `success`, `schema_violation`, `timeout`, `rate_limited`, `auth_failure`.
- Error code + message (on failure).

### Per render job

- `tenant_id`, `brand_id`, `job_id`, `render_job_id`.
- Slide count.
- Render mode (html-css / svg / template).
- Total duration.
- Per-slide duration.
- Worker version.
- Asset upload status.
- Result status: `complete`, `partial_failure`, `failed`.

### Per state transition

- Job state changes with timestamp.
- Webhook deliveries with status code + latency.
- Retry attempts.

### Per user action

- API requests (method, path, status, latency).
- Approvals / revisions / cancellations.
- Brand Pack updates (with diff summary).

---

## What NOT to log

- Brand Pack content verbatim (it's stored in DB; logs reference by hash).
- Customer-identifying details from `proof-assets.md` flagged "do not cite publicly."
- API keys, tokens, signing secrets.
- Full assembled prompts (log a hash; the assembly is reproducible from `prompt_version` + `brand_pack_version`).
- Generated content from `failed_pending_human_review` jobs in plaintext (sensitive).

---

## Logging levels

- `DEBUG` — full payload echoes; only in dev.
- `INFO` — state transitions, successful jobs.
- `WARN` — retries, partial successes, missing inputs surfaced.
- `ERROR` — terminal failures, schema violations.
- `CRITICAL` — cross-tenant contamination, auth bypass, data leak.

---

## Log structure

JSON-line format:

```json
{
  "timestamp": "2026-05-06T12:34:56.789Z",
  "level": "INFO",
  "service": "crud-platform",
  "event": "job.generation_complete",
  "tenant_id": "...",
  "brand_id": "...",
  "job_id": "...",
  "request_id": "...",
  "prompt_version": "v2.0.0",
  "module": "carousel-machine",
  "duration_ms": 12345,
  "tokens_in": 14200,
  "tokens_out": 3100,
  "cost_usd": 0.18
}
```

---

## Metrics

Track over time:

- Jobs per hour (per tenant + brand).
- Success rate by module.
- Quality Gate critical-fail rate.
- Renderability fail rate.
- Mean / p95 / p99 latency by module.
- Cost per job (and by tenant, by brand).
- Retry rate.
- Webhook delivery success rate.

Visualize in your dashboard tool.

---

## Alerts

| Trigger | Severity |
|---|---|
| Cross-tenant prompt detected | CRITICAL |
| Webhook signature verification failure rate > 1% | CRITICAL |
| Auth bypass attempt | CRITICAL |
| Quality Gate critical-fail rate > 10% | WARN |
| Render fail rate > 5% | WARN |
| p95 latency > 60s for carousel module | WARN |
| Cost spike (> 2x 7-day average) | WARN |
| Storage usage > 80% | WARN |
| Webhook delivery failure > 5 retries on a single subscription | WARN |

---

## Tracing

Distributed traces span:

```
Consumer → CRUD platform → Claude API call (gen) → Quality Gate call → Renderable call → Worker → Object storage
```

Correlate with `request_id` and `job_id`.

---

## Investigation playbook

When a job fails unexpectedly:

1. Look up `job_id` → state history + error code.
2. Pull the assembled prompt hash and `prompt_version`.
3. Reproduce the assembly locally with the same Brand Pack version.
4. If the prompt is fine, replay the Claude API call.
5. If the response is invalid, escalate to model behavior (file an issue with Anthropic if reproducible).
6. If the schema is too strict, evaluate whether to relax (rare; major version bump).

---

## Privacy

- Logs scrubbed for PII before long-term retention.
- Logs encrypted at rest.
- Access to production logs gated by RBAC.
- Log queries by support / engineering audited.

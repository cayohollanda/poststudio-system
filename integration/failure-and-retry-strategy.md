# Failure and Retry Strategy

> When to retry, when to fail loud, when to escalate to human.

---

## Failure taxonomy

| Failure | Layer | Strategy |
|---|---|---|
| Invalid JSON from Claude | Generation | Retry once with stricter system. Then `SCHEMA_VIOLATION`. |
| Schema validation failure | Generation | Same. |
| Quality Gate critical fail | Quality | Auto-improve up to 2 loops. Then escalate. |
| Renderability check fail | Renderable | Auto-repair up to 2 loops. Then escalate. |
| Worker render fail (per slide) | Render | Auto-repair via `prompts/16-repair-renderable-output.md`, max 2. Then partial-success. |
| Worker timeout | Render | Retry once. Then mark slide failed. |
| Asset upload failure | Worker | Retry 3× with backoff. Then fail the slide. |
| Brand Pack missing | Generation | Return `MISSING_BRAND_PACK`. No retry. |
| Unsafe claim demanded | Generation | Return `UNSAFE_CLAIM`. No retry. |
| Rate limit (Claude) | Generation | Honor `Retry-After`; queue. |
| Rate limit (worker) | Render | Honor backpressure; queue. |
| Authentication failure | Any | No retry. Surface to ops. |

---

## Retry budgets

| Loop | Max iterations |
|---|---|
| Schema validation retry | 1 |
| Quality Gate improve loop | 2 |
| Renderability repair loop | 2 |
| Worker render retry | 2 |
| Asset upload retry | 3 |
| Webhook delivery retry | 5 (over 24h) |

Beyond budget: structured failure + human review queue.

---

## Backoff

Exponential with jitter:

```
attempt 1: immediate
attempt 2: 2s ± 0.5s
attempt 3: 5s ± 1s
attempt 4: 12s ± 2s
attempt 5: 30s ± 5s
```

For webhooks, use longer backoff (1m, 5m, 30m, 2h, 6h).

---

## Escalation paths

When retries fail:

1. **Generation failure** → mark job `failed` → fire `job.failed` webhook → log for ops.
2. **Quality Gate persistent fail** → mark job `failed_pending_human_review` → surface in CRUD UI for human → no auto-ship.
3. **Render persistent fail** → mark job partially-rendered → surface failed slides in UI → human can manually re-trigger or fix.
4. **Brand Pack issue** → return `MISSING_BRAND_PACK` to consumer → consumer prompts user to update Brand Pack.

---

## Distinct failure modes for renderable mode

| Failure | What it means |
|---|---|
| `SCHEMA_VIOLATION` | Claude returned invalid renderable JSON. Retry generation once. |
| `UNSAFE_RENDERABLE` | Renderable contains JS / remote assets / fake logos. Trigger repair. |
| `TEXT_OVERFLOW` | Headline overflowed canvas at render time. Trigger repair (shorten copy). |
| `FONT_NOT_AVAILABLE` | Font referenced isn't in the worker's library. Repair: substitute. |
| `ASSET_NOT_FOUND` | Asset ID doesn't resolve. Repair: fall back to flat background or surface as missing input. |
| `TEMPLATE_NOT_FOUND` | template_id not registered. Repair: pick closest match or fall back to html-css. |
| `SLOT_VIOLATION` | Slot value violates type/length. Repair: truncate or rephrase. |

See [`modules/renderable-creative-system/repair-rules.md`](../modules/renderable-creative-system/repair-rules.md).

---

## What humans review

- `failed_pending_human_review` queue.
- Persistent claim violations.
- Brand Pack conflicts that auto-resolution didn't handle.
- Render outputs that passed schema but look wrong.

Volume in this queue is a quality signal. Track over time.

---

## Logging on failure

Every failure logs:

- `job_id`, `tenant_id`, `brand_id`.
- Stage where it failed.
- Error code, message, remediation hint.
- Attempt number.
- Final state.
- (Optional) the offending payload (with secrets redacted).

See [`observability-and-logging.md`](observability-and-logging.md).

---

## What never retries

- Authentication failures.
- 4xx client errors (other than 429 rate-limit).
- `UNSAFE_CLAIM` / `MISSING_BRAND_PACK` / `MODULE_NOT_SUPPORTED`.
- Cancelled jobs.

These need human / consumer action, not retry.

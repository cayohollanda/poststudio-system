# Async Job Lifecycle

> The state machine for a generation job.

---

## States

```
   created
      │
      ▼
   queued ─────────────────────────┐
      │                            │
      ▼                            │
   generating                      │
      │                            │
      ├─── failed (schema/timeout) ┘
      │
      ▼
   generated
      │
      ▼
   quality_gate_running
      │
      ├─── quality_gate_failed (critical fails)
      │           │
      │           ▼
      │     improving
      │           │
      │           ├─── improve_failed (max iterations)
      │           │
      │           ▼
      │     quality_gate_running (loop, max 2x)
      │
      ▼
   quality_gate_passed
      │
      ├──[render: false]──▶ approved_for_delivery ──▶ delivered
      │
      └──[render: true]
            │
            ▼
       renderable_generating
            │
            ├─── failed
            │
            ▼
       renderable_generated
            │
            ▼
       render_validating
            │
            ├─── render_repair_needed
            │           │
            │           ▼
            │     renderable_generating (loop, max 2x)
            │
            ▼
       render_dispatched
            │
            ▼
       rendering
            │
            ├─── render_failed
            │           │
            │           ▼
            │     renderable_generating (repair loop, max 2x)
            │
            ▼
       render_complete
            │
            ▼
       approved_for_delivery
            │
            ▼
       delivered
```

---

## State definitions

| State | Meaning |
|---|---|
| `created` | Job exists; inputs validated. |
| `queued` | Awaiting a worker thread. |
| `generating` | Claude API call in flight. |
| `generated` | Generation response received and schema-validated. |
| `quality_gate_running` | Critique API call in flight. |
| `quality_gate_passed` | All critical items pass. |
| `quality_gate_failed` | Critical fail; improvement loop triggered. |
| `improving` | Improve API call in flight. |
| `improve_failed` | Max iterations exceeded; flagged for human review. |
| `renderable_generating` | Renderable conversion API call in flight. |
| `renderable_generated` | Renderable JSON received and schema-validated. |
| `render_validating` | Renderability checks running. |
| `render_repair_needed` | Renderability fail; repair loop triggered. |
| `render_dispatched` | Job sent to worker. |
| `rendering` | Worker is rendering. |
| `render_complete` | Final assets uploaded; URLs available. |
| `render_failed` | Worker failed; repair loop triggered. |
| `approved_for_delivery` | Ready for the consumer app. |
| `delivered` | Consumer notified (webhook or polling). |
| `cancelled` | User cancelled. |
| `failed` | Terminal failure; manual review needed. |

---

## Timeouts

| Transition | Timeout |
|---|---|
| `queued → generating` | 60s |
| `generating → generated` | 90s |
| `quality_gate_running → quality_gate_passed/failed` | 60s |
| `improving → quality_gate_running` | 90s |
| `renderable_generating → renderable_generated` | 120s |
| `render_dispatched → render_complete` | 5 min |
| Total job | 15 min |

Beyond timeout: mark `failed` with reason; surface for retry.

---

## Retry budgets

| Loop | Max iterations |
|---|---|
| Schema validation retry | 1 |
| Quality Gate improve loop | 2 |
| Renderable repair loop | 2 |
| Worker render retry | 2 |

Beyond budget: mark `failed_pending_human_review` and surface in CRUD UI.

---

## Idempotency

- Each job has a unique `job_id`.
- Re-creating a job with the same `job_id` returns the existing job's state.
- The CRUD platform may issue `request_id` separately for retries within a job.

---

## State transitions and webhooks

The CRUD platform fires webhooks on these state changes:

- `created` → `job.created`
- `generated` → `job.generation_complete`
- `quality_gate_failed` → `job.quality_gate_failed`
- `improving` → (no webhook; internal)
- `renderable_generated` → `job.renderable_complete`
- `render_dispatched` → `job.render_started`
- `render_complete` → `job.render_complete`
- `render_failed` → `job.render_failed`
- `approved_for_delivery` → `job.approved`
- `delivered` → `job.delivered`
- `failed` → `job.failed`

See [`webhook-contract.md`](webhook-contract.md).

---

## Cancellation

A job can be cancelled while in:
- `queued`, `generating`, `quality_gate_running`, `improving`, `renderable_generating`, `render_validating`, `render_dispatched`, `rendering`.

Cancellation:
- Marks the job `cancelled`.
- Does NOT roll back work already done (the artifacts produced are saved).
- Fires `job.cancelled` webhook.

---

## Cleanup

- Completed jobs older than retention period (e.g. 90 days) → archive.
- Failed jobs → keep audit log forever; archive artifacts after 30 days.
- Cancelled jobs → archive after 7 days.

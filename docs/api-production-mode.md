# API Production Mode (Mode 2)

> Using PostStudio System inside an external CRUD platform that calls Claude API. The same repo files serve as versioned context.

---

## When to use Mode 2

- You're building a SaaS or internal platform where users manage brands and generate content.
- You need to scale beyond what a single Claude Project can handle.
- You need audit logs, multi-tenant isolation, and structured webhook events.
- You want JSON output validated against schemas, not free-form Markdown.

If you're a single human in a Claude Project, you're in Mode 1 — skip this doc.

---

## What changes in Mode 2

| Aspect | Mode 1 | Mode 2 |
|---|---|---|
| Runtime | Claude Project (manual) | Claude API (programmatic) |
| Output | Markdown (default) | JSON (default) |
| Clarifying questions | Up to 3 in chat | Never; surface as `missing_inputs[]` |
| Quality Gate | Same chat (loose discipline) | Separate API call (mandatory) |
| Storage | User decides (Notion, files) | CRUD platform DB + object store |
| Versioning | Per upload | `prompt_version` per job |
| Multi-tenant | One brand per Project | N brands per tenant, per CRUD |

---

## The minimum viable Mode 2 setup

1. **Pin the repo version.** Tag like `v2.0.0` and reference that tag in `prompt_version` per job.
2. **Implement 5 API operations.** See [`integration/external-crud-platform-contract.md`](../integration/external-crud-platform-contract.md):
   - `POST /jobs/generation`
   - `GET /jobs/{id}`
   - `POST /jobs/{id}/quality-gate`
   - `POST /jobs/{id}/render` (Mode 3 only)
   - `POST /jobs/{id}/approve`
3. **Validate every request and response** against the schemas in `integration/schemas/` and `schemas/`.
4. **Assemble prompts** per [`integration/prompt-assembly-strategy.md`](../integration/prompt-assembly-strategy.md): system layer + module layer + Brand Pack layer + schema reference + brief.
5. **Run Quality Gate as a separate Claude API call** per [`integration/playbooks/generate-carousel-end-to-end.md`](../integration/playbooks/generate-carousel-end-to-end.md).
6. **Store everything keyed by `tenant_id + brand_id + job_id`.**
7. **Emit webhooks** per [`integration/webhook-contract.md`](../integration/webhook-contract.md).

---

## Prompt assembly per request

```
system: [
  Layer 1 — system core (master-instructions, operating-system, safety, output-rules, quality-checklist)
  Layer 2 — module instructions (e.g. modules/carousel-machine/instructions.md)
  Layer 3 — Brand Pack (brands/<slug>/* concatenated)
  Layer 4 — schema reference (the output schema for the requested module)
]
messages: [
  { role: "user", content: <the brief> }
]
```

Cache layers 1, 2, 4 per repo version. Cache layer 3 per brand. Only the brief changes per call.

See [`integration/context-loading-strategy.md`](../integration/context-loading-strategy.md) and [`integration/prompt-assembly-strategy.md`](../integration/prompt-assembly-strategy.md).

---

## Output discipline

Mode 2 outputs are **JSON only**. No surrounding text. No Markdown fences. No "Here's your carousel."

If Claude can't conform to the schema, return a structured error:

```json
{
  "version": "1.0",
  "prompt_version": "v2.0.0",
  "request_id": "...",
  "errors": [
    {
      "code": "MISSING_BRAND_PACK",
      "message": "Brand 'foo' is referenced but no Brand Pack was provided.",
      "field": "brand_id",
      "remediation": "Provide brand pack or run prompts/02-generate-brand-pack.md."
    }
  ]
}
```

See [`system/api-output-rules.md`](../system/api-output-rules.md).

---

## Quality Gate as a separate call

After every generation:

1. Store the generation response.
2. Make a fresh Claude API call with `prompts/08-critique-output.md` + the generation as input.
3. Different system prompt: the quality-gate module's instructions.
4. Receive `quality-review` JSON.
5. Decide: ship, improve (max 2 loops), or escalate.

Never run generation and critique in the same context. The model defends its own draft.

---

## Multi-tenant boundaries

- Each brand is a separate folder + DB key.
- Brand Pack data **never crosses tenants**.
- Per-tenant API keys. Per-brand API keys optional.
- Logs partitioned by `brand_id`.
- Cache keyed by tenant + brand.

See [`integration/multi-tenant-boundaries.md`](../integration/multi-tenant-boundaries.md).

---

## Versioning

Every job records `prompt_version` and `brand_pack_version`. This lets you:

- Reproduce historical generations.
- Detect breaking changes.
- A/B test prompt versions.
- Roll back if a release degrades quality.

Pin in production. Update via release cycle, not silently.

---

## Failure handling

- Schema violations: retry once with stricter system. Then `SCHEMA_VIOLATION` failure.
- Quality Gate critical fails: improve loop max 2.
- Rate limits: honor `Retry-After`; queue.
- Worker failures (Mode 3): repair via `prompts/16-repair-renderable-output.md` max 2; then `failed_pending_human_review`.

See [`integration/failure-and-retry-strategy.md`](../integration/failure-and-retry-strategy.md).

---

## Cost discipline

- Cache stable prompt layers (system + module + schema) per repo version.
- Cache Brand Pack per brand.
- Use Anthropic's prompt caching where supported.
- Only the brief changes per call → high cache hit rate.
- Track cost per job; alert on anomalies.

---

## Common Mode 2 patterns

### Generate one carousel
```
POST /jobs/generation { module: "carousel-machine", brief: {...} }
→ webhook: job.generation_complete
→ POST /jobs/{id}/quality-gate
→ webhook: job.quality_gate_complete (verdict)
→ POST /jobs/{id}/approve
→ webhook: job.delivered
```

### Onboard a new brand
```
POST /brands { slug: "newco", name: "..." }
POST /jobs/generation { module: "brand-pack-builder", brief: {pasted_context: ...} }
→ store the 12 sections
POST /jobs/generation { module: "brand-diagnosis-agent", brief: {} }
→ display diagnosis
POST /jobs/generation { module: "content-strategy-os", brief: { time_window_days: 30 } }
→ display strategy
```

### Run a 14-day campaign
```
POST /jobs/generation { module: "campaign-builder", brief: { duration_days: 14, ... } }
→ for each day's post in the campaign:
POST /jobs/generation { module: "carousel-machine" | "reels-script-machine", brief: {...} }
→ run quality gate per post
→ store as a connected campaign artifact
```

---

## What Mode 2 doesn't change

- The prompts themselves. Same `prompts/00-17.md` files.
- The schemas. Same `schemas/*.schema.json`.
- The Brand Pack shape. Same 12 files.
- The Quality Gate rubric. Same 30-point + 10-point checklists.

The runtime is different. The system is the same.

---

## What never works in Mode 2

- Claude asking clarifying questions in chat. There is no chat. Surface as `missing_inputs[]`.
- Reading a file Claude doesn't have. The CRUD platform must include the file in the prompt.
- Cross-brand context. Strict per-brand assembly.
- Skipping the schema validation. The system trusts the schema, not the LLM.

---

## Next reading

- [`integration/README.md`](../integration/README.md)
- [`integration/external-crud-platform-contract.md`](../integration/external-crud-platform-contract.md)
- [`integration/playbooks/generate-carousel-end-to-end.md`](../integration/playbooks/generate-carousel-end-to-end.md)
- [`system/api-output-rules.md`](../system/api-output-rules.md)

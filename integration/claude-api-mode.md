# Claude API Mode

> How to call Claude API using PostStudio System repository files as the context source — without depending on Claude Project as the runtime.

---

## The principle

Production should not depend on Claude Project. Claude Project is a *manual* tool. Production needs a deterministic API runtime backed by versioned files.

Both modes use the same repository. The runtime difference is who assembles the prompt: Claude Project (auto, by knowledge upload) vs the CRUD platform (programmatic, by file ingestion).

---

## Setup checklist

- [ ] PostStudio System repo cloned to a known location (or pulled into your build pipeline).
- [ ] Pin to a specific tag (e.g. `v2.0.0`).
- [ ] Brand Packs stored in your CRUD database, keyed by `brand_slug`.
- [ ] Schemas validated server-side.
- [ ] Quality Gate runs as a separate API call.
- [ ] Renderable outputs validated against the renderable schema before render dispatch.

---

## Required Claude API request shape

```json
{
  "model": "claude-opus-4-7",
  "system": "<assembled system prompt>",
  "messages": [
    { "role": "user", "content": "<the brief>" }
  ],
  "max_tokens": 8000,
  "metadata": {
    "request_id": "<uuid>",
    "brand_id": "<id>",
    "prompt_version": "v2.0.0",
    "module": "carousel-machine"
  }
}
```

The `system` prompt is assembled per [`prompt-assembly-strategy.md`](prompt-assembly-strategy.md).

---

## Per-module API calls

| Module | Prompt | Schema |
|---|---|---|
| Carousel Machine | `prompts/04-generate-carousel.md` | `schemas/carousel-output.schema.json` |
| Brand Diagnosis | `prompts/01-run-brand-diagnosis.md` | `schemas/brand-diagnosis.schema.json` |
| Brand Pack Builder | `prompts/02-generate-brand-pack.md` | `schemas/brand-pack.schema.json` |
| Content Strategy | `prompts/03-generate-content-strategy.md` | `schemas/content-strategy.schema.json` |
| Visual Prompts | `prompts/05-generate-image-prompts.md` | n/a (typically embedded in carousel) |
| Reels Script | `prompts/06-generate-reels-script.md` | `schemas/reels-script.schema.json` |
| Campaign | `prompts/07-generate-campaign.md` | `schemas/campaign.schema.json` |
| Critique | `prompts/08-critique-output.md` | `schemas/quality-review.schema.json` |
| Improve | `prompts/09-improve-output.md` | depends on input |
| Export JSON | `prompts/10-export-json.md` | depends on input |
| Renderable Carousel | `prompts/13-generate-renderable-carousel.md` | `schemas/renderable-carousel.schema.json` |
| Renderable Repair | `prompts/16-repair-renderable-output.md` | `schemas/renderable-carousel.schema.json` |

---

## Output discipline (enforced server-side)

- Reject any response that isn't valid JSON.
- Reject any response missing `prompt_version`, `request_id`.
- Reject any response that doesn't conform to the requested module's schema.
- On rejection: retry once with a stricter system prompt, then fail with `SCHEMA_VIOLATION`.

---

## Quality Gate as separate call

The Quality Gate is **a separate API call**, not the same context as generation. The CRUD platform:

1. Receives a generation response.
2. Stores it.
3. Makes a fresh Claude API call with `prompts/08-critique-output.md` + the generation as input.
4. Stores the critique alongside the generation.
5. Decides next step: ship, improve, or escalate.

Never run generation and critique in the same context. The model defends its own draft.

---

## Renderable mode

When `render: true` in the request:

1. Generate the carousel content first (Carousel Machine).
2. Make a separate API call with `prompts/13-generate-renderable-carousel.md` to convert it to renderable JSON.
3. Run renderability checks (Quality Gate's renderability pass).
4. Dispatch to the worker only if checks pass.

See [`async-job-lifecycle.md`](async-job-lifecycle.md) for the state machine.

---

## What the CRUD platform must NOT do

- Modify Claude's response before storing it (audit trail).
- Strip `[PROOF_NEEDED]` markers (they're how missing inputs surface).
- Override Brand Pack constraints based on user input.
- Mix two brands' Brand Packs in one prompt.
- Run prompts the schema doesn't define.
- Let the consumer app call Claude directly bypassing the CRUD platform.

---

## Cost discipline

- Cache the assembled stable prompt layers (system + module + schema) per brand.
- Only the `messages[]` (brief) changes per request.
- Use prompt caching where the API supports it.
- Track cost per job in metadata.

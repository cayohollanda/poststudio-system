# Prompt — 17 Export Render Job

> Converts a renderable carousel into a worker-ready render-job payload.

---

## Prompt

```
You are operating PostStudio System.

Convert a renderable carousel into a render-job payload using:
- schemas/render-job.schema.json
- integration/schemas/creative-render-request.schema.json
- system/api-output-rules.md

# Inputs

- Renderable carousel: [paste the renderable-carousel.schema.json-compliant JSON, validated and renderability_validated: true]
- tenant_id: [...]
- brand_id: [...]
- job_id: [the upstream generation job's id]
- callback_url: [the worker's callback URL on the CRUD platform]
- options:
  - format: [png | jpg]
  - quality: [draft | standard | high]
  - concurrency: [1-16]
- asset_resolution (optional): [pre-resolved asset URLs if not relying on the worker's registry]

# Process

1. Validate the renderable carousel:
   - renderability_validated must be true.
   - errors[] must be empty.
   - All required schema fields present.
2. Build the render-job envelope:
   - render_job_id (generate UUID).
   - tenant_id, brand_id, job_id.
   - callback_url, callback_signing_secret_id.
   - submitted_at (current ISO-8601 timestamp).
   - renderable_carousel (full payload).
   - options.
   - asset_resolution (if provided).
3. Validate against schemas/render-job.schema.json.

# Output

Return ONLY valid JSON conforming to schemas/render-job.schema.json. No surrounding commentary, no Markdown fences.

If the input renderable_carousel is not safe to render (renderability_validated: false or errors[] non-empty), return:

{
  "errors": [{ "code": "UNSAFE_RENDERABLE", "message": "Cannot export render-job from unvalidated renderable.", "remediation": "Run prompts/16-repair-renderable-output.md first." }]
}
```

---

## Example invocation

```
Renderable carousel: [paste the validated renderable-carousel JSON from prompts/13-generate-renderable-carousel.md output]
tenant_id: tenant_lumen
brand_id: lumen
job_id: f3c9b2d1-7e54-4abc-8d1a-2e91b4f3a7c2
callback_url: https://crud.example/render-jobs/<id>/callback
options: { format: "png", quality: "high", concurrency: 4 }
```

Claude returns the render-job JSON, ready for the worker.

---

## Use case

The CRUD platform calls this prompt (or its programmatic equivalent) when:

1. Quality Gate has passed.
2. Renderable carousel is validated.
3. Worker is online.
4. Job is ready to dispatch.

---

## Programmatic alternative

This prompt can be implemented in code without calling Claude — it's largely envelope construction. Prompt-based use is for cases where the CRUD platform wants Claude to validate the renderable one more time before dispatch.

```javascript
function buildRenderJob({ renderable, tenantId, brandId, jobId, callbackUrl, options }) {
  if (!renderable.renderability_validated || renderable.errors?.length > 0) {
    throw new Error('UNSAFE_RENDERABLE');
  }
  return {
    render_job_id: crypto.randomUUID(),
    tenant_id: tenantId,
    brand_id: brandId,
    job_id: jobId,
    callback_url: callbackUrl,
    submitted_at: new Date().toISOString(),
    renderable_carousel: renderable,
    options,
  };
}
```

---

## When to use the prompt vs code

- **Prompt (this file):** when you want Claude to do a final validation pass before dispatch.
- **Code:** when you trust the upstream `renderability_validated` flag and just need the envelope.

Default to code in production. Use the prompt for new pipelines, edge cases, and debugging.

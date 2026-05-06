# Playbook — Run Quality Gate Before Render

> Mode 3 requires Quality Gate to run *before* a render job is dispatched. This playbook describes the sequence.

---

## Why

Rendering is expensive and produces a final asset the user sees. If the content fails Quality Gate, rendering it embeds bad content into PNGs that survive in the brand's archive. Always gate before render.

---

## Sequence

```
1. Generate carousel (content)
2. Run Content Quality Gate
3. If pass: continue. If fail: improve loop, then re-gate.
4. Generate renderable carousel
5. Run Renderability Quality Gate
6. If pass: dispatch render. If fail: repair loop, then re-gate.
7. Dispatch render job
```

---

## Stage 1 — Generate carousel content

Per [`generate-carousel-end-to-end.md`](generate-carousel-end-to-end.md) Stage 1-3.

---

## Stage 2 — Content Quality Gate

`prompts/08-critique-output.md` runs in a separate Claude API call.

Inputs:
- The carousel JSON.
- The active Brand Pack.
- The original brief.

Outputs: `quality-review` JSON.

If `verdict == "Rewrite required"` → trigger improve loop (max 2). If still failing, mark `failed_pending_human_review`.

If pass → continue.

---

## Stage 3 — Generate renderable carousel

Separate Claude API call:

```
prompts/13-generate-renderable-carousel.md
+ system/renderable-creative-framework.md
+ modules/renderable-creative-system/<sub-mode>-rules.md
+ Brand Pack
+ schemas/renderable-carousel.schema.json
+ the approved carousel JSON
+ canvas dimensions, available_fonts, available_assets
```

Returns `renderable-carousel.schema.json`-compliant JSON.

Validate against schema. If invalid → retry once. Then `SCHEMA_VIOLATION`.

Store as `artifacts/renderable.json`. Fire `job.renderable_complete`.

---

## Stage 4 — Renderability Quality Gate

Separate Claude API call:

```
prompts/08-critique-output.md (with renderable input)
+ modules/quality-gate/renderability-checks.md
+ schemas/quality-review.schema.json
+ the renderable JSON
```

Returns `quality-review` with renderability section populated.

Verdict logic:
- All renderability critical (R1-R7) pass → **dispatch render**.
- Any R1-R7 fail → invoke `prompts/16-repair-renderable-output.md` (max 2 iterations).
- Only R8-R10 fail → dispatch with warnings.

If repair loop exceeds 2 iterations → `failed_pending_human_review`.

---

## Stage 5 — Dispatch render job

`POST /render-jobs` to the worker per [`partner-creative-api.md`](../partner-creative-api.md).

Mark job `render_dispatched` → `rendering` once worker accepts.

---

## Stage 6 — Receive render result

Worker callback or polling:

- `complete` → mark `render_complete`, store assets, fire `job.render_complete`.
- `partial_failure` → invoke render-side repair via `prompts/16-repair-renderable-output.md`, retry max 2.
- `failed` → mark `render_failed`, fire `job.render_failed`.

---

## Stage 7 — Approve / deliver

Mark `approved_for_delivery` → `delivered`. Fire `job.delivered`.

---

## End-to-end latency

- Stage 1-2: 15-50s.
- Stage 3: 15-45s.
- Stage 4: 5-15s.
- Stage 5-6: 30-180s (worker time).
- Stage 7: < 500ms.

Typical Mode 3 job: 60-180s end-to-end.

---

## Why two separate Quality Gate passes

- **Content QG** asks "is the *content* good?" — rubric-driven critique of the carousel itself.
- **Renderability QG** asks "is the *renderable form* safe?" — schema, asset, JS, font, overflow checks.

Both can pass independently. A renderable that fails QG isn't safe to render even if the content is fine. A content carousel that fails QG isn't worth rendering even if the renderable JSON is valid.

Run both. In order. Don't skip.

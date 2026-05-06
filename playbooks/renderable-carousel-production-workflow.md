# Playbook — Renderable Carousel Production Workflow

> Mode 3. Mode 2 plus a rendering worker producing finished PNG/JPG slides.

This is the most automated path. Brief in → finished image assets out.

---

## Pipeline

```
[Brief]
  → Generate carousel content (Claude API)
  → Quality Gate (content) — separate Claude API call
  → Generate renderable carousel (Claude API)
  → Quality Gate (renderability) — separate Claude API call
  → Submit render-job to worker
  → Worker renders + uploads
  → Receive asset URLs
  → Deliver
```

Steps 1-4 are Mode 2. Steps 5-7 add Mode 3.

---

## Per-stage detail

### 1. Generate carousel content

Standard `carousel-machine` job per `prompts/04-generate-carousel.md`. Output: `carousel-output.schema.json`.

### 2. Content Quality Gate

Separate Claude API call with `prompts/08-critique-output.md`. If `Rewrite required` → improve loop. Standard Mode 2 behavior.

### 3. Generate renderable carousel

After content QG passes, the CRUD platform makes another Claude API call with `prompts/13-generate-renderable-carousel.md`:

- Input: the approved content carousel + canvas + available_fonts + available_assets + render_mode.
- Output: `renderable-carousel.schema.json`-compliant JSON.
- Sub-modes: `html-css` | `svg` | `template`.

### 4. Renderability Quality Gate

Separate Claude API call. Specifically scopes to renderability checks (the 10 R-items):

- Schema valid.
- Canvas correct.
- No remote assets.
- No JavaScript.
- No invented logos.
- Fallback fonts present.
- Single accent applied.
- No headline overflow.
- No banned CSS / SVG features.
- Metadata complete.

If any critical fail (R1-R7) → invoke `prompts/16-repair-renderable-output.md`. Max 2 repair iterations.

### 5. Submit render-job

CRUD platform builds a `render-job` per `schemas/render-job.schema.json` and POSTs to the worker per `integration/partner-creative-api.md`.

### 6. Worker renders

Stateless. Validates → sanitizes → resolves assets/fonts → renders → optimizes → uploads → returns `render-result`.

Per-slide concurrent rendering (typically 4 in flight). Per-slide failures are isolated; the worker returns `partial_failure` if some slides rendered and some didn't.

### 7. Receive results

CRUD validates `render-result`. Stores asset URLs. Fires `job.render_complete` webhook.

---

## Failure paths

| Stage | Failure | Action |
|---|---|---|
| 1 (gen) | Schema violation | retry once → fail |
| 2 (content QG) | Critical fails | improve loop max 2 → fail |
| 3 (renderable) | Schema violation | retry once → fail |
| 4 (render QG) | R1-R7 fail | repair loop max 2 → fail |
| 5 (submit) | Worker rejected | check worker health |
| 6 (render) | Per-slide fail | repair affected slides max 2 |
| 7 (results) | Asset upload fail | worker retries 3× → mark slide failed |

Beyond budgets → `failed_pending_human_review`.

---

## Latency budget (typical)

| Stage | Latency |
|---|---|
| 1. Generate content | 8-25s |
| 2. Content QG | 5-15s |
| 3. Generate renderable | 15-45s |
| 4. Renderable QG | 5-15s |
| 5. Submit render | < 200ms |
| 6. Render (per slide × 4 concurrent) | 4-12s |
| 7. Upload + callback | 1-3s |
| **Total (8 slides)** | **~60-180s** |

For hero slides at high quality: add 20-50%.

---

## Brand setup checklist (before first Mode 3 job)

- [ ] Brand Pack `visual-style.md` complete with palette + typography + composition rules.
- [ ] Brand fonts registered in worker font registry.
- [ ] Brand assets registered in worker asset registry.
- [ ] (Template mode only) brand templates registered in worker template library.
- [ ] Test render of a "hello world" slide passes.
- [ ] Asset CDN domain whitelisted in worker.
- [ ] Webhook subscription for `job.render_complete` set up.

See [`integration/playbooks/onboard-new-brand-production.md`](../integration/playbooks/onboard-new-brand-production.md) Day 3.

---

## Sub-mode decision

| When | Use |
|---|---|
| MVP, first month | freeform HTML/CSS |
| Production at scale | template-driven |
| Vector-first / editorial | SVG |
| Bespoke creative | freeform HTML/CSS |

See [`rendering/template-vs-freeform-rendering.md`](../rendering/template-vs-freeform-rendering.md).

---

## Quality assurance after render

Per `rendering/quality-assurance.md`:

- Per-slide post-render checks (dimensions, file size, edge sampling).
- Cross-slide checks (consistent margins, brand mark, slide numbers).
- pHash drift detection vs brand reference (optional, for high-stakes brands).
- Human review for new brands' first 5 carousels.

---

## Operational signals

Track over time, per brand:

- Render success rate.
- Repair-loop trigger rate.
- Mean per-slide render time.
- Asset substitution rate.
- Font fallback rate.

Drift in any signal indicates upstream problems (Brand Pack changes, prompt drift, registry gaps).

---

## When to add a new template (template mode)

- Recurring layout pattern (3+ uses).
- New brand with distinct visual rules.
- Regulatory requirement.
- New content type (e.g. data drop) needs dedicated layout.

Don't add for one-off needs. Use freeform.

---

## Common Mode 3 hiccups

- **Headline overflow on render** — text was longer than the canvas at the chosen font-size. Repair loop shortens.
- **Font fallback used** — primary font not registered. Surface and register.
- **Asset 404** — placeholder didn't resolve. Update registry or strip from renderable.
- **Sanitizer stripped a feature** — usually `position: sticky`, `<script>`, or remote URL. Repair via worker-supported equivalent.
- **Template not found** — Claude picked a template_id not in the brand's library. Restrict via `available_templates[]` in the request.

---

## On-call

When render fails:

1. Pull the renderable_carousel from `outputs/[brand]/[job]/renderable.json`.
2. Pull the worker error report.
3. Reproduce locally per `rendering/playbooks/debug-render-failures.md`.
4. Most issues reproduce on the first try.
5. If asset / font registry gap: register and re-run.
6. If renderable was malformed: invoke repair loop or regenerate.

---

## Why Mode 3 is worth the complexity

- Same-day shipping from prompt to PNG.
- Per-brand visual consistency without manual layout.
- Multi-tenant scale.
- End-to-end audit trail.
- Webhook-driven downstream automation.

The trade-off is upfront infrastructure cost (worker, registries, sanitization). Once running, it amortizes across many jobs.

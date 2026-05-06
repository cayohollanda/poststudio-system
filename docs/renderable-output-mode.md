# Renderable Output Mode (Mode 3)

> Adding a rendering worker to Mode 2 so Claude's output becomes finished PNG/JPG slides.

---

## When to use Mode 3

- You want **finished image assets**, not specs for a designer.
- You're running an automated content pipeline that ships to social schedulers / CMSs.
- You're scaling a multi-brand engine where consistency at the visual level matters.
- You're shipping renderable carousels at MVP stage.

If you're a single human in Mode 1 designing slides in Figma, you don't need Mode 3.

---

## Mode 3 = Mode 2 + worker

Mode 3 is Mode 2 with one additional layer: a stateless rendering worker.

```
[Mode 2 pipeline runs end-to-end with render: true]
  → Claude API returns renderable-carousel JSON
  → CRUD validates renderable schema
  → CRUD posts a render-job to the worker
  → Worker validates + sanitizes + renders + optimizes + uploads
  → Worker returns render-result JSON (PNG URLs)
  → CRUD stores assets, fires webhook
```

---

## The three sub-modes

### Freeform HTML/CSS — best for MVP

Claude writes a complete HTML/CSS document per slide. The worker renders it via headless Chromium / Playwright.

**Pros:** maximum flexibility, fast to start.
**Cons:** less deterministic, higher sanitization burden.

**Use when:** new brand, MVP, creative exploration.

### SVG — best for vector / editorial

Claude writes a valid `<svg>` element per slide. The worker renders via `resvg` or browser.

**Pros:** vector-first, sharp at any size.
**Cons:** no automatic text wrapping, renderer support varies.

**Use when:** brands with strong typographic identity.

### Template-driven JSON — best for production

Claude picks a `template_id` from the worker's library and fills slot data. The worker renders the template.

**Pros:** deterministic, brand-consistent, safe.
**Cons:** less per-slide flexibility, requires template library.

**Use when:** steady-state production, multi-brand pipelines.

See [`rendering/template-vs-freeform-rendering.md`](../rendering/template-vs-freeform-rendering.md).

---

## Universal rules (all sub-modes)

1. **Canvas:** default 1080×1350 (configurable).
2. **No JavaScript** anywhere.
3. **No remote URLs** — only `data:` URIs and `{{asset:<id>}}` placeholders.
4. **No copyrighted logos / fake brand assets.**
5. **Deterministic layout** — same input, same output.
6. **Editable text** — real text nodes, not rasterized.
7. **Fallback fonts** in every text element.
8. **One accent color per slide,** applied selectively.
9. **Safe margins** (default 48px on each side).
10. **Slide metadata** preserved alongside renderable code.

See [`system/renderable-creative-framework.md`](../system/renderable-creative-framework.md).

---

## The worker's responsibilities

```
1. Validate renderable_carousel against schemas/renderable-carousel.schema.json
2. Sanitize HTML/CSS/SVG per rendering/safety-and-sanitization.md
3. Resolve {{asset:<id>}} placeholders to real URLs
4. Inject approved fonts
5. Render each slide at canvas dimensions (default 1080×1350)
6. Optimize PNGs (sharp / equivalent)
7. Upload to object storage
8. Return render-result with asset URLs
9. On failure: return structured errors per slide
```

The worker does **not**:
- Generate content.
- Choose visual mode or accent color.
- Modify slot values (template mode).
- Cache renders across jobs.

---

## Setting up an MVP worker

1. Stand up a worker process: Node.js + Playwright + sharp + S3 (or equivalent).
2. Implement the [`integration/partner-creative-api.md`](../integration/partner-creative-api.md) endpoint.
3. Implement sanitization per [`rendering/safety-and-sanitization.md`](../rendering/safety-and-sanitization.md).
4. Run the [MVP playbook](../rendering/playbooks/mvp-html-to-png-workflow.md).

For production, layer in template-driven sub-mode per [`rendering/playbooks/template-driven-rendering-workflow.md`](../rendering/playbooks/template-driven-rendering-workflow.md).

---

## Quality Gate runs *before* rendering

Always:

1. Generate the carousel content.
2. Run Content Quality Gate. If fail → improve loop.
3. Generate renderable carousel.
4. Run Renderability Quality Gate.
5. If pass → dispatch render. If fail → repair loop.
6. Worker renders.
7. (Optional) post-render QA per `rendering/quality-assurance.md`.

See [`integration/playbooks/run-quality-gate-before-render.md`](../integration/playbooks/run-quality-gate-before-render.md).

---

## Repair when rendering fails

The worker returns structured errors per slide. The CRUD platform invokes `prompts/16-repair-renderable-output.md` with the error report. Claude returns a repaired renderable. Re-render. Max 2 iterations.

If 2 iterations fail → `failed_pending_human_review`.

See [`modules/renderable-creative-system/repair-rules.md`](../modules/renderable-creative-system/repair-rules.md).

---

## Per-platform canvas

Default: 1080×1350 (4:5).

Other platforms:

- Instagram square: 1080×1080
- TikTok: 1080×1440
- X: 16:9 or 1:1
- LinkedIn carousel doc: 1200×1500 (higher resolution if upgrading)

The request specifies `canvas`. Worker honors it; ignores the renderable's internal dimensions if they differ.

See [`rendering/slide-dimensions.md`](../rendering/slide-dimensions.md).

---

## Multi-target rendering

The worker can render the same renderable carousel at multiple aspect ratios (e.g. one job rendered for both LinkedIn 4:5 and Instagram 1:1). See `rendering/slide-dimensions.md`'s multi-canvas section.

This requires Claude to generate the renderable with `width`/`height` derived from the request's canvas, not hardcoded.

---

## Brand asset registry

The worker maintains a registry per brand:

```
assets/lumen/
  fractured-glass-dome-01.png
  ...
```

Asset placeholders `{{asset:<id>}}` resolve from the registry. Cross-brand asset access is forbidden.

See [`rendering/font-and-asset-handling.md`](../rendering/font-and-asset-handling.md).

---

## Brand template library (template mode)

The worker maintains a template library per brand:

```
templates/lumen/
  cinematic-dark.hook.v1/
  cinematic-dark.problem.v1/
  ...
```

The CRUD platform passes `available_templates[]` in every renderable generation request so Claude knows which to use.

See [`rendering/playbooks/template-driven-rendering-workflow.md`](../rendering/playbooks/template-driven-rendering-workflow.md).

---

## What Mode 3 unlocks

- Same-day shipping from prompt to PNG.
- Per-brand consistency at the visual level.
- Multi-tenant scale.
- Audit trail from brief to asset.
- Webhook-driven downstream automation (CMS, scheduler, ad platform).

---

## What Mode 3 doesn't replace

- Human-in-the-loop review (still recommended for new brands' first 5 carousels).
- Layout taste (the worker renders what Claude wrote; if the visual mode is wrong, the output is wrong).
- The Brand Pack (renderable output is only as good as the visual-style.md it derives from).

---

## Next reading

- [`rendering/README.md`](../rendering/README.md)
- [`rendering/architecture.md`](../rendering/architecture.md)
- [`rendering/playbooks/mvp-html-to-png-workflow.md`](../rendering/playbooks/mvp-html-to-png-workflow.md)
- [`integration/partner-creative-api.md`](../integration/partner-creative-api.md)
- [`system/renderable-creative-framework.md`](../system/renderable-creative-framework.md)

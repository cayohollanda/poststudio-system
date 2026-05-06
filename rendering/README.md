# Rendering Layer

> The technical layer that converts Claude-generated HTML/CSS/SVG/template-driven JSON into final PNG/JPG carousel slides.

This is the implementation guide for the **rendering worker** — the third role in the production architecture (after the CRUD platform and Claude API).

---

## When to read this

- You're building a rendering worker.
- You're operating Mode 3 in production.
- You're debugging render failures.

If you're using Mode 1 (manual layout in Figma) or Mode 2 without rendering — you don't need this layer.

---

## What's in this folder

```
rendering/
├── README.md                              # this file
├── architecture.md                        # high-level architecture
├── renderable-output-contract.md          # the contract Claude conforms to
├── html-css-generation-rules.md           # rules Claude follows for HTML/CSS
├── svg-generation-rules.md                # rules Claude follows for SVG
├── safety-and-sanitization.md             # the worker's safety pipeline
├── playwright-renderer.md                 # HTML/CSS → PNG via headless Chromium
├── sharp-optimization.md                  # PNG/JPG optimization
├── template-vs-freeform-rendering.md      # decision guide
├── font-and-asset-handling.md             # font registry + asset registry
├── slide-dimensions.md                    # canvas + safe margins
├── failure-and-retry-strategy.md          # render-side failure handling
├── quality-assurance.md                   # post-render QA
├── schemas/                               # JSON schemas for the rendering layer
│   ├── renderable-carousel.schema.json    # (mirrors /schemas/ root)
│   ├── renderable-slide.schema.json
│   ├── render-job.schema.json
│   └── render-result.schema.json
├── prompts/                               # Claude prompts for renderable generation + repair
│   ├── generate-renderable-carousel.md
│   ├── generate-html-css-slide.md
│   ├── generate-svg-slide.md
│   ├── repair-broken-render-output.md
│   └── critique-rendered-slide.md
├── examples/                              # canonical artifacts
│   ├── renderable-carousel.json
│   ├── html-css-slide.html
│   ├── svg-slide.svg
│   ├── render-job.json
│   └── render-result.json
└── playbooks/                             # operational runbooks
    ├── end-to-end-rendering-workflow.md
    ├── mvp-html-to-png-workflow.md
    ├── svg-to-png-workflow.md
    ├── template-driven-rendering-workflow.md
    └── debug-render-failures.md
```

---

## The three sub-modes

| Sub-mode | Best for | Trade-off |
|---|---|---|
| **Freeform HTML/CSS** | MVP, creative exploration | Less deterministic; needs strong sanitization |
| **SVG** | Vector-first, editorial typography | Manual text wrapping; limited renderer support for advanced features |
| **Template-driven** | Production, brand consistency | Less creative flexibility per slide; requires template library |

See [`template-vs-freeform-rendering.md`](template-vs-freeform-rendering.md).

---

## Worker responsibilities

```
1. Validate the renderable carousel against the schema.
2. Sanitize HTML/CSS/SVG (strip JS, remote URLs, etc.).
3. Resolve {{asset:<id>}} placeholders to real URLs.
4. Inject approved fonts.
5. Render each slide at canvas dimensions (default 1080×1350).
6. Optimize PNGs.
7. Upload to object storage.
8. Return render-result with asset URLs.
9. On failure: return structured errors per slide.
```

The worker does **not**:
- Generate content.
- Choose visual mode or accent color.
- Modify slot values (template mode).
- Cache renders across jobs.

---

## Recommended MVP path

1. Stand up a worker with Playwright + sharp + S3.
2. Implement Freeform HTML/CSS rendering (cheapest and most flexible).
3. Wire to the CRUD platform via the [`partner-creative-api`](../integration/partner-creative-api.md).
4. Ship 5-10 carousels manually-reviewed before automating.
5. Once stable, layer in template-driven sub-mode for production.

See [`playbooks/mvp-html-to-png-workflow.md`](playbooks/mvp-html-to-png-workflow.md).

---

## Recommended production path

1. Build a template library per brand (under the worker's control).
2. Switch to template-driven sub-mode for steady-state output.
3. Keep Freeform HTML/CSS available for one-offs and creative exploration.
4. Add SVG sub-mode if vector / editorial layouts become a recurring need.

See [`playbooks/template-driven-rendering-workflow.md`](playbooks/template-driven-rendering-workflow.md).

---

## Tool-agnostic guidance

The docs reference common tool *categories*:

- Headless browser renderer (Playwright, Puppeteer, Chromium).
- SVG renderer (`resvg`, `librsvg`, browser-based).
- Image optimizer (`sharp`, `imagemin`).
- Object storage (S3, R2, GCS, Cloudflare Images).

But the layer is not dependent on a specific vendor. Swap tools as needed.

---

## Related

- System framework: [`system/renderable-creative-framework.md`](../system/renderable-creative-framework.md)
- Module: [`modules/renderable-creative-system/`](../modules/renderable-creative-system/)
- Integration: [`integration/partner-creative-api.md`](../integration/partner-creative-api.md)
- Quality Gate (renderability): [`modules/quality-gate/renderability-checks.md`](../modules/quality-gate/renderability-checks.md)

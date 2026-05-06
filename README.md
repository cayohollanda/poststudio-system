# PostStudio System

> A documentation-first, prompt-first **Content Operating System** that turns Claude (or any capable LLM) into a senior content team for any brand — and ships results in three runtime modes: **Claude Project Manual Mode**, **API Production Mode**, and **Renderable Creative Worker Mode**.

PostStudio System is not an app. It's a layered repository of system instructions, modules, prompts, schemas, and templates that an LLM consumes to produce strategy, copy, visual direction, renderable creative code, and final asset specs — for any brand, niche, or platform.

You don't `npm install` it. You **upload it to a Claude Project**, or wire it into an external CRUD platform that calls Claude API and a separate rendering worker.

---

## Three runtime modes

```
┌────────────────────────────────────────────────────────────────────────────┐
│  Mode 1 — Claude Project Manual Mode                                       │
│  Upload the repo into a Claude Project. A human chats with Claude using    │
│  the prompts in prompts/. Outputs are read by humans, designed by hand.    │
│  Best for: founders, ghostwriters, agencies, prototyping new brands.       │
└────────────────────────────────────────────────────────────────────────────┘

┌────────────────────────────────────────────────────────────────────────────┐
│  Mode 2 — API Production Mode                                              │
│  An external CRUD platform stores brands, briefings, and jobs. It calls    │
│  Claude API using the same repo files as versioned context, validates      │
│  responses against schemas, runs Quality Gate, returns structured JSON.    │
│  Best for: SaaS platforms, internal content engines, multi-brand ops.      │
└────────────────────────────────────────────────────────────────────────────┘

┌────────────────────────────────────────────────────────────────────────────┐
│  Mode 3 — Renderable Creative Worker Mode                                  │
│  Claude returns HTML/CSS, SVG, or template-driven JSON. A separate worker  │
│  validates, sanitizes, and renders into final 1080×1350 PNG/JPG slides.    │
│  Best for: production pipelines that ship final assets, not just specs.    │
└────────────────────────────────────────────────────────────────────────────┘
```

The three modes are **the same repo**, used at different points of automation. The system instructions, schemas, and quality gates are shared.

---

## What this system produces

For any brand you plug in, the system can generate:

- **Brand diagnosis** — gaps in positioning, audience, offer, proof, voice, conversion path.
- **Brand pack** — 12 reusable files defining the brand's voice, audience, offer, claims, proof, visual identity, content pillars.
- **Content strategy** — pillars, recurring series, 30/90-day plans, content-to-offer mapping, distribution.
- **Carousels** — strategic angle, slide-by-slide copy, visual direction, captions, CTAs, alternative hooks.
- **Renderable carousels** — HTML/CSS, SVG, or template JSON ready for a worker to render into final 1080×1350 PNG/JPG.
- **Reels / short-video scripts** — talking-head, faceless, B-roll, founder POV, product demo, contrarian, launch.
- **Campaigns** — multi-post sequences for launches, authority, proof, comparisons, founder stories, waitlists.
- **Copy assets** — hooks, headlines, CTAs, captions, arguments, objection handling, contrarian copy.
- **Quality reviews** — adversarial scoring against a 30-point checklist; renderability checks; safety/claims audits.
- **Agency deliverables** — onboarding, monthly plans, handoffs, reporting templates.
- **API contracts** — request/response schemas, render-job specs, webhook events.

---

## Who this is for

- **Founders & operators** posting on LinkedIn / Instagram / X / TikTok and wanting a system, not vibes.
- **Content teams & ghostwriters** managing multiple brands without re-inventing the process.
- **Agencies** delivering content as a productized service.
- **AI/SaaS product teams** embedding content generation into their platform via API.
- **Devs building creative pipelines** who need a separate rendering worker to produce final image assets from LLM-generated code.

---

## Repository structure (top level)

```
poststudio-system/
├── README.md                      # You are here
├── LICENSE                        # MIT
├── CHANGELOG.md                   # Versioned updates
├── .gitignore
│
├── docs/                          # How-to guides for humans
├── system/                        # Universal rules — the OS kernel
├── modules/                       # 11 functional modules (diagnosis, pack-builder, …)
├── prompts/                       # 18 ready-to-use prompts (00-17)
├── schemas/                       # 17 JSON schemas for automation
├── brands/                        # One folder per brand
├── outputs/                       # Per-brand output archive (templates included)
├── integration/                   # API production layer (CRUD platform contract, schemas, playbooks)
├── rendering/                     # Renderable Creative Worker layer (HTML/CSS/SVG → PNG)
├── examples/                      # Fictional reference brand + worked outputs
└── playbooks/                     # End-to-end workflow recipes
```

For the full directory tree, see [`docs/module-overview.md`](docs/module-overview.md).

---

## The mental model

```
              ┌──────────────────────────────────────────────────┐
              │  system/  (universal rules, content OS kernel)   │
              └─────────────────────┬────────────────────────────┘
                                    │
              ┌─────────────────────┴────────────────────────────┐
              │  modules/  (focused capabilities — diagnose,      │
              │            pack, strategize, carousel, render,    │
              │            critique, deliver)                     │
              └─────────────────────┬────────────────────────────┘
                                    │
              ┌─────────────────────┴────────────────────────────┐
              │  brands/[slug]/  (your brand context)             │
              └─────────────────────┬────────────────────────────┘
                                    │
              ┌─────────────────────┴────────────────────────────┐
              │  prompts/00-17  (the action interface)            │
              └─────────────────────┬────────────────────────────┘
                                    │
              ┌─────────────────────┴────────────────────────────┐
              │  Output:                                           │
              │   • Markdown (for humans)                          │
              │   • JSON (for API + rendering)                     │
              │   • Renderable HTML/CSS/SVG (for the worker)       │
              └──────────────────────────────────────────────────┘
```

---

## Quick start by mode

### Mode 1 — Claude Project Manual Mode

1. Upload the repo to a new Claude Project as Project Knowledge.
2. Paste the custom instructions from [`docs/how-to-use-with-claude-projects.md`](docs/how-to-use-with-claude-projects.md).
3. Run [`prompts/00-start-here.md`](prompts/00-start-here.md) to orient Claude.
4. Run [`prompts/02-generate-brand-pack.md`](prompts/02-generate-brand-pack.md) to bootstrap a brand.
5. Run [`prompts/04-generate-carousel.md`](prompts/04-generate-carousel.md) to ship your first post.

### Mode 2 — API Production Mode

1. Read [`docs/api-production-mode.md`](docs/api-production-mode.md) and [`integration/claude-api-mode.md`](integration/claude-api-mode.md).
2. Implement the 5 minimum API operations (see [`integration/external-crud-platform-contract.md`](integration/external-crud-platform-contract.md)).
3. Pin the prompt version. Validate responses against [`schemas/`](schemas/).
4. Run [`integration/playbooks/generate-carousel-end-to-end.md`](integration/playbooks/generate-carousel-end-to-end.md).

### Mode 3 — Renderable Creative Worker Mode

1. Read [`docs/renderable-output-mode.md`](docs/renderable-output-mode.md) and [`rendering/architecture.md`](rendering/architecture.md).
2. Stand up a worker (Chromium/Playwright + image optimizer + object storage).
3. Implement the [`rendering/renderable-output-contract.md`](rendering/renderable-output-contract.md).
4. Run [`rendering/playbooks/mvp-html-to-png-workflow.md`](rendering/playbooks/mvp-html-to-png-workflow.md) for an MVP, or `template-driven-rendering-workflow.md` for production.

---

## End-to-end lifecycle

```
[Input] → [Diagnose] → [Brand Pack] → [Strategy] → [Brief]
       → [Generate Carousel] → [Quality Gate] → [Renderable Output]
       → [Render Job] → [Worker Renders PNGs] → [Deliver] → [Iterate]
```

Defined in detail in [`system/operating-system.md`](system/operating-system.md).

---

## The 11 modules

| Module | What it does | Mode support |
|---|---|---|
| Brand Diagnosis Agent | Audits a brand for gaps in positioning, audience, offer, proof, conversion. | 1, 2 |
| Brand Pack Builder | Interview-driven brand context bootstrap (12 files). | 1, 2 |
| Content Strategy OS | Pillars, series, 30/90-day plans, content-to-offer mapping. | 1, 2 |
| Carousel Machine | Strategic angle + slide-by-slide + visual + caption + CTA. | 1, 2, 3 |
| Visual Prompt System | 12 cinematic / editorial / surreal visual modes for image gen. | 1, 2 |
| Renderable Creative System | HTML/CSS, SVG, or template-driven JSON for slide rendering. | 2, 3 |
| Copy Vault | Reusable hooks, headlines, CTAs, captions, arguments. | 1, 2 |
| Reels Script Machine | 15s / 30s / 60s scripts with B-roll + on-screen text. | 1, 2 |
| Campaign Builder | 7/14/30-day multi-post campaigns. | 1, 2 |
| Quality Gate | Adversarial scoring, renderability checks, claim safety audit. | 1, 2, 3 |
| Agency Delivery Kit | Client onboarding, handoff, reporting, monthly plans. | 1 |

Plus the **Integration Layer** (`integration/`) and **Rendering Layer** (`rendering/`) which are infrastructure, not modules.

---

## How to add a new brand

1. Copy `brands/_template/` to `brands/your-brand-slug/`.
2. Run [`prompts/02-generate-brand-pack.md`](prompts/02-generate-brand-pack.md) to interview-fill the 12 files (or paste existing context).
3. Run [`prompts/01-run-brand-diagnosis.md`](prompts/01-run-brand-diagnosis.md) for a gap analysis.
4. Run [`prompts/03-generate-content-strategy.md`](prompts/03-generate-content-strategy.md) for pillars + 30-day plan.
5. Outputs land under `outputs/your-brand-slug/`.

---

## Quality and claims principles

- **Never invent** numbers, customers, awards, quotes, or comparisons not in the brief or `proof-assets.md`.
- **Mark gaps** with `[PROOF_NEEDED: short description]` and surface them in Missing Inputs.
- **Honor the Brand Pack** as a hard constraint — voice, words-to-avoid, claims-allowed, claims-forbidden.
- **Run Quality Gate** before shipping (Mode 1) or before rendering (Modes 2 & 3).
- **Adversarial review is separate from writing** — never approve your own draft in the same context.

See [`docs/claims-and-proof.md`](docs/claims-and-proof.md) and [`system/safety-and-claims-rules.md`](system/safety-and-claims-rules.md).

---

## Suggested tools (categories, not dependencies)

The repo is tool-agnostic. For real production, pair it with:

- **LLM** — Claude (recommended; long context + JSON), Claude API for production.
- **Image generation** — Midjourney, ChatGPT Images, Flux, Leonardo, Ideogram (for Mode 1 visuals).
- **Headless browser renderer** — Playwright / Puppeteer / Chromium (for HTML/CSS → PNG in Mode 3).
- **SVG renderer** — `resvg`, `librsvg`, or browser-based.
- **Image optimizer** — `sharp`, `imagemin`.
- **Object storage / CDN** — S3, R2, GCS, Cloudflare Images.
- **Layout (Mode 1)** — Figma, Canva, Paper.design, Photoshop.
- **Storage / CMS** — Notion, Drive, GitHub, your CRUD platform.

PostStudio System does not depend on any specific vendor.

---

## Use cases

- A founder running a personal LinkedIn + Instagram operation.
- A 3-person agency delivering carousels for 8 clients per month.
- A SaaS platform letting customers spin up a brand and generate posts via API.
- A consultancy productizing brand-diagnosis + monthly-content-plan as a fixed-price offering.
- An internal content team across 12 product lines needing voice consistency.
- A creator network that bills per rendered carousel and routes through a worker pipeline.

---

## Common workflows

- **Single carousel** — Mode 1, 60-90 minutes once the Brand Pack exists. See [`playbooks/single-carousel-workflow.md`](playbooks/single-carousel-workflow.md).
- **Monthly content engine** — Mode 1 or 2. See [`playbooks/monthly-content-engine-workflow.md`](playbooks/monthly-content-engine-workflow.md).
- **Brand onboarding** — Mode 1. See [`playbooks/brand-onboarding-workflow.md`](playbooks/brand-onboarding-workflow.md).
- **Launch campaign** — Mode 1 or 2. See [`playbooks/launch-campaign-workflow.md`](playbooks/launch-campaign-workflow.md).
- **API production** — Mode 2. See [`playbooks/api-production-workflow.md`](playbooks/api-production-workflow.md).
- **Renderable carousel pipeline** — Mode 2 + 3. See [`playbooks/renderable-carousel-production-workflow.md`](playbooks/renderable-carousel-production-workflow.md).

---

## Contribution

Fork it. Customize it. Upstream improvements that stay **brand-agnostic** are welcome:

- New carousel angles → `system/content-system.md`
- New visual modes → `system/visual-framework.md`
- New modules → `modules/`
- New prompts → `prompts/`
- New rendering recipes → `rendering/playbooks/`

PRs that hardcode real brands are closed. Use placeholders or the fictional example brands.

---

## License

MIT. See [`LICENSE`](LICENSE).

The example brands ("Lumen", "SignalForge") are fictional. Any resemblance to real companies is coincidental.

---

## Status

- **v2.0.0** — full Content Operating System: 3 runtime modes, 11 modules, 18 prompts, 17 schemas, integration layer, rendering layer.
- See [`CHANGELOG.md`](CHANGELOG.md) for what changed.

---

## Next steps

- Read [`docs/getting-started.md`](docs/getting-started.md).
- Skim [`system/operating-system.md`](system/operating-system.md) — the master lifecycle.
- Read [`docs/module-overview.md`](docs/module-overview.md) for the full module map.
- For Mode 2: read [`docs/api-production-mode.md`](docs/api-production-mode.md).
- For Mode 3: read [`docs/renderable-output-mode.md`](docs/renderable-output-mode.md).

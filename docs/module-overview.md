# Module Overview

> A map of the 11 functional modules + the integration and rendering layers. Use this to navigate.

---

## The 11 modules

```
modules/
├── brand-diagnosis-agent/        — audits a brand for gaps
├── brand-pack-builder/           — interview-driven brand context bootstrap
├── content-strategy-os/          — pillars, series, 30/90-day plans
├── carousel-machine/             — workhorse carousel generation
├── visual-prompt-system/         — image prompts, 12 visual modes
├── renderable-creative-system/   — HTML/CSS/SVG/template-driven output
├── copy-vault/                   — reusable hooks, headlines, CTAs, etc.
├── reels-script-machine/         — short-video scripts
├── campaign-builder/             — multi-post campaigns
├── quality-gate/                 — adversarial reviewer
└── agency-delivery-kit/          — onboarding, handoff, reporting
```

---

## What each module does

### Brand Diagnosis Agent
Audits a brand for gaps in 12 categories (positioning, audience, offer, proof, voice, content/visual consistency, CTAs, conversion path, profile, authority, messaging gaps). Returns scored audit with specific fixes.

**Inputs:** pasted brand context OR existing partial Brand Pack.
**Outputs:** `schemas/brand-diagnosis.schema.json`.
**Prompt:** `prompts/01-run-brand-diagnosis.md`.

### Brand Pack Builder
Interview or context-mode pack creation. Returns 12 Brand Pack files.

**Inputs:** user interview answers OR pasted website / decks / transcripts.
**Outputs:** 12 Markdown files matching `brands/_template/`.
**Prompt:** `prompts/02-generate-brand-pack.md`.

### Content Strategy OS
Pillars + recurring series + 30/90-day plan + content-to-offer mapping.

**Inputs:** Brand Pack + time window.
**Outputs:** `schemas/content-strategy.schema.json`.
**Prompt:** `prompts/03-generate-content-strategy.md`.

### Carousel Machine
The default carousel generator. Strategic angle + slide-by-slide + visual + caption + CTA + alt hooks.

**Inputs:** Brand Pack + topic + audience + platform + goal.
**Outputs:** `schemas/carousel-output.schema.json`.
**Prompt:** `prompts/04-generate-carousel.md`.

### Visual Prompt System
12 visual modes + reusable image-prompt templates. Refines image prompts per generator (Midjourney, Flux, ChatGPT Images, etc.).

**Inputs:** existing carousel + Brand Pack.
**Outputs:** refined image prompts per slide.
**Prompt:** `prompts/05-generate-image-prompts.md`.

### Renderable Creative System
The bridge between content generation and rendering. Produces HTML/CSS, SVG, or template-driven JSON.

**Inputs:** Brand Pack + carousel content + render mode + canvas + available fonts/assets/templates.
**Outputs:** `schemas/renderable-carousel.schema.json`.
**Prompt:** `prompts/13-generate-renderable-carousel.md`.

### Copy Vault
Reusable patterns. Hooks, headlines, CTAs, captions, arguments, objections, transitions, proof copy, launch copy, contrarian copy.

**Inputs:** referenced from other modules.
**Outputs:** pattern catalogs Claude pulls from.
**Files:** `modules/copy-vault/*.md`.

### Reels Script Machine
Short-video scripts (15s / 30s / 45s / 60s / 90s). Talking-head, faceless, B-roll-led, hybrid.

**Inputs:** Brand Pack + topic + audience + duration + script type.
**Outputs:** `schemas/reels-script.schema.json`.
**Prompt:** `prompts/06-generate-reels-script.md`.

### Campaign Builder
Multi-post campaigns (7 / 14 / 30 days). 15 campaign types.

**Inputs:** Brand Pack + campaign type + duration + objective.
**Outputs:** `schemas/campaign.schema.json`.
**Prompt:** `prompts/07-generate-campaign.md`.

### Quality Gate
Adversarial reviewer pass. Scores against 30-point rubric + 10-point renderability rubric. Identifies single highest-leverage fix.

**Inputs:** any module's output + Brand Pack + original brief.
**Outputs:** `schemas/quality-review.schema.json`.
**Prompt:** `prompts/08-critique-output.md`.

### Agency Delivery Kit
Operational kit for productizing PostStudio System as a service. Onboarding, discovery, handoff, reporting, scope, revisions.

**Files:** `modules/agency-delivery-kit/*.md`.

---

## Plus two infrastructure layers

### Integration Layer (`integration/`)
The contract between PostStudio System and an external CRUD platform. API schemas, prompt assembly, async job lifecycle, webhooks, multi-tenant boundaries, storage model, observability, security.

**Use when:** building a Mode 2 production pipeline.

### Rendering Layer (`rendering/`)
The technical layer for converting renderable JSON into PNG/JPG. Worker architecture, sanitization, Playwright, sharp, font/asset handling, slide dimensions, failure handling, QA.

**Use when:** building a Mode 3 rendering worker.

---

## How modules compose

Common chains:

### Brand onboarding
```
brand-pack-builder → brand-diagnosis-agent → content-strategy-os
```

### Single carousel (Mode 1)
```
carousel-machine → quality-gate → improve-loop (if needed)
```

### Renderable carousel (Mode 3)
```
carousel-machine → quality-gate (content) → renderable-creative-system → quality-gate (renderability) → render-job → worker
```

### Monthly cycle (agency)
```
content-strategy-os → (per post: carousel-machine | reels-script-machine | campaign-builder)
  → quality-gate per output
  → agency-delivery-kit (handoff, reporting)
```

### Campaign launch (Mode 2 / 3)
```
campaign-builder → (per post in sequence: carousel-machine + renderable-creative-system)
  → quality-gate per post → render → deliver
```

---

## Module independence

Each module is self-contained:

- Has its own `README.md`, `instructions.md`, output template, quality checklist.
- References shared `system/` files for universal rules.
- Outputs conform to a JSON schema in `schemas/`.

A new module can be added without disturbing existing modules. The `agency-delivery-kit/`, for instance, was added later without touching `carousel-machine/` or any other.

---

## Files per module (typical)

```
modules/<module-name>/
├── README.md             — what this is, when to use, mode support
├── instructions.md       — Claude's system prompt for this module
├── output-template.md    — output structure
└── quality-checklist.md  — module-specific checks
```

Some modules have additional files (templates, sub-mode rules, etc.) per their needs.

---

## Adding a new module

1. Create `modules/<module-name>/`.
2. Add `README.md`, `instructions.md`, `output-template.md`, `quality-checklist.md`.
3. Add a schema in `schemas/<module-name>-output.schema.json`.
4. Add a prompt in `prompts/<NN>-<module-name>.md`.
5. Update `system/master-instructions.md` boot sequence to reference the new module.
6. Update this overview.
7. Add an example in `examples/`.
8. Bump the repo version.

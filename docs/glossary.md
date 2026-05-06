# Glossary

> Terms used across PostStudio System. Use this as a reference when a doc or prompt uses unfamiliar vocabulary.

---

## Brand and content

**Brand Pack** — The 12-file structured context that grounds every generation for a brand. Lives at `brands/[slug]/`.

**Brand slug** — Lowercase, hyphenated identifier for a brand (e.g. `lumen`, `acme-co`). Matches the folder name.

**Pillar** — A top-level argument category. Each brand has 3-5. Topics live within pillars; series anchor on pillars.

**Series** — A repeating content format with a known cadence (e.g. "Operator Essays — weekly Wednesdays").

**Anchor post** — A past post that performed well and serves as a reference for future generations.

**Reference brand** — A brand whose voice the brand admires (without copying). Used for calibration, not plagiarism.

---

## Carousel structure

**Slide role** — The job a slide plays in the carousel. One of: hook, problem, mistake, reframe, mechanism, example, proof, step, trade-off, comparison, application, generalization, bonus, cta.

**Hook** — The first slide. Pattern interrupt + promise + tension. The most expensive slide to design.

**Mechanism** — The slide that explains *why* a claim is true. The slide that earns the rest of the carousel.

**Reframe** — The slide that replaces the audience's old belief with a new one.

**CTA (Call to Action)** — The single specific action you want the reader to take. One per post.

**Alternative hook** — A different hook for the same content. Every carousel ships with 5.

**Strategic angle** — The 1-3 sentence summary of the angle, tension, and audience fit. Opens every carousel output.

---

## Visual

**Visual mode** — One of 12 named visual treatments (Cinematic Dark, Editorial Minimal, Futuristic Interface, etc.). One per carousel.

**Accent color** — A single color (from the Brand Pack) applied selectively. Never two accents.

**Renderable** — Output (HTML/CSS, SVG, or template-driven JSON) that a worker can convert into a PNG/JPG image.

**Canvas** — The slide's pixel dimensions (default 1080×1350 = 4:5 aspect ratio).

**Safe margin** — The padding around the slide content. Default 48px on each side.

**Asset placeholder** — `{{asset:<id>}}` in renderable code. The worker resolves it to a real URL at render time.

---

## Quality and safety

**Quality Gate** — The adversarial reviewer pass. Scores against a 30-point rubric (and 10-point renderability rubric for Mode 3).

**Critical item** — A checklist item that, if it fails, blocks shipping. There are 10 critical items (content) + 7 critical items (renderability).

**`[PROOF_NEEDED]`** — Inline marker in slide bodies indicating a claim that requires evidence the system doesn't have. Surfaced in `Missing Inputs / Proof Needed`.

**`[MISSING_INPUT]`** — Inline marker indicating a Brand Pack field the user hasn't filled in.

**Trigger phrase** — A phrase that auto-routes to legal review (defined in `constraints.md`).

**Off-limits competitor** — A competitor we don't name in posts (per `constraints.md`).

---

## Runtime modes

**Mode 1 — Claude Project Manual Mode** — Human chats with Claude in a Claude Project. Markdown output.

**Mode 2 — API Production Mode** — External CRUD platform calls Claude API. JSON output, schema-validated.

**Mode 3 — Renderable Creative Worker Mode** — Mode 2 plus a rendering worker that produces PNG/JPG slides.

---

## Architecture

**CRUD Platform** — The external system of record. Stores brands, briefings, jobs, outputs. Orchestrates Claude API and worker calls.

**Rendering Worker** — The stateless service that converts renderable JSON into PNG/JPG.

**Tenant** — A customer of the CRUD platform.

**`prompt_version`** — The repo tag (e.g. `v2.0.0`) recorded with every job so generations are reproducible.

**`brand_pack_version`** — The version of the Brand Pack used for a generation.

---

## Prompts and modules

**Prompt (numbered)** — A copy-paste-ready prompt template (e.g. `prompts/04-generate-carousel.md`).

**Module** — A capability area (e.g. `modules/carousel-machine/`). Has README, instructions, output template, quality checklist.

**Module instructions** — The system prompt Claude reads when running that module.

**Output template** — The standard output shape per module.

---

## Schemas

**Schema** — A JSON Schema document (in `schemas/` or sub-folders) defining the shape of inputs and outputs.

**Renderable carousel** — JSON conforming to `schemas/renderable-carousel.schema.json`. Contains per-slide HTML/CSS/SVG/template payloads.

**Render job** — JSON conforming to `schemas/render-job.schema.json`. The CRUD platform → worker request.

**Render result** — JSON conforming to `schemas/render-result.schema.json`. The worker → CRUD platform response.

**Generation request / response** — Top-level API request/response envelopes (`integration/schemas/generation-request.schema.json` and `generation-response.schema.json`).

---

## Visual modes (the 12)

| # | Mode | Best for |
|---|---|---|
| 1 | Cinematic Dark | Founder, contrarian, premium |
| 2 | Editorial Minimal | Educational, B2B |
| 3 | Futuristic Interface | AI / SaaS launches |
| 4 | Surreal Product Metaphor | Storytelling, product-led |
| 5 | Premium Founder/Operator | Founder narratives |
| 6 | Technical Blueprint | Architecture posts |
| 7 | Exploded Diagram | Teardowns |
| 8 | Meme-but-Premium | Cultural moments |
| 9 | Luxury Object | Premium products |
| 10 | Abstract System | Frameworks |
| 11 | AI Command Center | Agent workflows |
| 12 | Before/After Split Scene | Transformations |

---

## Carousel structures (the 8)

```
1. Default (8 slides) — workhorse
2. Launch (10 slides) — product / feature / brand launches
3. Educational (9 slides) — concepts and frameworks
4. Comparison (8 slides) — X vs Y
5. Benchmark / Proof (9 slides) — data-driven
6. Thought Leadership (8 slides) — founder / executive
7. Product-Led (10 slides) — product is protagonist
8. Project Announcement (8 slides) — OSS / public artifacts
```

---

## Carousel angles (the 17)

```
1. Contrarian
2. Mistake
3. Hidden Feature
4. Before/After
5. Comparison
6. Myth vs Reality
7. Benchmark / Proof
8. Build in Public
9. Founder Perspective
10. Technical Breakdown
11. Workflow Reveal
12. Checklist
13. Opinionated Ranking
14. "You're Using X Wrong"
15. "Stop Doing X"
16. "Nobody Tells You This About X"
17. "I Tested X So You Don't Have To"
```

---

## Campaign types (the 15)

```
1. Product launch
2. Feature launch
3. Brand announcement
4. Authority building
5. Proof / case study
6. Comparison
7. Education series
8. Waitlist
9. Offer validation
10. Open-source / project announcement
11. Event / webinar / workshop
12. Founder story
13. Problem awareness
14. Objection handling
15. Re-engagement
```

---

## Roles and statuses

**Job status** — One of: created, queued, generating, generated, quality_gate_running, quality_gate_passed, quality_gate_failed, improving, improve_failed, renderable_generating, renderable_generated, render_validating, render_repair_needed, render_dispatched, rendering, render_complete, render_failed, approved_for_delivery, delivered, cancelled, failed, failed_pending_human_review.

**Verdict** — One of: Ship-ready, Ship with flags, Borderline, Rewrite required.

**Grade** — One of: pass, fail, risk.

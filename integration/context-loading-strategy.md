# Context Loading Strategy

> Which files to load per Claude API call. Minimize tokens; maximize signal.

---

## Layered loading

Load in this order. Cache stable layers; only the brief changes per request.

```
Layer 1 — Stable core (cache: forever per repo version)
  system/master-instructions.md
  system/operating-system.md
  system/safety-and-claims-rules.md
  system/api-output-rules.md
  system/output-rules.md
  system/quality-checklist.md

Layer 2 — Module-specific (cache: forever per repo version)
  modules/<module>/instructions.md
  + module-specific files (e.g. modules/carousel-machine/carousel-structures.md)
  + relevant system frameworks (e.g. system/content-system.md, system/carousel-framework.md)

Layer 3 — Brand-specific (cache: per brand, refresh on Brand Pack update)
  brands/<slug>/brand.md
  brands/<slug>/audience.md
  brands/<slug>/offer.md
  brands/<slug>/voice.md
  brands/<slug>/visual-style.md
  brands/<slug>/proof-assets.md
  brands/<slug>/constraints.md
  + remaining 5 brand pack files

Layer 4 — Schema reference (cache: forever per repo version)
  schemas/<output-module>.schema.json

Layer 5 — Brief (no cache; fresh per request)
  the user's brief
```

---

## Per-module load list

### Carousel Machine
- Layers 1, 4 (carousel-output.schema.json), 5
- Layer 2: `modules/carousel-machine/instructions.md` + `modules/carousel-machine/carousel-structures.md` + `system/content-system.md` + `system/carousel-framework.md` + `system/visual-framework.md` + `system/copywriting-rules.md`
- Layer 3: full Brand Pack

### Brand Diagnosis
- Layers 1, 4 (brand-diagnosis.schema.json), 5
- Layer 2: `modules/brand-diagnosis-agent/instructions.md` + `system/diagnosis-framework.md`
- Layer 3: existing partial Brand Pack (if any) + the user's pasted context (in the brief)

### Brand Pack Builder
- Layers 1, 4 (brand-pack.schema.json), 5
- Layer 2: `modules/brand-pack-builder/instructions.md` + `modules/brand-pack-builder/interview-flow.md` + `modules/brand-pack-builder/brand-pack-template.md`
- Layer 3: any existing partial Brand Pack to merge

### Renderable Carousel
- Layers 1, 4 (renderable-carousel.schema.json), 5
- Layer 2: `modules/renderable-creative-system/instructions.md` + `modules/renderable-creative-system/<sub-mode>-rules.md` + `system/renderable-creative-framework.md`
- Layer 3: full Brand Pack (especially `visual-style.md`)

### Quality Gate
- Layers 1, 4 (quality-review.schema.json), 5
- Layer 2: `modules/quality-gate/instructions.md` + `modules/quality-gate/critique-rubric.md` + `modules/quality-gate/scoring-system.md` + `modules/quality-gate/improvement-rules.md` + (renderable case) `modules/quality-gate/renderability-checks.md`
- Layer 3: Brand Pack (for context on voice / claims / constraints)
- Layer 5: the *output to critique* + the *original brief*

(Other modules follow the same pattern.)

---

## Token discipline

- Layer 1 + 2: ~5-15k tokens (varies by module).
- Layer 3 (full Brand Pack): ~3-8k tokens.
- Layer 4 (schema): ~1-3k tokens.
- Layer 5 (brief): ~200-1000 tokens.

Total per request: typically 10-25k tokens. Stays well within Claude's context window.

For long-context Claude (1M tokens), full repo loading is fine but wasteful. Selective loading is faster and cheaper.

---

## Caching

- **System layer:** cache by repo tag.
- **Module layer:** cache by repo tag + module name.
- **Brand layer:** cache by brand_id + Brand Pack version.
- **Schema layer:** cache by repo tag.
- **Brief:** never cache.

Use Anthropic's prompt caching where the SDK supports it. The cache hit on Layers 1-4 dramatically reduces cost and latency.

---

## When to load less

- For repair calls (`prompts/16-repair-renderable-output.md`): load only the renderable framework + the broken output + the worker error report. Skip non-relevant modules.
- For trivial tasks (e.g. "improve this single hook"): load only the hooks pattern library + Brand Pack voice + the input.
- For batch-processing flagged outputs: pre-cache the brand layer once, then iterate on briefs.

---

## When to load more

- For multi-step tasks (e.g. "diagnose, then build the pack, then strategize"): load the union of relevant modules. Or split into separate calls.
- For tasks crossing modules (e.g. carousel that references a campaign): load both module instructions.

---

## Brand Pack composite

Many CRUD platforms store the Brand Pack as a single composite blob rather than 12 files. That's fine. Translate to/from the 12-file shape at load time:

```javascript
function brandPackContext(brand) {
  return [
    `# Brand Pack — ${brand.slug}`,
    `## brand.md`, brand.brand,
    `## audience.md`, brand.audience,
    // ... 12 files
  ].join('\n\n');
}
```

Claude reads the concatenated form just as well.

# Prompt Assembly Strategy

> The exact composition of each Claude API call's `system` prompt + `messages`.

---

## The shape

```
system: [Layer 1] + [Layer 2] + [Layer 3] + [Layer 4]
messages[0].role = "user"
messages[0].content = [Layer 5: the brief]
```

Layers come from [`context-loading-strategy.md`](context-loading-strategy.md).

---

## Joining layers

Concatenate with a clear separator:

```
=== LAYER: SYSTEM CORE ===
<contents of system/master-instructions.md>

=== LAYER: SYSTEM CORE ===
<contents of system/operating-system.md>

[... other system files ...]

=== LAYER: MODULE — Carousel Machine ===
<contents of modules/carousel-machine/instructions.md>

[... other module files ...]

=== LAYER: BRAND PACK — lumen ===
<contents of brands/lumen/brand.md>

[... 12 brand pack files ...]

=== LAYER: SCHEMA REFERENCE ===
<contents of schemas/carousel-output.schema.json>
```

Use `===` (or any unambiguous separator) so Claude can mentally segment. Markdown `#` headings work too.

---

## The user message

The user message contains only the brief. Example:

```json
{
  "role": "user",
  "content": "Generate a carousel.\n\n- Brand: lumen\n- Topic: ...\n- Audience: ...\n- Platform: linkedin\n- Language: en\n- Goal: save\n- Signal: ...\n- Render: false\n\nReturn JSON only conforming to the carousel-output.schema.json reference above."
}
```

Don't restate the system instructions in the user message.

---

## Caching the stable layers

Layers 1-4 are stable for a given (repo_version, module, brand). Cache them.

When using Anthropic's prompt caching:

```
system: [
  { "type": "text", "text": "<Layer 1 + 2 + 4>", "cache_control": { "type": "ephemeral" } },
  { "type": "text", "text": "<Layer 3 — brand pack>", "cache_control": { "type": "ephemeral" } }
]
messages: [
  { "role": "user", "content": "<the brief>" }
]
```

This produces large cost reductions on repeated calls for the same brand.

---

## Schema reference inclusion

Including the JSON schema in the prompt helps Claude conform. Keep it minimal:

- Either include the full schema as JSON.
- Or include a markdown summary of the required fields and types.

Both work. Schemas with many `oneOf` branches benefit from the markdown summary; flat schemas can be passed verbatim.

---

## Multi-call workflows

Some tasks need multiple Claude API calls in sequence:

### Generate → Critique → Improve

```
Call 1: Generate Carousel
  system = Layer 1 + 2(carousel) + 3(brand) + 4(carousel-schema)
  user = brief
  → carousel JSON

Call 2: Critique
  system = Layer 1 + 2(quality-gate) + 3(brand) + 4(quality-review-schema)
  user = "Critique this output: <call 1 JSON>"
  → quality-review JSON

Call 3: Improve (if needed)
  system = Layer 1 + 2(carousel) + 3(brand) + 4(carousel-schema)
  user = "Apply this fix to the previous output: <fix from critique>. Original output: <call 1 JSON>."
  → improved carousel JSON
```

Each call is independent. The CRUD platform passes the relevant prior outputs as part of the user message.

### Generate → Renderable → Render

```
Call 1: Generate Carousel
  → content carousel JSON

Call 2: Generate Renderable
  system = Layer 1 + 2(renderable + sub-mode) + 3(brand) + 4(renderable-schema)
  user = "Convert this content carousel into renderable HTML/CSS: <call 1 JSON>. Render mode: html-css. Canvas: 1080x1350. Available fonts: [...]. Available assets: [...]."
  → renderable carousel JSON

Worker call: Render
  → render-result JSON
```

---

## Token budget guidance

| Module | Typical system prompt size |
|---|---|
| Carousel Machine | 12-18k tokens |
| Brand Diagnosis | 8-12k tokens |
| Brand Pack Builder | 10-14k tokens |
| Renderable Carousel | 14-20k tokens |
| Quality Gate | 10-15k tokens |
| Reels / Campaign | 10-14k tokens |

Brief: 200-1000 tokens.

Output: 2-8k tokens.

Total per call: typically 15-30k tokens.

---

## What to NOT include

- Examples of other brands' content (cross-contamination).
- Logs of past sessions (use the brand's `examples.md` instead).
- The full `outputs/_template/` directory (these are templates for downstream tooling, not for Claude).
- Internal notes / TODOs.

The prompt is a controlled context, not a knowledge dump.

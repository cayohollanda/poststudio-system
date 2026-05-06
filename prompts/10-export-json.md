# Prompt — Export Carousel JSON

> Converts an approved Markdown carousel into the JSON output contract, validated against `schemas/carousel-output.schema.json`.

This prompt exists for automation: feeding carousel data into design tools, scheduling tools, internal CMSs, analytics pipelines, or downstream LLM agents.

---

## How to invoke

Paste this prompt into Claude after a carousel has been generated and (preferably) approved via `prompts/critique-carousel.md`. Claude returns valid JSON only — no surrounding markdown.

---

## Prompt

```
You are operating PostStudio System.

Your job: convert a Markdown carousel into the JSON output contract defined by schemas/carousel-output.schema.json. Return ONLY valid JSON. No surrounding text, no commentary, no markdown fences unless explicitly requested.

# Inputs

- Brand: brands/[BRAND_SLUG]/                 (load brand.md, visual-style.md to confirm brand slug, accent color, visual mode)
- Carousel: [paste the full Markdown carousel output]
- Schema target version: [default: 1.0]
- Include schema reference: [true | false]   # whether to include the "$schema" field

# Process

1. Read the schema at schemas/carousel-output.schema.json.
2. Map every Markdown field into the corresponding JSON field.
3. Preserve all [PROOF_NEEDED] markers verbatim inside the relevant fields.
4. Convert checklist marks to {"item_name": "pass" | "fail" | "risk"}.
5. Surface missing inputs into the missing_inputs array.
6. Compute platform from the source carousel; default to "linkedin" if unspecified.
7. Validate field types: numbers as numbers, arrays as arrays, strings as strings.

# Output

Return ONLY valid JSON, conforming to schemas/carousel-output.schema.json. Top-level fields:

- $schema (optional, depends on input)
- version
- brand (slug)
- topic
- platform
- language
- goal
- angle
- structure
- visual_mode
- accent_color
- slides[] — each with: slide_number, role, headline, support_text, visual_concept, image_prompt, layout_instruction
- caption
- cta
- first_comment
- alternative_hooks[]
- quality_checklist:
  - critical: { item_key: pass | fail | risk, ... }
  - quality: { item_key: pass | fail | risk, ... }
  - quality_score (number)
  - quality_total (number, default 30)
- missing_inputs[]

# Behavior

- Do not invent fields not in the schema.
- Do not add commentary.
- If a field is empty in the source carousel, use null or "—" depending on the schema.
- Use ASCII-safe JSON (escape quotes, etc).
- Keep image_prompt strings unchanged; do not rewrite them.
- If "Include schema reference" is true, add "$schema": "../schemas/carousel-output.schema.json" at the top.
```

---

## Example output

```json
{
  "$schema": "../schemas/carousel-output.schema.json",
  "version": "1.0",
  "brand": "lumen",
  "topic": "why most AI tools fail at deep work",
  "platform": "linkedin",
  "language": "en",
  "goal": "save",
  "angle": "contrarian + mechanism",
  "structure": "default",
  "visual_mode": "cinematic-dark",
  "accent_color": "#E2A852",
  "slides": [
    {
      "slide_number": 1,
      "role": "hook",
      "headline": "Most AI tools quietly destroy your deep work.",
      "support_text": "And the chat is why.",
      "visual_concept": "A fractured glass dome over a wooden desk, late-light shadows raking across the wall.",
      "image_prompt": "A fractured glass dome over a wooden desk in a quiet study, late-afternoon light raking from the upper left, deep shadows, atmospheric haze, cinematic 35mm photograph, hyperreal materials, the dome is the central subject offset slightly to the upper-right, leaving the bottom-third of the frame empty for typography, no text, no logos, no watermark, no AI-typical hand anatomy, no flat illustration look",
      "layout_instruction": "Headline in the bottom-third left, white serif (brand headline face), accent #E2A852 on the word 'destroy'. Slide number top-right, brand mark bottom-right at 14px."
    }
  ],
  "caption": "...",
  "cta": "Save this for the next time you're about to install another AI extension.",
  "first_comment": "...",
  "alternative_hooks": [
    "I installed 14 AI tools in a quarter. My deep work collapsed.",
    "Stop installing AI tools that interrupt you.",
    "The fastest way to lose focus this year is to over-adopt AI.",
    "What if your AI stack is the bottleneck, not the unlock?",
    "Three signs your AI tools are taxing your attention without you noticing."
  ],
  "quality_checklist": {
    "critical": {
      "scroll_stops": "pass",
      "hook_earns_swipe": "pass",
      "one_idea_per_slide": "pass",
      "tension_resolved": "pass",
      "mechanism_shown": "pass",
      "voice_matches_brand": "pass",
      "no_invented_claims": "pass",
      "words_to_avoid_respected": "pass",
      "cta_one_action": "pass",
      "passes_external_review": "pass"
    },
    "quality": {
      "hook_specific": "pass",
      "no_stale_patterns": "pass",
      "audience_named_in_first_14_words": "pass",
      "slide_count_in_range": "pass",
      "structure_fits_count": "pass",
      "pacing_alternates": "pass",
      "each_slide_answers_previous": "pass",
      "visual_metaphor_present": "pass",
      "no_marketing_jargon": "pass",
      "no_filler_intensifiers": "pass",
      "headlines_in_range": "pass",
      "active_voice": "pass",
      "numbers_concrete_or_proof_needed": "risk",
      "one_visual_mode": "pass",
      "one_accent_color": "pass",
      "headline_in_bottom_third": "pass",
      "margins_consistent": "pass",
      "image_prompts_no_text_no_logos": "pass",
      "caption_has_comment_prompt": "pass",
      "first_comment_value_stacked": "pass"
    },
    "quality_score": 29,
    "quality_total": 30
  },
  "missing_inputs": [
    "Slide 6 (proof) needs real before/after numbers for deep-work session length."
  ]
}
```

---

## Validation

Before saving the JSON output, validate it against `schemas/carousel-output.schema.json`. You can do this:

- In Claude: "Validate this JSON against schemas/carousel-output.schema.json. Report any deviations."
- In your editor with a JSON Schema plugin (VS Code: Even Better TOML / JSON Schema).
- Programmatically with `ajv` (Node.js): `ajv validate -s carousel-output.schema.json -d carousel.json`.

Common validation failures:

- `accent_color` not a valid hex string.
- `slide_number` out of order or non-integer.
- `quality_checklist.critical` missing one of the 10 required keys.
- `alternative_hooks` length ≠ 5.

---

## Use cases

- **Design tool integration:** pipe slide data into Figma plugins or template engines.
- **Content scheduling:** populate Buffer / Hootsuite / Publer with caption + first comment + scheduled time.
- **Analytics:** log every published carousel with structured metadata for retroactive analysis.
- **Multi-platform expansion:** generate JSON once, then transform per platform (LinkedIn doc carousel vs Instagram image carousel).
- **Brand archive:** keep a structured archive of every carousel under `brands/[slug]/archive/`.

---

## When NOT to use this prompt

- The carousel is still in draft. Get the writer pass approved first.
- You're not consuming the JSON downstream. The Markdown output is the canonical human-readable form; converting to JSON without a use case is busywork.

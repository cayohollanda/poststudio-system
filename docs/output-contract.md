# Output Contract

> The exact format Claude returns when generating a carousel. The contract is enforced by `system/master-instructions.md` and validated against `schemas/carousel-output.schema.json`.

This contract exists so:

- Outputs are predictable.
- Designers and ops teams know where to look.
- Automation pipelines can parse the output.
- Reviewers can grade against a stable structure.

There are two output formats: **Markdown** (default, human-readable) and **JSON** (for automation).

---

## Markdown output (default)

When the user asks for a carousel, Claude returns this exact structure. Headings and order are non-negotiable. Empty fields are filled with `—`, never omitted.

```markdown
# Strategic Angle

[1-3 sentences. Name the angle from system/content-system.md, the tension, and why it works for this brand and audience.]

# Carousel

## Slide 1 — Hook
- Role: hook
- Headline: [the line that goes on the slide]
- Support text: [optional second line, max 8 words, or "—"]
- Visual concept: [what the image shows, in one sentence]
- Image prompt: [reusable prompt for image generators]
- Layout instruction: [headline placement, accent color, typography note]

## Slide 2 — [Role]
- Role: [problem / mistake / reframe / mechanism / example / proof / step / trade-off / application / generalization]
- Headline: ...
- Support text: ...
- Visual concept: ...
- Image prompt: ...
- Layout instruction: ...

[... continues for slides 3-N ...]

## Slide N — CTA
- Role: cta
- Headline: ...
- Support text: ...
- Visual concept: ...
- Image prompt: ...
- Layout instruction: ...

# Caption

[120-220 words for LinkedIn, 100-180 for Instagram, 80-140 for TikTok. Social-native voice. Ends with a comment-worthy prompt.]

# CTA

[The single action you want the viewer to take. One line.]

# First Comment

[A value-stacked first comment. 30-80 words. Could be a link, a bonus, a contrarian footnote, or a receipt.]

# Alternative Hooks

1. [Variant 1 — different angle or framing]
2. [Variant 2 — different angle or framing]
3. [Variant 3 — different angle or framing]
4. [Variant 4 — different angle or framing]
5. [Variant 5 — different angle or framing]

# Quality Checklist

Critical (must pass)
- [x] Slide 1 stops the scroll
- [x] Hook earns the swipe
- [x] One idea per slide
- [x] Tension named and resolved
- [x] Mechanism shown, not asserted
- [x] Voice matches Brand Pack
- [x] No invented claims
- [x] Words-to-avoid respected
- [x] CTA is one action
- [x] Could pass an external reviewer

Quality (each fail = flag, not block)
- [x] Hook is specific
- [x] No stale patterns
- [x] First 14 words name audience
- [x] 7-10 slides
- [x] Slide count fits structure
- [x] Pacing alternates pinch/release
- [x] Each slide answers previous
- [x] Visual metaphor present
- [x] No marketing jargon
- [x] No filler intensifiers
- [x] Headlines 6-12 words
- [x] Active voice
- [ ] Numbers concrete or [PROOF_NEEDED]   ← FLAG: Slide 6 needs real numbers
- [x] One visual mode used
- [x] One accent color
- [x] Headline in bottom third
- [x] Margins consistent
- [x] Image prompts say "no text, no logos"
- [x] Caption has comment prompt
- [x] First comment is value-stacked

Score: 29/30 — Ship-ready with 1 flag for proof in slide 6.

# Missing Inputs / Proof Needed

- [Each [PROOF_NEEDED] from the body, listed]
- [Each Brand Pack field that was absent and would have improved the output]
- [Each assumption made in the absence of clarity from the brief]
```

---

## Field-by-field reference

### Strategic Angle

- **Length:** 1-3 sentences.
- **Must name:**
  - The angle (from `system/content-system.md`'s 17 angles).
  - The tension (the fault line the carousel will exploit).
  - Why this angle fits this brand and audience.
- **Should not:**
  - Restate the topic in marketing language.
  - Be longer than 3 sentences.

### Slide blocks

Every slide block has six lines, in this order. Don't skip lines; use `—` if a field doesn't apply.

| Field | Description | Constraints |
|---|---|---|
| `Role` | The slide's job | One of: hook / problem / mistake / reframe / mechanism / example / proof / step / trade-off / application / generalization / cta / bonus |
| `Headline` | The slide's main line | 6-12 words; this is what gets typeset on the slide |
| `Support text` | Optional second line | 0-8 words; use `—` if not used |
| `Visual concept` | What the image shows | One sentence; describes the *idea*, not the prompt |
| `Image prompt` | Generator-ready prompt | Includes subject, environment, lighting, framing, style, negative; says "no text, no logos"; reserves typography space |
| `Layout instruction` | Designer-ready instruction | Headline placement, accent color usage, typography note, slide-number style |

### Caption

- **Length:** depends on platform. LinkedIn 150-250 words; Instagram 100-180; TikTok 80-140; X 100-150.
- **Structure:**
  - First line restates the hook differently.
  - Body argues the post's thesis in your own voice.
  - Second-to-last line: a comment-worthy prompt.
  - Last line: the CTA, on its own line.
- **Voice:** match `brands/[slug]/voice.md`.

### CTA

- **One line.**
- **One specific action** — never "save and share."
- **Energy matches the post.**

### First Comment

- **Length:** 30-80 words.
- **Job:** add value the post couldn't carry. One of:
  - The link the platform demoted.
  - A bonus or template.
  - A contrarian footnote.
  - A receipt (screenshot, quote, data point).

### Alternative Hooks

- **Always 5.**
- **Genuinely different from the chosen hook** — different angle, different audience entry, different framing.
- **Each is ≤14 words.**

### Quality Checklist

- **Use the checkmark format above** with `[x]` for pass and `[ ]` for fail/risk.
- **Critical items separated from quality items.**
- **Score line at the end** in the format `Score: N/30 — [verdict].`

### Missing Inputs / Proof Needed

- **List every `[PROOF_NEEDED]`** marker from the body.
- **List Brand Pack gaps** that would improve the output.
- **List assumptions** made when the brief was vague.

---

## JSON output (for automation)

When the user asks for JSON (or runs `prompts/export-carousel-json.md`), return only valid JSON matching `schemas/carousel-output.schema.json`. No surrounding Markdown.

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
      "visual_concept": "A fractured glass dome over a wooden desk, late-light shadows.",
      "image_prompt": "A fractured glass dome ... [full prompt]",
      "layout_instruction": "Headline bottom-third, white serif on dark, accent on 'destroy'."
    }
  ],
  "caption": "...",
  "cta": "Save this for the next time you're about to install another AI extension.",
  "first_comment": "...",
  "alternative_hooks": ["...", "...", "...", "...", "..."],
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
    "quality_score": 29,
    "quality_total": 30
  },
  "missing_inputs": [
    "Slide 6 needs real before/after numbers for the deep-work session length."
  ]
}
```

See `schemas/carousel-output.schema.json` for the full validating schema.

---

## What the contract does *not* allow

- **No surrounding commentary.** When asked for a carousel, return only the contract. No "Here's your carousel!" preamble. No "Hope this helps!" closing.
- **No skipping fields.** Every slide has all 6 fields. Use `—` for unused ones.
- **No inventing sections.** Don't add "Bonus Tips" or "Key Takeaways" sections.
- **No silent omissions.** If you didn't write a Quality Checklist, the output is incomplete.

---

## What the contract *does* allow

- **Variation in slide count** within 7-10 (or up to 10 for launches; up to 9 for educational/benchmark).
- **Variation in caption length** within platform sweet spots.
- **Variation in role names** within the role vocabulary.
- **Markdown emphasis** (italic, bold) inside fields where helpful — sparingly.
- **Light annotation in `[PROOF_NEEDED]`** explaining what's missing.

---

## Versioning

The contract is versioned. The current version is `1.0`. Future versions will:

- Be backward-compatible where possible.
- Be flagged in `CHANGELOG.md` and `schemas/carousel-output.schema.json`.
- Be opt-in via the `version` field in JSON outputs.

If you're using PostStudio System programmatically, pin to a specific version.

# Output Template — Carousel Machine

> Module-specific output reference. Master spec lives in [`docs/output-contract.md`](../../docs/output-contract.md). JSON schema: [`schemas/carousel-output.schema.json`](../../schemas/carousel-output.schema.json).

---

## Markdown form (Mode 1)

```markdown
# Strategic Angle

[1-3 sentences naming the angle from system/content-system.md, the tension, and why it works for this brand and audience.]

# Carousel

## Slide 1 — Hook
- Role: hook
- Headline: [the line that goes on the slide]
- Support text: [optional second line, max 8 words, or "—"]
- Visual concept: [what the image shows, in one sentence]
- Image prompt: [reusable prompt for image generators, includes "no text, no logos"]
- Layout instruction: [headline placement, accent color, typography note]

## Slide 2 — [Role]
[same fields]

[... slides 3-N ...]

## Slide N — CTA
[same fields]

# Caption

[Platform-appropriate length. Voice match. Ends with comment-worthy prompt.]

# CTA

[One line, one action.]

# First Comment

[30-80 words. Adds value the post couldn't carry.]

# Alternative Hooks

1. [variant — different angle/framing]
2. [variant]
3. [variant]
4. [variant]
5. [variant]

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

Quality (each fail = flag)
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
- [ ] Numbers concrete or [PROOF_NEEDED]   ← FLAG: Slide 6
- [x] One visual mode
- [x] One accent color
- [x] Headline in bottom third
- [x] Margins consistent
- [x] Image prompts say "no text, no logos"
- [x] Caption has comment prompt
- [x] First comment is value-stacked

Score: 29/30 — Ship-ready with 1 flag for proof in slide 6.

# Missing Inputs / Proof Needed

- [list every [PROOF_NEEDED] from the body]
- [list any Brand Pack fields that were absent and would have improved the output]
- [list any assumptions made when the brief was vague]
```

---

## JSON form (Mode 2)

See `schemas/carousel-output.schema.json` and the worked example in `examples/example-carousel-output.json`.

---

## Renderable JSON form (Mode 3)

If the request includes `render: true`, the output is `renderable-carousel.schema.json`-compliant JSON. See `modules/renderable-creative-system/renderable-output-template.md`.

---

## Notes

- Do not skip slide fields. Use `—` if a field doesn't apply.
- Honor the slide range (7-10).
- Generate exactly 5 alternative hooks.
- The Quality Checklist score must be honest. If you wouldn't approve this work as a reviewer, don't claim 30/30.
- The Missing Inputs section is the writer's exit signal — if the user wants better, they have a list of exactly what to provide.

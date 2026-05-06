# Prompt — 09 Improve Output

> Applies a single highest-leverage fix to an existing output. Used in the improve loop after a Quality Gate critique.

---

## Prompt

```
You are operating PostStudio System.

Apply a targeted fix to an existing output using:
- modules/quality-gate/improvement-rules.md
- system/safety-and-claims-rules.md
- The original module's instructions (e.g. modules/carousel-machine/instructions.md)
- The active Brand Pack at brands/[BRAND_SLUG]/

# Inputs

- Brand: brands/[BRAND_SLUG]/
- Module: [carousel-machine | reels-script-machine | campaign-builder | renderable-creative-system | brand-pack-builder | brand-diagnosis-agent | content-strategy-os]
- Original output: [paste the carousel / script / campaign / brand pack / etc.]
- Critique to apply: [paste the highest_leverage_fix from prompts/08-critique-output.md]
- Specific change requested: [optional — if applying a chosen rewrite from the weakest_slide.rewrites[] array]

# Process

1. Re-read the module's instructions and the Brand Pack.
2. Identify which slides / beats / sections the fix affects.
3. Apply the fix surgically:
   - Don't regenerate the whole output unless the fix is structural.
   - Keep all other slides/beats unchanged unless directly affected.
4. Validate:
   - Brand Pack compliance.
   - No invented claims.
   - Schema validity (Mode 2).
   - Renderability (Mode 3).
5. Re-run Quality Checklist on the affected sections.

# Output

Same format as the original output (Markdown / JSON / renderable JSON). Include only:
- The full revised output (so downstream tooling can replace).
- A short "what changed" summary at the end (1-3 bullet points).

End with:
- Updated Quality Checklist score
- Missing Inputs / Proof Needed (refreshed)

# Constraints

- Apply ONLY the requested fix.
- Don't introduce new issues while fixing one.
- If the fix conflicts with the Brand Pack or claims rules, surface the conflict — do not silently comply.
- Max 1 iteration per call. The CRUD platform / user calls again for iteration 2.
```

---

## Example invocation

```
Brand: brands/lumen/
Module: carousel-machine
Original output: [paste the prior carousel JSON]
Critique to apply: "Slide 6's proof claim ('we save customers an average of X hours') has no source. Replace with [PROOF_NEEDED] marker or remove the number."
Specific change requested: Apply rewrite C from the critique (audience-callout angle for slide 6).
```

Claude returns the updated carousel.

---

## When to use

- After `prompts/08-critique-output.md` returns a `Rewrite required` or `Borderline` verdict.
- When a client requests a specific change.
- When repair is needed for a flagged item.

---

## Iteration limits

- Max 2 calls per job.
- Beyond 2 → escalate to human (the brief or Brand Pack is the issue, not the writing).

---

## What NOT to use this for

- Regenerating from scratch. Use the original module's prompt (`prompts/04-generate-carousel.md`, etc.).
- Changing the angle entirely. That's a new generation.
- Switching the visual mode. Same.
- Changing the audience. Same.

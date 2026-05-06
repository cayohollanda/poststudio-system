# Prompt — 16 Repair Renderable Output

> Fixes broken renderable output given a worker error report. Loop max 2 iterations before escalating.

---

## Prompt

```
You are operating PostStudio System.

Repair a broken renderable carousel using:
- modules/renderable-creative-system/repair-rules.md
- system/renderable-creative-framework.md
- modules/renderable-creative-system/<sub-mode>-rules.md
- system/api-output-rules.md
- The active Brand Pack at brands/[BRAND_SLUG]/

# Inputs

- Brand: brands/[BRAND_SLUG]/
- Original renderable carousel: [paste the renderable-carousel.schema.json-compliant JSON]
- Worker errors: [list, with slide_number, error code, message, details]
- Iteration: [1 or 2]
- Render constraints (re-confirm): [canvas, available_fonts, available_assets, available_templates]

# Process

1. For each error in the worker report:
   - Identify the slide and root cause.
   - Apply the repair from modules/renderable-creative-system/repair-rules.md.
2. Repairs by error code:
   - TEXT_OVERFLOW → shorten headline OR re-break lines OR reduce max-width OR reduce font-size (last resort).
   - FONT_NOT_AVAILABLE → substitute via fallback chain; surface in warnings.
   - ASSET_NOT_FOUND → strip asset; flat color fallback; surface as missing input.
   - UNSAFE_HTML_FEATURE → substitute with worker-supported equivalent.
   - INVALID_SVG / INVALID_HTML → fix parser errors.
   - TEMPLATE_NOT_FOUND → pick closest match from available_templates; surface; or fall back to html-css.
   - SLOT_VIOLATION → truncate / rephrase to fit constraint.
   - RENDER_TIMEOUT → simplify the slide (reduce filters, complexity).
3. Preserve all unaffected slides unchanged.
4. Re-validate against renderable-carousel.schema.json.
5. Re-run renderability checklist on the affected slides.
6. Add repair_metadata block.

# Output

Return ONLY the full repaired renderable-carousel JSON (same shape as the original). Include:
- repair_metadata: { iteration, errors_addressed, changes_summary }
- Updated warnings[] / missing_inputs[]
- renderability_validated set correctly

If repair impossible (iteration 2 still failing same errors), return:

{
  "version": "1.0",
  "errors": [{ "code": "RENDER_REPAIR_FAILED", "message": "...", "remediation": "Escalate to human review." }]
}

# Hard rules

- Apply ONLY the listed repairs. Don't introduce new content changes.
- Don't repair an issue not in the error list.
- Don't break a previously-passing renderability check.
- Iteration 2 max — escalate beyond.
```

---

## Example invocation

```
Brand: brands/lumen/
Original renderable carousel: [paste]
Worker errors: [
  {
    slide_number: 4,
    code: "TEXT_OVERFLOW",
    message: "Headline at 76px exceeds canvas by 28px right.",
    details: { rendered_width_px: 1108, canvas_width_px: 1080 }
  },
  {
    slide_number: 6,
    code: "FONT_NOT_AVAILABLE",
    message: "GT Sectra Display not registered.",
    details: { fallback_used: "Times" }
  }
]
Iteration: 1
Canvas: { width: 1080, height: 1350 }
Available fonts: [
  { family: "Tiempos Headline", fallback_chain: ["Georgia", "serif"] },  ← note: GT Sectra removed
  ...
]
```

Claude returns the repaired carousel with shortened slide 4 headline and font-substituted slide 6.

---

## Repair iteration tracking

Iteration 1: first repair attempt.
Iteration 2: repair of iteration 1's residual errors.
Iteration 3+: do not run; escalate.

---

## Self-check after repair

- All listed errors addressed?
- No previously-passing checks now failing?
- Schema-valid?
- Renderability checks pass?

If any "no" → either return with explicit limitations or escalate.

---

## When NOT to use

- Errors aren't from the worker (e.g. a content critique). Use `prompts/09-improve-output.md` instead.
- The whole renderable is fundamentally broken (regenerate via `prompts/13-generate-renderable-carousel.md`).
- Iteration > 2. Escalate to human review.

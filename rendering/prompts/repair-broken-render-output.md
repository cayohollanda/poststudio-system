# Prompt — Repair Broken Render Output (rendering reference)

> Mirror of [`prompts/16-repair-renderable-output.md`](../../prompts/16-repair-renderable-output.md). Used to fix renderable carousel JSON given a worker error report.

---

## When to use

- Worker returned `partial_failure` or `failed`.
- Renderability Quality Gate flagged unsafe / overflow issues.
- A specific slide failed to render with a structured error.

## Inputs

- The original `renderable-carousel` JSON.
- The worker's error report (per-slide errors with codes and details).
- The active Brand Pack.

## Output

A repaired `renderable-carousel` JSON. Same shape as the original. The `repair_metadata` block summarizes what changed.

---

## Repair behaviors

| Error code | Repair behavior |
|---|---|
| `TEXT_OVERFLOW` | Shorten the headline; re-break lines; reduce `max-width`; reduce `font-size` (last resort). |
| `FONT_NOT_AVAILABLE` | Substitute font in the chain; surface in warnings. |
| `ASSET_NOT_FOUND` | Strip the asset; flat color fallback; surface as missing input. |
| `UNSAFE_HTML_FEATURE` | Remove the offending feature; substitute equivalent. |
| `INVALID_SVG` / `INVALID_HTML` | Fix parser errors. |
| `TEMPLATE_NOT_FOUND` | Pick closest match; surface; or fall back to html-css if allowed. |
| `SLOT_VIOLATION` | Truncate / rephrase slot value to fit constraint. |

---

## Iteration limits

- Max 2 repair iterations per slide.
- Beyond 2 → return without repair; surface as `failed_pending_human_review`.

---

## Self-checks

After repair, re-run the renderability checklist on the affected slides. If a previously-passing item now fails, you broke something — revert and try a different repair approach.

---

## See

- Canonical prompt: [`prompts/16-repair-renderable-output.md`](../../prompts/16-repair-renderable-output.md)
- Repair rules: [`modules/renderable-creative-system/repair-rules.md`](../../modules/renderable-creative-system/repair-rules.md)
- Failure handling: [`rendering/failure-and-retry-strategy.md`](../failure-and-retry-strategy.md)

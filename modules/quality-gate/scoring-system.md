# Scoring System

> How to compute scores, combine content + renderability passes, and produce the verdict.

---

## Pass / fail / risk

For every checklist item, the reviewer assigns:

- **pass** — clearly meets the bar.
- **fail** — clearly does not meet the bar.
- **risk** — borderline; passes today but is a known weakness.

Default to honesty over generosity. The reviewer's job is to find what the writer missed.

---

## Content score

```
content_pass = count of "pass" across 30 content items
content_total = 30
content_score = "{content_pass}/{content_total}"
```

A `risk` does not count as a pass. It's surfaced as a flag.

A critical-item `fail` overrides the score: regardless of total, the verdict is `Rewrite required`.

---

## Renderability score (Mode 3 only)

```
render_pass = count of "pass" across 10 renderability items
render_total = 10
render_score = "{render_pass}/{render_total}"
```

Critical renderability items (R1-R7): any fail blocks the render.

Quality renderability items (R8-R10): fail → flag, dispatch with warnings.

---

## Combined score (when both passes ran)

```
total_pass = content_pass + render_pass
total = content_total + render_total = 40
combined_score = "{total_pass}/{total}"
```

---

## Verdicts

For content-only review:

| Score | Verdict | Meaning |
|---|---|---|
| ≥ 27/30 | Ship-ready | All critical pass; quality flags acceptable |
| 24-26/30 | Ship with flags | Surface flags; consumer decides |
| 22-23/30 | Borderline | Consumer decides; recommend at least 1 repair pass |
| < 22/30 | Rewrite required | Don't ship; loop back to writer |

Override: any critical `fail` → `Rewrite required` regardless of score.

For renderability review:

| Score | Verdict |
|---|---|
| R1-R7 all pass | Cleared for render |
| Any of R1-R7 fail | Block render; invoke repair |
| R8-R10 fail | Dispatch with warnings |

---

## Combined verdicts (Mode 3 with both passes)

| Content verdict | Renderability verdict | Combined |
|---|---|---|
| Ship-ready | Cleared for render | Dispatch render |
| Ship with flags | Cleared for render | Dispatch render with warnings |
| Borderline | Cleared for render | Consumer decides |
| Rewrite required | * | Don't render; loop back |
| * | Block render | Repair before render |

---

## Iteration tracking

Every Quality Gate output includes an `iteration` field:

- `iteration: 1` — first review of the original output.
- `iteration: 2` — review of the first repair attempt.
- `iteration: 3+` — escalate to human review.

If the same critical item fails across two iterations, the writer doesn't understand the fix. Escalate.

---

## Score evolution across iterations

A repaired output should show measurable improvement:

```
Iteration 1: 21/30 — Rewrite required (3 critical fails)
Iteration 2: 26/30 — Ship with flags (0 critical fails, 4 quality flags)
Iteration 3: 28/30 — Ship-ready
```

If iteration 2's score is ≤ iteration 1's, the repair didn't work. Escalate.

---

## Reporting score in the output

```json
{
  "verdict": "Ship-ready",
  "iteration": 1,
  "content_score": { "pass": 28, "total": 30 },
  "render_score": { "pass": 10, "total": 10 },
  "combined_score": { "pass": 38, "total": 40 },
  "critical_fails": [],
  "quality_flags": ["numbers_concrete_or_proof_needed: slide 6 [PROOF_NEEDED]"],
  "renderability_flags": [],
  "highest_leverage_fix": "...",
  "weakest_slide_rewrites": [...]
}
```

The CRUD platform / consumer uses verdict + flags to decide next action.

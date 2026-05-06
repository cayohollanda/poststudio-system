# Playbook — Retry Low-Quality Output

> When the Quality Gate flags an output as `Rewrite required` or `Borderline`, the CRUD platform invokes the improve loop.

---

## Trigger

- Quality Gate verdict: `Rewrite required` (any critical fail) or `Borderline` (score 22-23/30 with no critical fails).
- Or: client-requested revision via `POST /jobs/{id}/revisions`.

---

## Stage 1 — Read the critique

The Quality Gate's response includes:

- `verdict`.
- `critical_items[]` with grades.
- `quality_items[]` with grades.
- `highest_leverage_fix` (paragraph).
- `weakest_slide.rewrites[]` (2-3 proposed rewrites).
- `safety_violations[]`.

Identify the single highest-leverage fix.

---

## Stage 2 — Decide approach

| Situation | Approach |
|---|---|
| 1 critical fail, narrow scope | Targeted fix via `prompts/09-improve-output.md` |
| 1-2 quality flags, narrow scope | Targeted fix |
| 3+ critical fails | Regenerate from sharper brief |
| Safety / claims violation | BLOCK; surface to user; do not auto-fix |
| Voice drift on multiple slides | Targeted voice fix; do not regenerate |
| Borderline (22-23) | Apply 1 fix; re-gate |

---

## Stage 3 — Improve API call

```python
improve_response = anthropic.messages.create(
    model="claude-opus-4-7",
    system=carousel_machine_system,  # same as original
    messages=[{
      "role": "user",
      "content": (
        "Apply this single fix to the previous output. Do not regenerate. "
        "Keep all other slides unchanged unless directly affected.\n\n"
        f"Fix: {highest_leverage_fix}\n\n"
        f"Original output: {json.dumps(original_output)}"
      )
    }],
    max_tokens=8000
)
```

Validate against schema. Store as iteration N.

---

## Stage 4 — Re-run Quality Gate

Stage 4 from `generate-carousel-end-to-end.md`. The QG must be a fresh API call, not a continuation.

---

## Stage 5 — Decide next iteration

| QG verdict on iteration N+1 | Next |
|---|---|
| Ship-ready | Mark approved; deliver |
| Ship with flags | Mark approved; deliver with flags surfaced |
| Borderline | iteration < 2 → loop again; iteration >= 2 → escalate |
| Rewrite required | iteration < 2 → loop again; iteration >= 2 → escalate |

---

## Iteration limits

- Max 2 improve iterations per job.
- Beyond 2 → mark `failed_pending_human_review`.
- Beyond 2 → log: "Improve loop exceeded budget; intervention required."

---

## When the same critical item fails twice

- The fix isn't reaching the actual problem.
- Don't loop again.
- Escalate. The brief or the Brand Pack is the issue, not the writing.

---

## Cost discipline

Each improve iteration is a Claude API call. Budget per job:

- 1 generation call.
- 1 Quality Gate call.
- Up to 2 improve iterations × (1 improve call + 1 QG call) = up to 4 additional calls.

Total worst case: 6 Claude API calls per job. Track cost; alert if a tenant trends above 4 per job consistently (signal of weak briefs or weak Brand Packs).

---

## Client-requested revisions

When the client (not the QG) requests revisions:

1. Apply the client's specific feedback (per `revision-rules.md`).
2. Run QG again — does the new version still meet quality bars?
3. If client feedback violated Brand Pack or claim rules, surface the conflict; do not silently comply.
4. Mark as a *revision iteration*, not an *improve iteration*. Track separately.

---

## Reporting

Track per brand and per module:

- Average improve iterations per job.
- % of jobs requiring 2+ iterations.
- Most common critical fails.

Persistent patterns inform Brand Pack refreshes and prompt-engineering improvements.

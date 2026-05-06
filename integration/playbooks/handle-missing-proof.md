# Playbook — Handle Missing Proof

> When Claude returns output with `[PROOF_NEEDED]` markers, what the CRUD platform does next.

---

## Why this happens

A slide or claim in the brief required proof not in `proof-assets.md`. Claude correctly refused to invent and surfaced the gap. Now the CRUD platform decides whether to ship, ask, or block.

---

## Detection

Every generation response includes a `missing_inputs[]` array. Each entry references a slide or section requiring proof.

```json
"missing_inputs": [
  "Slide 6 needs real before/after time numbers.",
  "Caption references a customer story; no story is in proof-assets.md."
]
```

---

## Decision tree

```
For each missing_inputs[] entry:
  ├─ Is the gap critical for shipping?
  │    ├─ Yes (proof slide is core to the post) → BLOCK
  │    └─ No (a caption could be reworded) → SHIP-WITH-FLAG
  │
  ├─ Can the user provide the proof now?
  │    ├─ Yes → ASK
  │    └─ No → DEFER
  │
  └─ Is there an existing piece of proof we missed?
       ├─ Yes → re-run with that proof in the brief
       └─ No → ASK or DEFER
```

---

## BLOCK action

- Mark job `failed_pending_human_review`.
- Fire `job.failed` webhook with `missing_inputs[]`.
- Surface in CRUD UI:
  > "Slide 6 requires real before/after numbers. Add to `proof-assets.md` or remove the claim. [Update Brand Pack] [Re-run]"

---

## SHIP-WITH-FLAG action

- Keep the `[PROOF_NEEDED]` marker visible in the artifact.
- Mark job `delivered`.
- Surface the flag in the consumer app: "This carousel ships with 1 proof gap. Review before publishing."
- The user (human) decides whether to:
  - Replace the marker with a real claim before publishing.
  - Remove the slide.
  - Publish as-is (rare; only when the marker is on a non-critical claim).

---

## ASK action

- Build a question for the user:
  > "Slide 6's headline needs a before/after time number. Can you provide it?"
- Surface in CRUD UI as a structured input form.
- On answer → re-run the generation with the proof appended to `proof-assets.md` or to the brief inline.

---

## DEFER action

- Save the partial output.
- Schedule a reminder to revisit in N days.
- Add the missing proof item to a "proof backlog" per brand.
- When the proof is captured, re-run the relevant carousels.

---

## Updating the Brand Pack

When new proof is captured:

1. Add to `brands/[slug]/proof-assets.md` (or DB equivalent) under the appropriate section.
2. Mark `last_reviewed: <date>`.
3. Increment Brand Pack version.
4. Re-run any pending jobs that needed this proof.

---

## What never happens

- The CRUD platform does NOT auto-fill `[PROOF_NEEDED]` markers with invented values.
- The CRUD platform does NOT silently strip the markers from the output.
- The CRUD platform does NOT publish to social platforms with the markers visible.

Markers are visible to the user; they must be resolved before publish.

---

## Reporting on missing proof

Track over time:

- Number of jobs with `missing_inputs[]`.
- Most-flagged proof gaps per brand.
- Time-to-resolution.

Patterns identify which Brand Packs need refreshing.

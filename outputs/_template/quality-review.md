# Quality Review — `[brand-slug]` — `[YYYY-MM-DD-topic-slug]`

> Template scaffold for `prompts/08-critique-output.md` outputs. See [`schemas/quality-review.schema.json`](../../schemas/quality-review.schema.json).

**Verdict:** [Ship-ready | Ship with flags | Borderline | Rewrite required]
**Iteration:** [1 | 2 | 3]
**Content score:** __/30
**Renderability score:** __/10 (when applicable)
**Combined score:** __/40

## Critical items

| # | Item | Grade | Note |
|---|---|---|---|
| 1 | Slide 1 stops the scroll |  |  |
| 2 | Hook earns the swipe |  |  |
| 3 | One idea per slide |  |  |
| 4 | Tension named and resolved |  |  |
| 5 | Mechanism shown, not asserted |  |  |
| 6 | Voice matches Brand Pack |  |  |
| 7 | No invented claims |  |  |
| 8 | Words-to-avoid respected |  |  |
| 9 | CTA is one action |  |  |
| 10 | Could pass an external reviewer |  |  |

## Quality items

[20 rows in the same format]

## Renderability items (when applicable)

| # | Item | Grade | Note |
|---|---|---|---|
| R1 | Schema valid |  |  |
| R2 | Canvas correct |  |  |
| R3 | No remote assets |  |  |
| R4 | No JavaScript |  |  |
| R5 | No invented logos |  |  |
| R6 | Fallback fonts |  |  |
| R7 | Single accent applied selectively |  |  |
| R8 | Headline overflow estimate |  |  |
| R9 | No banned CSS / SVG features |  |  |
| R10 | Metadata complete |  |  |

## Highest-leverage fix

[One paragraph naming the single change that would most improve the output.]

## Weakest slide and proposed rewrites

**Slide N — [role]**

Current headline: `[paste]`
Why it's weak: [1-2 sentences]

**Rewrite A** ([angle]): [proposed headline + support]
**Rewrite B** ([angle]): [proposed headline + support]
**Rewrite C** ([angle]): [proposed headline + support]

## Safety / claims violations

[List, or "None detected."]

## Slides to combine or kill

[List, or "None recommended."]

## Platform fit

[1-2 sentences on whether the output will perform on the specified platform.]

## Missing inputs / Proof needed

- [list]

## Repair recommendations

If verdict is not Ship-ready:

1. Apply the highest-leverage fix above.
2. Re-run `prompts/08-critique-output.md`.
3. If iteration 3+, escalate.

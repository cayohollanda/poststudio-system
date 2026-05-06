# Quality Checklist — Brand Diagnosis Agent

> Diagnosis-specific checks. Run before returning to the user.

---

## Critical (any fail → don't ship)

- [ ] All 12 categories scored or marked `_(unscorable)_`.
- [ ] Each score has a one-line justification.
- [ ] No invented metrics about the brand's current performance.
- [ ] No invented quotes from customers or audience.
- [ ] Every score ≤ 3 has a named, specific gap.
- [ ] Every recommended fix is concrete (a person could do it on Monday).
- [ ] Quick wins separated from strategic opportunities.
- [ ] `Missing inputs` and `Proof needed` sections present.
- [ ] Top 3 next actions identified.

## Quality (each fail → flag)

- [ ] Executive summary is 3-5 sentences (not a bulleted list).
- [ ] Problems ranked by severity, not by category order.
- [ ] No "improve everything" recommendation; recommendations are scoped.
- [ ] Recommendations cross-reference modules and prompts (e.g. "run prompts/02-generate-brand-pack.md").
- [ ] If the diagnosis identifies a broken foundation (4+ scores ≤ 2), recommends a Brand Pack rebuild before any tactical fix.
- [ ] No banned vocabulary in the diagnosis itself ("transformative recommendations," "innovative fixes," "world-class positioning").
- [ ] Honest about what couldn't be audited.

---

## Specificity test

For every recommendation, ask: "Could the user do this by Friday?"

- "Improve audience clarity" → fail. Vague.
- "Replace 'innovative platform' in your bio with 'For [ROLE] who [SITUATION]'" → pass.
- "Build authority" → fail. Vague.
- "Have the founder publish one essay on LinkedIn this week, on the topic of [TOPIC]" → pass.

If any recommendation fails the Friday test, rewrite it.

---

## Diagnosis mistakes to avoid

- **Scoring positively without evidence.** "Audience clarity: 4 — feels well-defined" → no, what's the evidence?
- **Scoring negatively without specificity.** "Trust: 2 — needs more proof" → what specifically is missing?
- **Recommending tactics that contradict the brand's stated constraints.** Read `constraints.md` if it exists.
- **Recommending "more posts" or "post more often."** Volume isn't the lever.
- **Recommending a tool stack the brand isn't using.** Stay in scope.
- **Missing the structural problem.** If the audience is "everyone," every other category is downstream of that. Name the upstream problem.

---

## Approval

A diagnosis passes when:
- All critical items pass.
- ≥ 5 of 7 quality items pass.
- The user can identify the one biggest unlock from the executive summary alone.

If it fails, return the partial diagnosis with explicit limitations stated.

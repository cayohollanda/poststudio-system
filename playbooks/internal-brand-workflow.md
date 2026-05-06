# Playbook — Internal Brand Workflow

> Using PostStudio System for an internal brand (not a client engagement). Mode 1 or Mode 2.

---

## Why this is different from agency / client work

Internal brands have:

- Closer access to product, founders, customer interviews.
- Tighter feedback loops on what works.
- More flexibility on scope.
- Often weaker formal "review" structure (which is a risk, not a feature).
- The same Quality Gate discipline as client work.

The system doesn't relax for internal brands. The discipline is the value.

---

## Setup (one-time)

Per [`brand-onboarding-workflow.md`](brand-onboarding-workflow.md), but with a few internal-specific notes:

- **Day 1 Brand Pack** can use richer context (PRDs, internal positioning docs, customer call recordings).
- **Day 2 Diagnosis** can be deeper because you have access to internal performance data.
- **Day 5 Strategy** should integrate with the product roadmap (launches, feature drops).

---

## Recurring monthly cycle

Same as [`monthly-content-engine-workflow.md`](monthly-content-engine-workflow.md), with adjustments:

- **Inputs:** integrate with the product team's calendar (sprint demos, launch days, customer escalations worth turning into content).
- **Approval flow:** typically faster than client work — designate one approver (usually the founder or chief marketer).
- **Quality Gate:** still mandatory. Internal teams are *more* prone to self-approval bias because the writer and approver are often the same person.

---

## Reviewer integrity (the internal trap)

The biggest risk in internal work: writer = reviewer = founder = approver.

Mitigations:

- Always run `prompts/08-critique-output.md` in a *new chat* (different context).
- If the team has 2+ people, alternate writer / reviewer roles.
- For high-stakes posts (launches, contrarian essays), use a third party reviewer or a different LLM.
- Track Quality Gate scores over time. If most posts ship at 30/30, the bar is too low.

---

## Founder voice management

If the founder is the public voice:

- They author or co-author Mira-style posts (operator essays, founder reflections).
- They review every post that uses their voice.
- They DO NOT approve their own draft in the same context (separate review pass).

If the founder isn't the public voice:

- Brand voice is "we" plural.
- Operator essays come from a designated voice owner.
- Founder posts reserved for milestones (launches, anniversaries, reflections).

---

## Cross-functional integration

Internal content engines benefit from cross-functional inputs:

- **Sales** — brings objections and customer questions into content.
- **Engineering** — supplies build-in-public material (Theo's engineering essays).
- **Customer success** — brings real-time customer reactions for proof.
- **Product** — coordinates content with the roadmap.

A monthly cross-functional sync (30 min) keeps the content engine fed.

---

## Capacity planning

For a single internal brand:

- 4-6 posts/week is sustainable for a 1-person content owner.
- 6-10 posts/week for a 2-person team.
- 10+/week requires additional resources.

Don't push past sustainable capacity. Posting cliffs hurt the brand more than slow consistency.

---

## When the founder writes

Some posts deserve the founder's hands directly. Don't system-generate everything:

- Manifesto-style essays.
- Personal mistake stories.
- Reflections after a milestone.
- Cultural moment responses.

The system can help structure these, but the founder authors the voice. Treat as a "voice-direct" post category.

---

## Internal Brand Pack maintenance

Internal brands tend to neglect Brand Pack updates because the team feels they "know the brand."

That's the trap. The system uses what's in `brands/[slug]/`, not what's in the team's heads.

Discipline:

- Quarterly Brand Pack refresh.
- Monthly proof updates as customer reactions come in.
- Voice updates as audience-adopted vocabulary emerges.

If `last_reviewed` dates in `proof-assets.md` are > 90 days old, the pack is stale.

---

## When internal becomes external

If you're growing the internal team and want to share the brand voice with new hires / contractors:

1. The Brand Pack is the onboarding doc. New writers read it before generating.
2. The system enforces voice consistency.
3. New writers run their first 5 carousels through Quality Gate from a colleague (not self-review).

---

## When to consider Mode 2

Switch to Mode 2 when:

- Internal team is generating 30+ posts/month.
- Multiple sub-brands or product lines need separate content engines.
- You want analytics integration (post performance feeding back into Brand Pack updates).
- Manual orchestration becomes the bottleneck.

Until then, Mode 1 is sufficient and avoids platform overhead.

---

## End-of-quarter reflection

Internal brands benefit from a quarterly retrospective:

- Top-performing posts (by reach, saves, comments, DMs, or whatever the brand measures).
- Patterns in what worked.
- Topics that under-performed.
- Audience signals.
- Brand Pack updates.
- Next-quarter strategy direction.

Run this as a 90-min team session. Refresh the Brand Pack and `content-pillars.md` based on findings.

# Agency Delivery Workflow

> Using PostStudio System as a productized service for clients. Onboarding → discovery → input → delivery → review → handoff → reporting.

The full implementation is in [`modules/agency-delivery-kit/`](../modules/agency-delivery-kit/). This file is the high-level walk-through.

---

## When this applies

- You run a content agency with multiple clients.
- You're a freelance ghostwriter / content strategist.
- You manage internal brand teams across product lines.
- You productize content as a fixed-price monthly engagement.

---

## The 7-stage flow

```
[Onboarding] → [Brand Pack + Diagnosis] → [Strategy] → [Monthly cycle: input → produce → QA → review → ship] → [Reporting] → [Refresh]
```

---

## Stage 1 — Onboarding (week 1)

Per [`modules/agency-delivery-kit/client-onboarding.md`](../modules/agency-delivery-kit/client-onboarding.md):

- **Day 0:** send `discovery-questionnaire.md` + `access-request-checklist.md`.
- **Day 1:** kickoff call (45-60 min).
- **Day 2-3:** Brand Pack draft via `prompts/02-generate-brand-pack.md` in context mode.
- **Day 4:** Brand Diagnosis via `prompts/01-run-brand-diagnosis.md`.
- **Day 5:** diagnosis review with client; agree on unlocks.
- **Day 6:** Content Strategy draft via `prompts/03-generate-content-strategy.md`.
- **Day 7:** Strategy approval + first carousel kickoff.

**End of week 1:** client has a complete Brand Pack, a diagnosis, a locked content strategy, and a first carousel draft.

---

## Stage 2 — Monthly cycle

### Per cycle inputs

Per [`modules/agency-delivery-kit/input-collection-checklist.md`](../modules/agency-delivery-kit/input-collection-checklist.md):

- Theme of the month.
- Upcoming events / launches / proof drops.
- New proof captured since last cycle.
- What worked / didn't last cycle.
- Audience reactions captured.
- Off-limits topics for the period.

Run a 30-min check-in call OR an async form.

### Generate the monthly plan

Use `prompts/12-create-monthly-content-plan.md`. Output: 16-22 posts for the month, day-by-day, tagged by pillar.

### Per deliverable production

Per [`modules/agency-delivery-kit/delivery-workflow.md`](../modules/agency-delivery-kit/delivery-workflow.md):

```
Brief → Generate → Internal QA → Client review → Apply feedback → Final QA → Deliver
```

For each post:
- Run the appropriate generation prompt (carousel / reels / etc.).
- Run `prompts/08-critique-output.md` in a separate context for QA.
- Apply max 1 improve iteration.
- Send via `modules/agency-delivery-kit/handoff-template.md`.

### Internal vs client review

Per [`modules/agency-delivery-kit/review-workflow.md`](../modules/agency-delivery-kit/review-workflow.md):

- **Internal pass:** writer + reviewer (separate roles even if same person).
- **Client pass:** client + designated approver. Default 24-48h SLA.

The internal pass catches Brand Pack violations and claims issues. The client pass catches voice fit and factual accuracy.

### Revisions

Per [`modules/agency-delivery-kit/revision-rules.md`](../modules/agency-delivery-kit/revision-rules.md):

- 1 revision round included per deliverable.
- Additional rounds billed.
- Capture in `client-feedback-form.md`; log to `brands/[slug]/feedback-log.md`.

### Scope

Per [`modules/agency-delivery-kit/scope-boundaries.md`](../modules/agency-delivery-kit/scope-boundaries.md):

- In scope: Brand Pack, diagnosis, strategy, carousels, reels, campaigns, image direction, QA, monthly reporting.
- Out of scope: image production, final publishing, paid ads, email, blog posts, sales pages.

When a client expands scope informally, formalize it.

---

## Stage 3 — End-of-month reporting

Per [`modules/agency-delivery-kit/reporting-template.md`](../modules/agency-delivery-kit/reporting-template.md):

- What shipped (counts, formats, topics).
- What worked / didn't (specific posts).
- Audience signal (new vocabulary, reactions worth quoting).
- Brand Pack updates proposed.
- Risks for next month.
- Action items for client + agency.

Run on the last working day of the month. Send within 2-3 working days of month-end.

---

## Stage 4 — Quarterly Brand Pack refresh

Every 3 months:

- Re-run `prompts/01-run-brand-diagnosis.md`.
- Update `brands/[slug]/proof-assets.md` with new proof.
- Update `brands/[slug]/voice.md` with audience-adopted vocabulary.
- Update `brands/[slug]/examples.md` and `rejected-examples.md` with the quarter's evidence.
- Re-run `prompts/03-generate-content-strategy.md` for the next quarter.

---

## Time budget

| Stage | Internal time |
|---|---|
| Onboarding (week 1) | 6-10 hours |
| Per carousel | 90-180 min |
| Per reels script | 60-90 min |
| Per 7-day campaign | 4-6 hours |
| Monthly plan | 6-10 hours |
| End-of-month report | 2-3 hours |
| Quarterly refresh | 4-6 hours |

Add client review time on top.

---

## Storage layout per client

```
brands/[client-slug]/
  brand.md, audience.md, ... (12 files)
  feedback-log.md (running log of revisions)
  changelog.md (your private notes)

outputs/[client-slug]/
  YYYY-MM-DD-topic-slug/
    raw/
    qa/
    review/
    final/
    assets/
    notes.md
```

Per cycle keeps everything traceable.

---

## What the kit doesn't do

- Doesn't replace business development. You bring the client.
- Doesn't replace strategy taste. The system amplifies your judgment, not substitutes.
- Doesn't replace human review for new brands' first 5 deliverables.
- Doesn't replace legal / compliance review for regulated categories.

It standardizes the *operational* parts so the *strategic* parts get more attention.

---

## Multi-client structure

For a 3-10 client agency:

- One `brands/[slug]/` per client.
- One Claude Project per client (or one shared with explicit brand context per chat).
- Output archive per client.
- Scope agreements documented per client.
- Reviewer rotation across team members.

For 10+ clients, layer in API mode (Mode 2) for the production-line parts.

---

## Productization checklist

Before launching as a service:

- [ ] PostStudio System repo cloned and pinned.
- [ ] Discovery questionnaire customized for your service.
- [ ] Standard onboarding email + access checklist.
- [ ] Pricing per cycle defined (Brand Pack + monthly cycle + add-ons).
- [ ] Scope boundaries clear in your service agreement.
- [ ] Revision policy clear (1 round included, more billed).
- [ ] Reporting cadence defined.
- [ ] Internal QA process in place (writer + separate reviewer).
- [ ] Output archive structure decided.
- [ ] Brand Pack refresh cadence agreed.

---

## Next reading

- [`modules/agency-delivery-kit/README.md`](../modules/agency-delivery-kit/README.md) for the full kit.
- [`prompts/11-create-client-delivery.md`](../prompts/11-create-client-delivery.md) for handoff packaging.
- [`prompts/12-create-monthly-content-plan.md`](../prompts/12-create-monthly-content-plan.md) for monthly plan generation.
- [`playbooks/client-delivery-workflow.md`](../playbooks/client-delivery-workflow.md) for the operational checklist.

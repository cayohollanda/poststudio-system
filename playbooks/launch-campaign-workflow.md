# Playbook — Launch Campaign Workflow

> 14-day product / feature / brand launch. Setup → Pressure → Reframe → Proof → Ask. Mode 1 or Mode 2.

---

## Pre-launch (T-21 to T-15)

- [ ] Hard ship date confirmed.
- [ ] Brand Pack updated with launch-specific proof (early access user stories).
- [ ] Constraints reviewed (compliance for the launch claims).
- [ ] Visual assets queued (hero shots, product UI samples, founder photos if applicable).
- [ ] Pricing / availability cleared for public statement.
- [ ] Press / partnership pre-announcements coordinated (if any).

---

## T-14 — Generate the campaign

```
Use prompts/07-generate-campaign.md.
Brand: brands/[slug]/
Campaign type: product-launch
Duration: 14 days
Objective: [specific — e.g. drive 50 trial signups]
Primary CTA: try (or DM, depending on funnel)
Audience: [role + situation]
Pre-existing assets: [list]
Audience day-0 mental state: [...]
Audience day-N mental state: [...]
```

Returns a 14-day post sequence. Lock it.

---

## T-13 to T-1 — Production sprint

For each day's planned post, run [`single-carousel-workflow.md`](single-carousel-workflow.md) with these adjustments:

- **Setup posts (Days 1-3)** — focus on naming the audience pain. Don't mention the launch yet.
- **Pressure posts (Days 4-6)** — visible cost of the default. Tension building.
- **Reframe posts (Days 7-9)** — introduce the new mental model (without naming the product).
- **Proof posts (Days 10-12)** — case studies, demos, benchmarks. Real proof from `proof-assets.md`.
- **Ask posts (Days 13-14)** — the launch (Day 13) and objection-handling (Day 14).

Front-load production: aim to have Days 1-7 fully drafted by T-7.

---

## Day 13 — Launch

Pre-launch checklist (morning of):

- [ ] All visual assets approved.
- [ ] Caption + first comment ready.
- [ ] Link in bio updated.
- [ ] Pricing page live and tested.
- [ ] Trial signup flow confirmed.
- [ ] Customer support brief sent (if applicable).
- [ ] Founder available for first 4-6 hours of comments.
- [ ] Quality Gate passed on the launch post.

Launch post goes live at the brand's optimal time (typically Tue-Thu morning for B2B).

Within first hour:
- Drop the value-stacked first comment.
- Repost from the brand account if multi-channel.
- Founder pinned reply with a personal note.

---

## Day 14 — Objection handling

The post that addresses the top 3 objections from Day 13's comments:

```
Use prompts/04-generate-carousel.md.
Topic: addressing the top 3 questions / objections from yesterday's launch
Angle: Objection Handling
Signal: real comments from Day 13 (paste verbatim)
```

Ship same-day if possible.

---

## Days 15-21 — Post-launch

- Capture trials / signups / DMs into `brands/[slug]/proof-assets.md` (with permission).
- Surface what landed / didn't in `examples.md`.
- Plan a Day 18-21 "what we learned shipping" post if pattern is interesting.

---

## Compressed version (7-day launch)

For feature launches or smaller releases:

```
Day 1: Setup
Day 2: Pressure
Day 3: Reframe
Day 4: Proof
Day 5: Use case 1
Day 6: Use case 2
Day 7: Launch + ask
```

Run the same production sprint at half the duration.

---

## Extended version (30-day launch)

For category-defining launches:

```
Days 1-7: Setup + Pressure (heavy authority push on the problem)
Days 8-14: Reframe (new category framing)
Days 15-21: Proof (case studies, benchmarks)
Days 22-28: Ask (launch + objection handling + comparison content)
Days 29-30: Reflection / what we learned
```

This is a major commitment. Only run when the launch warrants 30 days of attention.

---

## Common launch failures

- **Burying the launch on slide 5.** If the hook is "we built X," slide 5 is too late. Tease on slide 1.
- **No "who it's not for" statement.** Honest scope earns more trust than hype.
- **Multi-CTA chaos.** Pick one ask per post; the campaign drives one ask total.
- **Proof posts before the audience accepts the framing.** If the audience hasn't bought into the reframe, proof reads as marketing. Sequence matters.
- **No objection-handling post.** Day 14 (or post-launch day) is mandatory.

---

## Mode 2 / 3 launch

In API mode, the campaign builder generates the 14-day plan, then the CRUD platform queues 14 generation jobs (one per planned post). Quality Gate runs per post. Mode 3 renders each. Asset URLs flow to the scheduler. Same logical flow; automated orchestration.

---

## After-launch report

Within 7 days post-launch:

- Trials / signups / DMs counted.
- Top-performing posts identified.
- Audience reactions captured.
- Lessons documented in `brands/[slug]/examples.md`.
- Brand Pack updated.

If the launch underperformed: run `prompts/01-run-brand-diagnosis.md` against the campaign to identify what shifted.

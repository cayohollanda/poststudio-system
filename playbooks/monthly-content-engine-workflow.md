# Playbook — Monthly Content Engine Workflow

> The recurring monthly cycle. Mode 1 or Mode 2. ~16-22 posts shipped per month per brand.

---

## Cycle structure

```
[End of month N-1: input collection + reporting + plan generation]
    ↓
[Day 1 of month N: first post ships]
    ↓
[Days 1-30: weekly production cadence]
    ↓
[End of month N: reporting + plan for month N+1]
```

---

## End-of-prior-month tasks

### EOM-3 (3 days before month-end)
- Send `input-collection-checklist.md` to client/team async.
- Collect: theme, upcoming events, new proof, last-cycle results, off-limits topics.

### EOM-2
- Receive inputs. Update Brand Pack:
  - `proof-assets.md` (new proof).
  - `voice.md` (new audience-adopted vocabulary).
  - `examples.md` / `rejected-examples.md` (last-cycle outcomes).

### EOM-1
- Run `prompts/12-create-monthly-content-plan.md` for next month.
- Send to client/team for approval (24-48h SLA).

### EOM (last working day)
- Send the prior-month report (`prompts/11-create-client-delivery.md` adapted, or use the reporting template).

### Day 1 of new month
- First scheduled post ships. Production loop begins.

---

## Weekly production cadence

For each week of the month:

### Monday
- Review week's planned posts (from `monthly-content-plan`).
- Confirm any inputs needed (founder availability, customer story permissions).
- Run [`single-carousel-workflow.md`](single-carousel-workflow.md) for week's first 1-2 posts.

### Tuesday
- Continue production. Apply [`renderable-carousel-production-workflow.md`](renderable-carousel-production-workflow.md) if Mode 3.
- Schedule posts for the week.

### Wednesday
- Series anchor day for many brands (e.g. "Operator Essays" weekly cadence).
- Light review: anything urgent?

### Thursday
- Buffer day. Catch up on any production slippage.
- Capture proof from posts shipped earlier in the week.

### Friday
- Engineering / data-drop posts (if brand has them).
- Light reflection: what worked this week?

---

## Mid-month checkpoint (day 15)

- Review month-to-date performance (qualitative if no analytics access).
- Adjust week 3-4 plan if needed (campaigns shifting, audience signals changing).
- Confirm proof needed for upcoming proof drops.

---

## Per-deliverable production loop

For each post in the plan:

```
[1] Brief (from plan)
[2] Generate (carousel-machine / reels-script-machine / etc.)
[3] Internal QA (prompts/08-critique-output.md)
[4] Apply fix if needed (max 1 iteration)
[5] Image prompts + visuals (Mode 1) OR renderable + render (Mode 3)
[6] Layout / final asset
[7] Caption + post + first comment
[8] Capture proof
```

Per-deliverable time: 60-180 min depending on format and stage.

---

## Capacity planning

For a 4-post-per-week cadence × 4 weeks = 16 carousels per month per brand:

- Solo operator: ~24-32 hours/month per brand.
- 2-person team: ~12-16 hours/month per brand each.

If running 5+ brands, automation becomes essential — switch to Mode 2 + Mode 3.

---

## Common monthly hiccups

| Symptom | Fix |
|---|---|
| Plan doesn't ship Monday | catch up Tuesday; never let posts slip past the planned week |
| Founder unavailable for series | substitute with operator essay or skip and pick up next week |
| Proof drop missing real data | reschedule to next month; replace with founder essay |
| Posts underperforming | mid-month adjustment to angles / hooks; capture pattern in `rejected-examples.md` |
| Client revisions piling up | confirm scope and revision policy; surface pattern in feedback log |

---

## Mode 2 / 3 monthly cycle

In API production:

- The CRUD platform schedules generation jobs from the monthly plan.
- Each post triggers `prompts/04-generate-carousel.md` (or reels / campaign equivalent).
- Quality Gate runs automatically.
- Mode 3: renderable + render run automatically.
- Final assets and metadata land in the consumer app.
- Webhooks fire on each completion.

The cycle is the same; the orchestration is automated.

---

## End-of-cycle reflection

What this cycle taught us:

- Patterns that worked (add to `examples.md`).
- Patterns that didn't (add to `rejected-examples.md`).
- Audience vocabulary that emerged (add to `voice.md`).
- Proof captured (add to `proof-assets.md`).

The Brand Pack ends each cycle sharper than it started.

---

## Brand Pack version bumps

Each cycle's updates roll into a new Brand Pack version. Track in `brands/[slug]/changelog.md` (private). Mode 2 records `brand_pack_version` per job for audit.

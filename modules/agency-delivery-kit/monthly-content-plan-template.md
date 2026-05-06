# Monthly Content Plan Template

> The deliverable the client receives at the start of each month. Lists every post planned for the next 30 days.

---

## Structure

```markdown
# Content Plan — [Brand] — [Month YYYY]

## Theme of the month

[1 sentence. What's the focus? E.g. "interrupt-budget thesis"]

## Active campaigns

- **Campaign A:** [name] — runs days [N-M]
- **Campaign B:** [name] — runs days [N-M]
- **Background drumbeat:** [recurring series episodes]

## Cadence

- Total posts: [N]
- Channels: [linkedin / instagram / tiktok / x / multi]
- Posting days: [Mon / Wed / Fri default]

## 30-day plan

| Day | Date | Topic | Format | Pillar | Channel | Primary CTA | Status | Notes |
|---|---|---|---|---|---|---|---|---|
| 1 | YYYY-MM-DD | [topic] | carousel | Pillar 1 | LinkedIn | save | drafted | series anchor |
| 3 | YYYY-MM-DD | [topic] | reel 30s | Pillar 2 | LinkedIn + IG | DM | brief sent | proof drop |
| 5 | YYYY-MM-DD | [topic] | carousel | Pillar 1 | LinkedIn | follow | planned | founder essay |
| ... | | | | | | | | |

## Required inputs from client (this month)

- [ ] Founder availability for posts on Day [N], Day [M].
- [ ] Customer story permissions for Day [N] proof drop.
- [ ] Confirmation of compliance flags for Day [N] (if any).

## Risks

- [list — timing conflicts, missing proof, audience fatigue]

## Success metrics for this month

- [Specific signals to watch — saves on series posts, comments on contrarian posts, DMs after launch]

## End-of-month review date

[YYYY-MM-DD — when we'll review with the client]
```

---

## Plan generation

Run `prompts/12-create-monthly-content-plan.md`. The prompt assembles inputs from:

- Brand Pack at `brands/[client-slug]/`.
- Past month's `examples.md` updates.
- Active campaigns in flight.
- Client's input from `input-collection-checklist.md`.

---

## Approval flow

1. Send the plan to the client by EOM-1 (one day before the new month starts).
2. Client reviews within 24-48h.
3. Apply any direction changes.
4. Lock the plan; begin shipping on Day 1.

---

## When the plan changes mid-month

- New launch surfaces → revise the second half.
- Customer event lands → redirect 1-2 slots.
- Proof drop earlier than expected → bring forward a proof campaign.
- Major industry shift → pause the planned content; rapid-respond.

Document changes in the same plan file with a "revisions" section so the audit trail survives.

---

## What NOT to do

- Don't fill the plan with placeholders ("topic TBD"). If a topic isn't decided, don't put it on the plan.
- Don't lock the plan rigidly — leave space for opportunistic posts.
- Don't ship the plan without an associated input collection.

# Client Onboarding

> The first week. Sets expectations, captures inputs, generates the foundational artifacts.

---

## Day 0 — Pre-kickoff

**Sent before the first call:**

- Welcome email with engagement summary, timeline, points of contact.
- `discovery-questionnaire.md` — async form to collect brand context.
- `access-request-checklist.md` — what we need from them to start.

---

## Day 1 — Kickoff call (45-60 min)

**Agenda:**

1. Introductions + roles (5 min).
2. Engagement scope walkthrough (10 min) — reference `scope-boundaries.md`.
3. Audience deep-dive (15 min) — beyond what they wrote in the questionnaire.
4. Voice calibration (10 min) — show 2-3 reference brands; ask which feels right.
5. Visual mood (10 min) — show 2-3 of the 12 visual modes from `system/visual-framework.md`.
6. Deliverables + cadence (5 min) — confirm.
7. Communication norms (5 min) — async vs sync, response times, channels.

**Output of the call:** notes, recorded if permitted, used as context for Brand Pack Builder.

---

## Day 2-3 — Brand Pack draft

Run `prompts/02-generate-brand-pack.md` in context mode using:

- The discovery questionnaire.
- The kickoff call notes.
- Any provided assets.

Save under `brands/[client-slug]/`.

Mark `[MISSING_INPUT]` for gaps. Send a one-page list of follow-up questions.

---

## Day 4 — Diagnosis

Run `prompts/01-run-brand-diagnosis.md` against the partial Brand Pack. Surface 1-2 critical unlocks.

Send the diagnosis as a PDF or shared doc. Schedule a 30-min review call (Day 5 or 6).

---

## Day 5 — Diagnosis review

Walk the client through the diagnosis. Decide together:

- Which unlocks they accept.
- Which are deferred (post-engagement).
- Which require Brand Pack updates before content can ship.

---

## Day 6 — Strategy draft

Run `prompts/03-generate-content-strategy.md`. Produce 3-5 pillars + 2-4 series + 30-day plan.

Send for review with a 24-hour comment window.

---

## Day 7 — Strategy approval + first carousel kickoff

Apply review feedback. Lock the strategy. Begin first carousel via `prompts/04-generate-carousel.md`.

Onboarding complete. Engagement is now in steady-state.

---

## Onboarding deliverables

By end of week 1, the client has received:

1. A complete Brand Pack (12 files) saved under `brands/[client-slug]/`.
2. A Brand Diagnosis (executive summary + 12 category scores + recommendations).
3. A locked content strategy (pillars + series + 30-day plan).
4. The first carousel draft.

---

## Onboarding red flags

- Client refuses to answer discovery questions → engagement risk; reset expectations.
- Client wants to "skip" Brand Pack → they'll dispute every output. Don't skip.
- Client provides pasted competitor copy as inspiration → set anti-copying expectations.
- Client wants to begin with paid ads / launch posts before authority is built → recommend a different engagement.

---

## Tone of onboarding

Professional, not corporate. Confident in the process, generous with explanations, firm on the steps. The client is paying for *expertise*, not "we'll do whatever you ask."

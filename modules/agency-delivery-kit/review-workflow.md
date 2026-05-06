# Review Workflow

> Internal QA + client review. Two distinct passes; never combined.

---

## Pass 1 — Internal QA (before client sees it)

**Owner:** the reviewer (not the writer).

**Inputs:** the generated output + the active Brand Pack + the brief.

**Process:**

1. Run `prompts/08-critique-output.md` against the output.
2. Capture the verdict: `Ship-ready / Ship with flags / Borderline / Rewrite required`.
3. If verdict is `Rewrite required`:
   - Run `prompts/09-improve-output.md` with the highest-leverage fix.
   - Re-run the critique.
   - Loop max 2 iterations.
4. Confirm:
   - Brand Pack compliance.
   - No invented claims.
   - All `[PROOF_NEEDED]` markers visible.
   - Renderability if applicable.

**Output:** the polished version + a one-paragraph internal note ("here's why this version, here's what got fixed, here's what's flagged").

---

## Pass 2 — Client review

**Owner:** the client (or designated approver).

**Inputs:** the polished output + handoff doc.

**Process:**

1. Send via agreed channel.
2. Set SLA expectation (default 24-48h).
3. Capture feedback in `client-feedback-form.md`.
4. Apply per `revision-rules.md`.

---

## When the writer and reviewer are the same person

If the agency is a one-person operation, the writer can play reviewer — but in a *separate context*:

- Pause for ≥ 30 min between writing and reviewing.
- Or use a different LLM for the critique pass.
- Or use a peer reviewer if available.

Self-approval bias is the #1 cause of weak shipped content.

---

## What the internal reviewer must check

- [ ] Hook stops the scroll.
- [ ] Tension named and resolved.
- [ ] Mechanism slide present.
- [ ] Voice matches Brand Pack.
- [ ] No invented claims.
- [ ] Words-to-avoid respected.
- [ ] CTA is one action.
- [ ] No banned visual modes.
- [ ] No off-limits topics from `constraints.md`.
- [ ] Renderability (Mode 3) — schema-valid, no remote assets, no JS, no fake logos.

If any fail, send back to writer; do not pass to client review.

---

## What the client should review (and what they shouldn't)

**Client reviews for:**
- Voice fit (does it sound like them?).
- Factual accuracy (numbers, customer names, claims).
- Compliance / legal flags.
- Strategic fit ("are we saying this *now*?").

**Client should NOT review for:**
- Hook strength (that's our job — done in internal QA).
- Slide structure (same).
- Visual mode (same — locked at Brand Pack stage).
- Caption length (same).

If the client critiques the hook, push back kindly and ask: "Is the hook *off-brand*, or just unfamiliar to you?" Often it's the latter.

---

## Reviewer red flags

- The reviewer is the writer in the same Claude session → switch contexts.
- The reviewer rubber-stamps every output → recalibrate; the bar is too low.
- The reviewer rewrites uninvited → reviewer is now writer; bring in a third party.
- The client repeatedly overrides Brand Pack → reset expectations or refresh the Brand Pack to match their preference.

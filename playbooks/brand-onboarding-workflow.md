# Playbook — Brand Onboarding Workflow

> Mode 1. From "I want to use PostStudio System for brand X" to "first carousel ready to ship." 1 week.

---

## Day 0 — Prep

- [ ] Clone `poststudio-system/`.
- [ ] Create a Claude Project; upload the repo as Project Knowledge.
- [ ] Paste custom instructions from [`docs/how-to-use-with-claude-projects.md`](../docs/how-to-use-with-claude-projects.md).
- [ ] Pick a brand-slug (lowercase, hyphenated).
- [ ] Gather raw brand context: bio, website copy, decks, recent posts, customer interviews.

---

## Day 1 — Brand Pack draft (Path B: context mode)

Run `prompts/02-generate-brand-pack.md` in context mode:

```
Use prompts/02-generate-brand-pack.md.
Brand slug: [slug]
Pasted context:
  === Bio ===
  [paste]
  === Recent posts ===
  [paste 5-10]
  === Founder essays ===
  [paste 1-3]
  === Customer quotes ===
  [paste 2-3]
  === Compliance / constraints ===
  [paste]
```

Claude returns the 12 Brand Pack files.

Save under `brands/[slug]/`. Re-upload to the Project.

---

## Day 2 — Diagnosis

```
Use prompts/01-run-brand-diagnosis.md against brands/[slug]/.
Focus: all 12 categories.
```

Review the 12-category scores. Identify the 1-2 biggest unlocks.

If 4+ categories scored ≤ 2 → loop back to Day 1; the Brand Pack needs more input.

---

## Day 3 — Diagnosis review (with team / client)

Walk through the diagnosis. Decide:

- Which unlocks to fix this week.
- Which are deferred (post-engagement / next quarter).
- Whether the Brand Pack needs another round before generating content.

Capture decisions in `brands/[slug]/changelog.md` (your private notes).

---

## Day 4 — Apply unlocks (small fixes)

Tactical fixes — typically:

- Bio rewrite.
- Pinned post selection.
- Visual mode selection.
- Typography lock.
- One quick proof asset capture.

Update Brand Pack files as fixes ship.

---

## Day 5 — Content strategy

```
Use prompts/03-generate-content-strategy.md against brands/[slug]/.
Time window: 30 days.
Channel scope: [primary platform].
Cadence: [N posts/week].
```

Returns: 3-5 pillars + 2-4 series + 30-day plan.

Review with team/client. Lock the strategy.

---

## Day 6 — First carousel kickoff

Pick the first post from the 30-day plan. Run [`single-carousel-workflow.md`](single-carousel-workflow.md).

Most brands' first carousel takes longer (~3 hours) because the visual template isn't locked yet. After 3-5 carousels, time drops.

---

## Day 7 — Ship + reflect

- Ship the first carousel.
- Capture proof (Stage 9 of single-carousel workflow).
- Confirm the production loop is repeatable.

---

## End-of-week checklist

- [ ] 12 Brand Pack files complete (or `[MISSING_INPUT]` flagged).
- [ ] Diagnosis run; unlocks decided.
- [ ] Content strategy locked.
- [ ] First carousel published.
- [ ] Capture-and-iterate loop established.
- [ ] Brand-pack refresh date set (90 days out).

---

## Common onboarding red flags

- Client / team can't articulate audience as a role + situation → push back; specify before continuing.
- No proof assets at all → engagement runs in degraded mode (every claim becomes `[PROOF_NEEDED]`); plan customer interviews ASAP.
- Voice contradicts compliance posture → resolve before the second carousel.
- Visual identity unfinished → lock typography + accent + 1 visual mode before the third carousel.

---

## After onboarding

The brand is *production-ready*. Subsequent posts run via [`single-carousel-workflow.md`](single-carousel-workflow.md) or [`monthly-content-engine-workflow.md`](monthly-content-engine-workflow.md).

Quarterly: re-run diagnosis. Refresh Brand Pack.

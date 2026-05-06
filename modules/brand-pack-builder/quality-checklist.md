# Quality Checklist — Brand Pack Builder

> The pack-specific checks. Run before returning to the user.

---

## Critical (any fail → don't ship)

- [ ] All 12 files present.
- [ ] `brand.md` has name, slug, one-liner, category, stage.
- [ ] `audience.md` has a primary segment with role + situation (not "everyone").
- [ ] `offer.md` has the unique mechanism (not just features).
- [ ] `positioning.md` has the one-liner positioning statement.
- [ ] `voice.md` has 5-7 adjectives, banlist, voice example, anti-example.
- [ ] `visual-style.md` has primary visual mode, color palette (with hex), typography.
- [ ] `proof-assets.md` distinguishes citable from non-citable proof.
- [ ] `constraints.md` has claims-allowed and claims-forbidden lists.
- [ ] `content-pillars.md` has at least 3 pillars.
- [ ] `competitors.md` includes the "do nothing" alternative.
- [ ] `examples.md` and `rejected-examples.md` exist (even if mostly `_(to be added)_`).

---

## Quality (each fail → flag, not block)

- [ ] One-liner is plain language (no "innovative platform" energy).
- [ ] Audience pains are quotes from real audience, not paraphrased by the brand.
- [ ] Differentiators are defensible (not "we care about customers").
- [ ] Voice example is brand-aligned and quotable.
- [ ] Visual mode is one of the 12 in `system/visual-framework.md`.
- [ ] Proof assets include `last_reviewed` dates.
- [ ] Constraints include trigger phrases for legal review.
- [ ] Content pillars each have topic clusters and content-type mapping.
- [ ] Competitors include both direct AND adjacent + "do nothing."
- [ ] Examples include at least 1 anchor post (a gold standard the system can reference).
- [ ] Rejected examples include the *why* — not just "this didn't work."

---

## Cross-file consistency check

- [ ] Audience role is the same across `audience.md`, `offer.md`'s positioning statement, and `voice.md`'s calibration.
- [ ] Visual mode in `visual-style.md` matches the references in `examples.md`.
- [ ] Claims-allowed in `constraints.md` are backed by entries in `proof-assets.md`.
- [ ] Words-to-avoid in `voice.md` show up as banned patterns in `rejected-examples.md`.
- [ ] Compliance posture in `constraints.md` doesn't contradict the offer in `offer.md`.

If any inconsistencies found, surface in the output's `warnings[]`.

---

## Common failure modes

- **The pack is "almost there."** 5 of 12 files have `_(to be added)_` for critical fields. The pack is not usable; recommend a follow-up session.
- **Voice is generic.** Adjectives like "friendly, professional, approachable" — useless. Force specificity.
- **No rejected examples.** Means the brand hasn't validated what *not* to do. Acceptable for new brands; flag as future-priority.
- **Proof assets contain invented numbers.** Audit the user's own claims; ask "what's the source?"
- **Brand voice contradicts the audience.** E.g. casual/playful voice for a formal compliance audience. Flag explicitly.

---

## Approval

A Brand Pack passes when:

- All 12 critical items pass.
- ≥ 8 of 11 quality items pass.
- No cross-file consistency issues unresolved.

If it fails, the Brand Pack Builder should not return a "complete pack." Return the partial pack with explicit gaps and a follow-up plan.

# Quality Checklist — Ship / No-Ship Gate

> Every carousel ends with a self-review. Not as ceremony, but as the actual decision: ship this or rewrite it.

This file defines the **30-point review** used by the writer before delivering, by the reviewer prompt (`prompts/critique-carousel.md`) when grading, and by the user when deciding to publish.

A carousel that fails any of the **10 critical items** does not ship.
A carousel that fails any **non-critical item** ships only with the failure flagged.

---

## How to use this checklist

After you finish writing a carousel, run through every item and mark `pass / fail / risk`:

- **pass** — the carousel clearly meets the bar.
- **fail** — the carousel does not meet the bar; rewrite required.
- **risk** — borderline; ship only with explicit acceptance from the user.

Surface every `fail` and `risk` in the output's `Quality Checklist` and `Missing Inputs / Proof Needed` sections.

If you wrote the carousel, **you do not approve it**. Use the `critique-carousel.md` prompt as a separate reviewer pass for high-stakes posts.

---

## The 10 critical items (any fail = no ship)

### 1. Slide 1 stops the scroll
Read slide 1 alone, with no context. Would a tired person on Sunday night stop?
**Pass:** specific subject + tension + implied payoff in ≤14 words.
**Fail:** vague, generic, or "thread starter" cadence.

### 2. The hook earns the swipe
Is there a real reason to see slide 2?
**Pass:** the reader has a specific question slide 2 will answer.
**Fail:** slide 2 just continues the same thought.

### 3. One idea per slide
Could the slide's idea be written in 4 words?
**Pass:** every slide has a single, namable idea.
**Fail:** any slide has two unrelated ideas or no idea.

### 4. Tension is named and resolved
Where's the fault line? Where does the carousel release the pressure?
**Pass:** there is a clear tension and a clear release (typically slides 4-5).
**Fail:** the carousel reads as a list with no narrative arc.

### 5. Mechanism is shown, not asserted
Does the carousel explain *why* the new approach works?
**Pass:** at least one slide explains the underlying mechanism.
**Fail:** the carousel gives the conclusion without showing the reasoning.

### 6. Voice matches the Brand Pack
Re-read the caption against `brands/[slug]/voice.md`.
**Pass:** voice, vocabulary, and register all match.
**Fail:** the post sounds like a different brand.

### 7. No invented claims
Are all numbers, customers, awards, and quotes in the user's brief or Brand Pack?
**Pass:** every specific claim is sourced. Anything not sourced is `[PROOF_NEEDED]`.
**Fail:** any specific claim is fabricated.

### 8. Words-to-avoid list is respected
Search the carousel for every word in `brands/[slug]/voice.md`'s "words to avoid" list.
**Pass:** no violations.
**Fail:** any forbidden word appears.

### 9. CTA is one action, not two
The carousel's last slide and the caption's closing line both point at one ask.
**Pass:** one clear, specific action.
**Fail:** "Save and share and follow and DM!"

### 10. The carousel could pass a `critique-carousel.md` review
If you ran the reviewer prompt right now, would it find a deal-breaker?
**Pass:** confident yes.
**Fail:** there's a soft spot you'd want to hide from the reviewer.

---

## The 20 quality items (each fail = flag, not block)

### Hook & opening

11. **The hook is specific.** Generic hooks ("3 lessons from building a startup") fail this.
12. **The hook isn't a stale pattern** ("Are you tired of...?" / "Did you know...?" / "Hot take:").
13. **The first 14 words tell the reader who the post is for.** Audience is implied or explicit.

### Slide structure & pacing

14. **The carousel has 7-10 slides.** Default to 8.
15. **The slide count fits the chosen structure.** (See `carousel-framework.md`.)
16. **Pacing alternates pinch and release.** No two low-energy slides back-to-back.
17. **Each slide answers the question raised by the previous slide.**
18. **Visual metaphor is present.** At least 1-2 slides use visual metaphor, not just text on a colored rectangle.

### Copy & language

19. **No marketing jargon.** No "seamless," "robust," "supercharge," "unleash," "transform," "synergy."
20. **No filler intensifiers.** No "absolutely," "literally," "100%," "truly."
21. **Headlines are 6-12 words.** Anything longer becomes blog copy.
22. **Active voice on slides.** Passive voice OK in the caption sparingly.
23. **Numbers are concrete or marked `[PROOF_NEEDED]`.**

### Visual & layout

24. **One visual mode** is used consistently across all slides.
25. **One accent color** is used (no two-accent designs).
26. **Headline lives in the bottom third** of slide 1 (or wherever the visual mode requires consistency).
27. **Slide margins are consistent** across all slides.
28. **Image prompts include "no text, no logos"** and reserve space for typography.

### Caption & CTA

29. **Caption ends with a comment-worthy prompt.** No "Thoughts?" or "Hope this helps!"
30. **First-comment is value-stacked**, not a copy of the caption.

---

## Red flags (immediate rewrite)

Any of these = stop and rewrite:

- 🚩 The hook starts with "Are you tired of..." or "Did you know..."
- 🚩 The carousel is mostly text-on-color slides with no visual metaphor.
- 🚩 A specific number appears that wasn't in the brief.
- 🚩 The CTA is "Hope this helps!" or "Let me know your thoughts!"
- 🚩 The post sounds like a corporate blog post.
- 🚩 The carousel describes a feature without grounding it in a job to be done.
- 🚩 Two slides argue the same point with different words.
- 🚩 The caption is the slides typed out.
- 🚩 Hashtag soup at the end of the caption.
- 🚩 Emoji in every line.

---

## Yellow flags (proceed with caution)

These don't kill the post, but each one weakens it:

- 🟡 The hook is a *category* hook ("X founders should...") instead of a *role + situation* hook ("X founders raising a Series A...").
- 🟡 The proof slide leans on a single anecdote without naming who it's from.
- 🟡 The carousel has 11+ slides.
- 🟡 The accent color is used on every slide instead of selectively.
- 🟡 The brand mark is visible on every slide instead of just slide 1 and the last.
- 🟡 The first comment exceeds 100 words.
- 🟡 The caption opens with the same line as slide 1 word-for-word.

---

## Score format for the writer's self-review

At the end of every carousel output, return a checklist block like this:

```
# Quality Checklist

Critical (must pass)
- [x] Slide 1 stops the scroll
- [x] Hook earns the swipe
- [x] One idea per slide
- [x] Tension named and resolved
- [x] Mechanism shown, not asserted
- [x] Voice matches Brand Pack
- [x] No invented claims
- [x] Words-to-avoid respected
- [x] CTA is one action
- [x] Could pass an external reviewer

Quality (each fail = flag, not block)
- [x] Hook is specific
- [x] No stale patterns
- [x] First 14 words name audience
- [x] 7-10 slides
- [x] Slide count fits structure
- [x] Pacing alternates pinch/release
- [x] Each slide answers previous
- [x] Visual metaphor present
- [x] No marketing jargon
- [x] No filler intensifiers
- [x] Headlines 6-12 words
- [x] Active voice
- [ ] Numbers concrete or [PROOF_NEEDED]   ← FLAG: Slide 6 needs numbers
- [x] One visual mode used
- [x] One accent color
- [x] Headline in bottom third
- [x] Margins consistent
- [x] Image prompts say "no text, no logos"
- [x] Caption has comment prompt
- [x] First comment is value-stacked

Score: 29/30 — Ship-ready with 1 flag for proof in slide 6.
```

---

## How the reviewer pass differs from the writer's self-review

The writer's self-review is **honest but generous**. The reviewer pass (using `critique-carousel.md`) is **adversarial**.

In reviewer mode, you should:

- Look for the *single highest-leverage fix*, not nitpicks.
- Propose 2-3 concrete rewrites for the weakest slide.
- Score honestly even if the writer was you.
- Flag any safety or claims violations the writer missed.
- If two slides could be combined, say so.
- If the post would underperform, say so.

The two passes are intentionally separated. Don't approve your own work in the same context where you wrote it.

---

## Final ship test

If the carousel passes the 10 critical items and scores 25+/30 on the full checklist, ship it.
If it fails any critical item or scores below 22/30, rewrite.
If it falls in 22-24/30, surface the flags and let the user decide.

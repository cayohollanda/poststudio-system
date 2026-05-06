# Reels / Short-Video Framework

> Rules and structures for short-form video scripts (Reels, TikTok, Shorts, X video). Output-shape only — actual production happens off-platform.

PostStudio System produces video **scripts** with: hook, beats, on-screen text, B-roll directions, captions, CTAs. We do not edit video. We do not generate video. We hand a director a brief.

---

## The 4 script types

| Type | Best for | Default duration |
|---|---|---|
| **Talking-head** | founder POV, opinions, reactions | 30-60s |
| **Faceless** | tutorials, frameworks, "how X works" | 30-90s |
| **B-roll-led** | product demos, before/after, transformations | 15-30s |
| **Hybrid** | founder + B-roll cuts | 45-60s |

The Brand Pack's `visual-style.md` and `constraints.md` may forbid faces (e.g. Lumen) — in that case, default to faceless or B-roll-led.

---

## The 5 durations

| Duration | Words target | Use case |
|---|---|---|
| **15s** | 30-40 words | strong hook + single beat + CTA — for high-frequency content |
| **30s** | 65-80 words | hook + 2-3 beats + CTA — the workhorse |
| **45s** | 95-115 words | hook + 3-4 beats + payoff + CTA — for tutorials |
| **60s** | 130-160 words | full narrative arc — for founder essays |
| **90s** | 200-240 words | rare; only when the audience is patient |

---

## Universal script structure

Every script has these beats, regardless of duration:

1. **Hook (0-2s)** — pattern interrupt + promise.
2. **Set-up (2-5s)** — the tension or premise.
3. **Body** — the argument, the steps, the demo, the story.
4. **Payoff** — the resolution or "aha."
5. **CTA (last 2-3s)** — one specific action.

For 15s scripts, beats 2-4 collapse into one moment.

---

## Hook patterns for short video

Short video is more brutal than carousel for hook physics. The first 2 seconds decide everything.

### Patterns that work
- **Visual surprise** — start mid-action, mid-transformation, mid-reveal.
- **Counter-claim** — "Most [audience] are doing this wrong."
- **Confession** — "I lost [time] / [money] before I figured this out."
- **Threshold** — "If you're still [doing X] in [year], stop."
- **Number-led** — "I tested [N]. Here's what shipped."
- **Demo-led** — show the result first, explain how second.

### Patterns that fail
- "Hey guys" / "What's up everyone" — instant scroll.
- "Today I want to talk about..." — burns 3 seconds.
- Logo intro card — scroll.
- Music-only intro — scroll.
- Slow zoom into a face — scroll.

---

## On-screen text rules

- Open with the hook on-screen (sound-off audiences are 80%).
- One idea per text block.
- Text on-screen ≥ 1 second per word as a minimum legibility baseline.
- Position text in the safe zone (avoid top 10%, bottom 15% — UI overlays cover those).
- Use a typeface from the Brand Pack. Captions inherit the brand voice.

---

## B-roll directions

When the script calls for cuts:

- Specify the *purpose* of each cut, not just "B-roll of laptop."
- One concrete, observable detail per cut.
- Reusable categories: hands working; product close-up; environment; before/after split; reference object metaphor.
- Don't direct AI-generated B-roll unless the Brand Pack allows it.

Example:
```
B-roll Cut 1 (3s): tight on a paper calendar page being torn — symbol of a meeting cancelled.
B-roll Cut 2 (2s): wide of an empty desk at golden hour — protected morning recovered.
B-roll Cut 3 (3s): hands closing a laptop deliberately — end of work, deep work over.
```

---

## CTA patterns for short video

CTAs in short video are more direct than carousel:

- **Save** — "Save this for the next [moment]."
- **Comment** — "Comment '[KEYWORD]' for the [bonus]."
- **Follow** — "Follow for more [topic] without the [pain]."
- **DM** — "DM me '[KEYWORD]' for the [resource]."
- **Try** — "Try it free at [URL] — link in bio."
- **Watch next** — "Part 2 drops [day]. Follow so you don't miss it."

Avoid:
- "Like and subscribe" energy from YouTube — Reels/TikTok punish this.
- Generic "What do you think?"
- Multi-action stacks.

---

## Caption rules (the post body, not on-screen text)

Short video captions are different from carousel captions:

- **Length:** 80-140 characters (TikTok), 100-200 (Reels), 200-400 (longer-form Reels essays).
- **Hook the first line.**
- **One CTA at the end.**
- **Hashtags:** 3-5 maximum, brand-pack-approved only. No hashtag soup.

---

## Output structure

Every Reels script returns:

```
# Strategic Angle
[1-2 sentences]

# Script
- Type: [talking-head / faceless / b-roll-led / hybrid]
- Duration: [15s / 30s / 45s / 60s / 90s]
- Word count: [target]

## Beat 1 — Hook (0-2s)
- Voice / on-screen: ...
- Visual / B-roll: ...
- Audio note: ...

## Beat 2 — Set-up (2-5s)
[same structure]

## Beat N — CTA (last 2-3s)
[same structure]

# On-screen text (timed)
[block by timestamp]

# B-roll list
[numbered, with purpose per cut]

# Caption
[80-200 words for the post body]

# CTA
[one line]

# Visual style
[reference Brand Pack visual-style.md]

# Editing notes
[any non-obvious post-production note]

# Quality checklist
[scorecard]

# Missing Inputs / Proof Needed
[gaps]
```

See `schemas/reels-script.schema.json` for the JSON form.

---

## Quality checklist (short-video specific)

- [ ] First 2 seconds carry a hook (visual + auditory + on-screen).
- [ ] Sound-off legible (on-screen text covers the message).
- [ ] One idea per beat.
- [ ] Word count fits duration.
- [ ] CTA is one specific action.
- [ ] Caption hooks line 1.
- [ ] B-roll directions are concrete (a director can follow them).
- [ ] No banned vocabulary from Brand Pack.
- [ ] Voice matches Brand Pack.
- [ ] No invented claims; `[PROOF_NEEDED]` where missing.

---

## Common failures

- **Hook lives in beat 2.** The "real opening" comes after a "today we're going to talk about" intro — kill the intro.
- **Walls of on-screen text.** Each block must finish before the next appears; cap at ~6 words per block.
- **B-roll mismatch.** "Hands working at laptop" appears 4 times in a 30s script. Vary cuts; specify purpose.
- **Multi-CTA confusion.** "Save and follow and DM and comment" → pick one.
- **Caption-as-script.** The caption isn't a transcript; it's a separate beat that adds context.

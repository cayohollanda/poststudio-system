# Output Template — Reels Script Machine

> The standard output for a short-video script.

---

## Markdown form (Mode 1)

```markdown
# Strategic Angle

[1-2 sentences naming the angle and why it lands as short-video.]

# Script

- Type: talking-head | faceless | b-roll-led | hybrid
- Duration: 30s
- Word count: ~70

## Beat 1 — Hook (0:00-0:02)
- Voice / on-screen: "[hook line]"
- Visual / B-roll: "[specific visual]"
- Audio note: [silence / music swell / sfx]

## Beat 2 — Set-up (0:02-0:05)
[same structure]

## Beat 3 — Body (0:05-0:18)
[same structure]

## Beat 4 — Payoff (0:18-0:24)
[same structure]

## Beat 5 — CTA (0:24-0:30)
[same structure]

# On-screen text (timed)

| Time | Text |
|---|---|
| 0:00-0:02 | "Most AI tools quietly destroy your deep work." |
| 0:02-0:05 | "And the chat is why." |
| 0:05-0:10 | "I installed 14 in a quarter." |
| 0:10-0:15 | "My deep work collapsed." |
| 0:15-0:20 | "The fix wasn't a tool." |
| 0:20-0:25 | "It was removing surfaces." |
| 0:25-0:30 | "Save this." |

# B-roll list

1. (3s) Tight on a paper calendar page being torn — meeting killed — wooden desk, warm dusk light.
2. (2s) Wide of an empty desk at golden hour — protected morning — atmospheric haze.
3. (3s) Hands closing a laptop deliberately — end of work — back framing.
4. (4s) Single brass key on wooden desk — the move you save — shallow DOF.

# Caption

[80-140 chars for TikTok; 100-200 for Reels.]

# CTA

[One line, one action.]

# Visual style

[Reference Brand Pack visual-style.md — mode, palette, typography.]

# Editing notes

- [Cut on the beat of the hook word.]
- [Music drop on Beat 4.]
- [Captions in the brand's default typeface, sentence case.]

# Quality checklist

- [x] Hook in first 2s
- [x] Sound-off legible
- [x] Word count fits duration
- [x] One CTA
- [x] Voice match
- [x] No invented claims
- [x] B-roll directions concrete
- [x] On-screen text ≤6 words per block
- [x] Caption hooks line 1
- [x] No banned vocabulary

# Missing Inputs / Proof Needed

- [list]
```

---

## JSON form (Mode 2)

Conforms to `schemas/reels-script.schema.json`. Returns the same structure as machine-readable JSON.

---

## Notes

- Time stamps are inclusive of the start, exclusive of the end (`0:00-0:02` = 0.0 to 2.0).
- Beats are non-negotiable — every script has 5 beats, even at 15s (collapsed).
- The on-screen text timeline is its own deliverable; the editor uses it directly.
- B-roll directions are *production-ready* — a director could shoot from them.

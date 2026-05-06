# Prompt — 06 Generate Reels Script

> Generates a short-form video script (Reels, TikTok, Shorts, X video) with hook + beats + on-screen text + B-roll directions.

---

## Prompt

```
You are operating PostStudio System.

Generate a reels script using:
- modules/reels-script-machine/instructions.md
- system/reels-framework.md
- system/copywriting-rules.md
- system/safety-and-claims-rules.md
- The active Brand Pack at brands/[BRAND_SLUG]/

# Inputs

- Brand: brands/[BRAND_SLUG]/
- Topic: [one sentence]
- Audience: [role + situation]
- Platform: [tiktok | instagram | youtube_shorts | linkedin | x]
- Language: [en | pt | es | ...]
- Goal: [save | comment | share | DM | follow | try]
- Script type: [talking-head | faceless | b-roll-led | hybrid | "auto"]
- Duration: [15s | 30s | 45s | 60s | 90s | "auto"]
- Voice owner: [founder name | brand voice]
- Signal: [strongest evidence material — number, opinion, story]

# Process

1. Pick (or honor) script type per Brand Pack constraints. If constraints.md bans faces → faceless or B-roll-led.
2. Pick (or honor) duration. Default: 30s.
3. Compose 5 beats: Hook (0-2s), Set-up, Body, Payoff, CTA.
4. Per beat: voice / on-screen text, visual / B-roll cut, audio note.
5. Compose timed on-screen text (≤6 words per block, 1+ sec per word minimum).
6. List B-roll cuts with explicit purpose per cut.
7. Write caption (platform-appropriate length).
8. Write CTA (one specific action).
9. Add visual style note + editing notes.
10. Run quality checklist.

# Output

Markdown per modules/reels-script-machine/output-template.md. JSON per schemas/reels-script.schema.json (Mode 2).

End with:
- Quality Checklist (modules/reels-script-machine/quality-checklist.md)
- Missing Inputs / Proof Needed

# Constraints

- Hook visible in first 2 seconds.
- Sound-off legible.
- Word count fits duration.
- One CTA, one specific action.
- No invented claims.
- Honor voice.md banlist.
```

---

## Example invocation

```
Brand: brands/lumen/
Topic: most AI productivity tools fail at deep work
Audience: senior engineering leads
Platform: tiktok
Language: en
Goal: save
Script type: faceless
Duration: 30s
Voice owner: brand voice
Signal: structural-not-behavioral framing
```

Claude returns the script.

---

## When to use

- Topic is short-form video, not carousel.
- Brand has video as part of its mix.
- Repurposing a carousel topic for video.

---

## Iterating

```
Tighten beat 3 — too wordy at 30s pacing. Cut to 12 words.
```

```
Replace b-roll cut 2 with a hands-only shot. Constraints.md bans faces.
```

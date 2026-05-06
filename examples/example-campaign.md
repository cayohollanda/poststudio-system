# Example Campaign — `lumen` (fictional)

> Worked example. 14-day product launch for Lumen v3.4 Focus Mode.

## Campaign

- Type: product-launch
- Duration: 14 days
- Objective: drive 50 trial signups for v3.4 Focus Mode within launch week
- Target audience: senior PMs and engineering leads at 50-500 person SaaS
- Core message: the morning-brief pattern, now built into Lumen
- Primary CTA: try
- Secondary CTAs: save (early posts), DM (proof posts)

## Audience journey

- **Day-0 mental state:** "I've installed too many AI tools. None of them give me my morning back."
- **Day-N mental state:** "Lumen is the structural fix. I should try it."
- **Friction:** (1) skepticism after over-installation last quarter; (2) uncertainty about cost vs benefit.

## Post sequence

### Day 1 — Setup: name the pain
- Format: carousel (8)
- Angle: Mistake (Mira)
- Hook: I installed 14 AI tools last quarter. My deep work collapsed.
- Body summary: confessional opener; structural-not-behavioral thesis; no launch mention.
- CTA: save
- Platform: LinkedIn
- Proof needed: none (uses Anecdote 1 from proof-assets.md)

### Day 2 — Setup: audience callout
- Format: reel 30s
- Angle: Audience callout + Mechanism
- Hook: Senior operators don't have more focus. They have fewer surfaces.
- Body summary: 30s talking-head from Mira; explicit audience naming.
- CTA: follow
- Platform: LinkedIn + IG
- Proof needed: none

### Day 3 — Setup: surface the default
- Format: carousel (7)
- Angle: Myth vs Reality
- Hook: The 5 AI productivity myths killing your week.
- CTA: comment
- Platform: LinkedIn
- Proof needed: none

### Day 4 — Pressure: visible cost
- Format: carousel (8)
- Angle: Visible cost
- Hook: What 14 AI tools actually cost your week.
- CTA: save
- Platform: LinkedIn
- Proof needed: none (qualitative)

### Day 5 — Pressure: founder essay
- Format: long-form post (text + 1 image)
- Angle: Founder Perspective
- Hook: Mira's voice essay on structural-not-behavioral thesis.
- CTA: follow
- Platform: LinkedIn
- Proof needed: none

### Day 6 — Pressure: contrarian cost
- Format: reel 30s
- Angle: Contrarian
- Hook: Stop installing AI tools that interrupt you.
- CTA: save
- Platform: TikTok + IG
- Proof needed: none

### Day 7 — Reframe: the mechanism (no product mention yet)
- Format: carousel (8)
- Angle: Reframe + Mechanism
- Hook: The replacement: one read at 8am. One read at 5pm.
- Body summary: argues for the morning-brief pattern. Doesn't name Lumen yet.
- CTA: comment
- Platform: LinkedIn
- Proof needed: none

### Day 8 — Reframe: workflow reveal
- Format: carousel (8)
- Angle: Workflow Reveal
- Hook: How operators run their week without AI chat tools.
- CTA: save
- Platform: LinkedIn + IG
- Proof needed: none

### Day 9 — Reframe: technical breakdown
- Format: carousel (9)
- Angle: Technical Breakdown (Theo)
- Hook: Why the chat-shaped UI is the wrong shape for focus.
- CTA: save / DM
- Platform: LinkedIn
- Proof needed: none

### Day 10 — Proof: anonymized case
- Format: carousel (9)
- Angle: Before/After
- Hook: VP of Engineering at a Series-C devtools company: how the brief changed her week.
- Body summary: anonymized case from `proof-assets.md` Story 1 + verbatim quote.
- CTA: DM
- Platform: LinkedIn
- Proof needed: confirm Customer A's permission (already in pack as anonymized).

### Day 11 — Proof: data drop
- Format: carousel (9)
- Angle: Benchmark
- Hook: What 1,200+ operators say works (and doesn't) in their morning.
- Body summary: NPS 62 (Q1 2026, n=312), median 3 uninterrupted morning blocks (self-reported, n=147).
- CTA: click (full data)
- Platform: LinkedIn
- Proof needed: methodology document attached to first comment.

### Day 12 — Proof: founder receipt
- Format: reel 45s
- Angle: Build in Public
- Hook: Theo's engineering essay on the no-chat decision.
- CTA: comment
- Platform: LinkedIn + X
- Proof needed: Anecdote 2 from proof-assets.md.

### Day 13 — Ask: launch
- Format: carousel (10)
- Angle: Launch + Product-Led
- Hook: Today we open Lumen v3.4 Focus Mode.
- Body summary: what it is, who it's for, who it's not, what it costs, what's the trade-off.
- CTA: try (link in bio)
- Platform: LinkedIn + IG + X
- Proof needed: confirmed launch date and pricing.

### Day 14 — Ask: objection-handling
- Format: carousel (8)
- Angle: Objection Handling
- Hook: Three things people ask before trying Lumen.
- Body summary: top 3 objections from launch-day comments answered honestly.
- CTA: try
- Platform: LinkedIn + IG
- Proof needed: live comments from Day 13 (captured in real-time).

## Campaign assets needed

- Mira available for posts on Days 1, 2, 5, 12 (4 founder appearances).
- Theo available for posts on Days 9, 12 (2 engineering appearances).
- Anonymized customer permission for Day 10 case.
- Methodology document for Day 11 data drop (already drafted internally).
- Launch landing page live by Day 13.
- Pricing page tested by Day 12.

## Distribution

- LinkedIn primary; carousels + essays.
- Instagram secondary; same carousels with stronger first-line caption hook.
- TikTok 1-2 reels.
- X for Theo's engineering essays as threads.
- First-comment strategy: each post's first comment links to the post 1-2 days back (compounding).

## Risks

- Audience launch fatigue if other industry launches land same week. Mitigation: scan calendar by Day 7.
- Customer permission for Day 10 case at risk; have backup (Story 2) ready.
- If Day 13 launch coincides with major news cycle, defer 1-2 days.

## Quality Checklist

- [x] Single named primary CTA (try).
- [x] Each post has a distinct role in the 5-act arc.
- [x] No two posts argue the same point with different words.
- [x] Proof posts cite real proof (Days 10-12).
- [x] Pacing alternates argument and proof.
- [x] Day 13 (Launch) is preceded by Day 12 founder receipt and followed by Day 14 objection-handling.
- [x] Voice consistent across the campaign.
- [x] No invented claims anywhere.
- [x] Each post can stand alone.
- [x] Campaign assets confirmed.

## Missing Inputs / Proof Needed

- Day 10: customer permission to be quoted (in flight; expected by Day 7).
- Day 13: launch landing page final URL (placeholder for now; locked by Day 12).

## Recommended next prompt

[`prompts/04-generate-carousel.md`](../prompts/04-generate-carousel.md) — start with Day 1's carousel; the post is already specced (uses the existing Anchor 1 carousel from `examples/example-carousel-output.md`).

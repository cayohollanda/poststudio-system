# Campaign Framework

> Multi-post sequences with a shared objective, narrative arc, and CTA path. Used when a single post is the wrong unit.

A campaign is more than a content calendar. It's an *argument made over time*. Each post advances the argument; the last post asks the audience to act.

---

## When to run a campaign vs. a single post

Run a campaign when:

- **Launch.** A product, feature, brand, or open-source release.
- **Authority push.** A focused 7-30 days of expertise on one topic.
- **Proof drop.** A series of case studies, benchmarks, or "we shipped" updates.
- **Comparison battle.** A measured argument vs the audience's current default.
- **Waitlist build.** 14-30 days of anticipation before a launch.
- **Objection handling.** A series addressing the top 3-5 reasons people don't buy.
- **Re-engagement.** Reactivating an audience that's gone quiet.

Don't run a campaign when:
- A single post would carry the argument faster.
- You don't have proof for the middle posts.
- You can't commit to the cadence.

---

## The 15 campaign types

Each has a distinctive arc and outcome.

| # | Campaign | Default duration | Anchor goal |
|---|---|---|---|
| 1 | Product launch | 14 days | Try / DM |
| 2 | Feature launch | 7 days | Try / Click |
| 3 | Brand announcement | 7 days | Awareness / Save |
| 4 | Authority building | 30 days | Follow / Save |
| 5 | Proof / case study | 7 days | Save / DM |
| 6 | Comparison | 7 days | Save / Click |
| 7 | Education series | 30 days | Save / Subscribe |
| 8 | Waitlist | 14-30 days | DM / Click |
| 9 | Offer validation | 14 days | DM / Reply |
| 10 | Open-source / project announcement | 7 days | Star / Click |
| 11 | Event / webinar / workshop | 14 days | Click / RSVP |
| 12 | Founder story | 14 days | Follow / Save |
| 13 | Problem awareness | 14 days | Save / Comment |
| 14 | Objection handling | 14 days | DM / Try |
| 15 | Re-engagement | 7 days | Comment / Save |

---

## Universal campaign structure

Every campaign has 5 acts:

1. **Setup** — name the tension, the topic, the audience.
2. **Pressure** — make the cost of the status quo visible.
3. **Reframe** — introduce the new mental model.
4. **Proof** — evidence, examples, demonstrations.
5. **Ask** — the call to act.

Spread these across the campaign's duration. A 7-day campaign uses 1-2 posts per act. A 30-day campaign uses 4-8.

---

## 7-day template (launch / proof / comparison)

```
Day 1 — Hook post: name the launch / proof / comparison.
Day 2 — Why now: the shift in the world that makes this needed.
Day 3 — The default and its cost.
Day 4 — The reveal / mechanism.
Day 5 — Proof slide: case, demo, benchmark.
Day 6 — Use case: "this is for [SEGMENT] when [SITUATION]."
Day 7 — CTA post: clear action, deadline if applicable.
```

---

## 14-day template (waitlist / authority / objection)

```
Days 1-3 — Setup arc: 3 posts naming the problem and the audience.
Days 4-6 — Pressure arc: 3 posts on the cost of the default.
Days 7-9 — Reframe arc: 3 posts introducing the new mental model.
Days 10-12 — Proof arc: 3 posts of evidence (each a different proof type).
Days 13-14 — Ask arc: 2 posts driving the CTA, second post addresses the top objection.
```

---

## 30-day template (authority building / education series)

```
Week 1 — Foundation: pillar posts naming the brand's core arguments (4-5 posts).
Week 2 — Mechanism: posts showing *why* the arguments are true (4-5 posts).
Week 3 — Application: posts on how to apply the arguments on Monday (4-5 posts).
Week 4 — Proof + ask: case studies, founder essays, then 1-2 CTA posts (4-5 posts).
```

---

## Campaign output structure

A generated campaign returns:

```
# Campaign
- Type: [type from the 15]
- Duration: [7 / 14 / 30 days]
- Objective: [one sentence]
- Target audience: [role + situation]
- Core message: [one sentence]
- Primary CTA: [the action the campaign drives toward]
- Secondary CTAs: [optional supporting CTAs]

# Audience journey
- Day 0 mental state: [what they believe entering]
- Day N mental state: [what they should believe by the end]
- Friction to overcome: [the 1-2 mental blocks]

# Post sequence

## Day 1 — [post role in arc]
- Format: [carousel / single / reel / thread]
- Angle: [from system/content-system.md]
- Hook: [one line]
- Body summary: [2-3 sentences]
- CTA: [the post's CTA]
- Platform priority: [linkedin / instagram / tiktok / x / multi]
- Proof needed: [list]

[... continue for each day ...]

# Campaign assets needed
[list of inputs the user must supply: stories, screenshots, numbers]

# Distribution
[how to amplify each post — first comments, cross-posts, repurposing]

# Risks
[list of campaign-level risks: timing, proof gaps, audience fatigue]

# Quality Checklist
[campaign-level checklist]

# Missing Inputs / Proof Needed
[gaps]
```

See `schemas/campaign.schema.json` for JSON.

---

## Campaign-level quality checklist

- [ ] Single, named primary CTA.
- [ ] Each post has a distinct role in the 5-act arc.
- [ ] No two posts argue the same point with different words.
- [ ] Proof posts are specific, not "we work hard."
- [ ] Pacing alternates argument and proof — no 5-post stretches of the same beat.
- [ ] Final post drives the CTA; the post before it pre-empts the top objection.
- [ ] Every brand voice rule respected across the campaign.
- [ ] No invented numbers, customers, awards, quotes anywhere.
- [ ] Each post can stand alone (audience doesn't need to have seen the previous one to swipe).
- [ ] Campaign assets (stories, screenshots, numbers) listed up-front, not assumed.

---

## Campaign anti-patterns

- **The slow build to nothing.** 13 posts of "we have something coming" → audience checks out by post 3.
- **The same hook, six times.** Different angle every post; if it sounds the same, it gets ignored.
- **CTA in every post.** The audience tunes out. Save the explicit CTA for acts 4-5.
- **Misaligned platforms.** Pushing the same launch sequence on LinkedIn and TikTok unchanged. Adapt format and hook per platform.
- **No proof in the middle.** Setup → reframe → ask, with no evidence in between, reads as marketing.
- **Founder absence.** A 30-day authority campaign with no founder voice at any point.
- **The "we" parade.** Every post is "we built / we shipped / we believe." Vary perspective across the campaign.

---

## Campaign cadence rules

- **Don't post twice the same day** unless the platform supports it (e.g. X). One per channel per day max.
- **Skip days the audience doesn't read.** Audit the Brand Pack's `audience.md` for posting-time signals.
- **Leave whitespace.** A 30-day campaign with 30 posts is too dense. Aim for 18-22 posts.
- **Front-load and back-load.** First 3 days and last 3 days are the most important; mid-campaign can have lighter posts.

---

## Cross-platform adaptation

A campaign post often needs to be adapted, not duplicated, per platform:

| Platform | Adaptation |
|---|---|
| LinkedIn | full carousel + 200-word caption |
| Instagram | same carousel, 100-word caption, stronger first line |
| TikTok | 30s reels script with the same hook, faster pace |
| X | thread with same argument, 5-7 tweets |
| Threads | shorter caption, no carousel; lean into the conversation prompt |

The campaign output should specify the *primary platform* per post, with optional adaptation notes.

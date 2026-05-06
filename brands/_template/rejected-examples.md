# Rejected Examples — `[brand-slug]`

> Patterns that don't work for this brand. Captured here so the system avoids repeating them.

This file is a *banlist with reasons*. Different from `voice.md`'s banlist (which is word-level). This file is *pattern-level*.

---

## Banned patterns

### Pattern: [name]
- **Description:** [1-2 sentences naming the pattern]
- **Example:** [paste a real attempt that didn't work]
- **Why it failed:** [specific reason — audience reaction, voice mismatch, off-brand, etc.]
- **What to do instead:** [the alternative pattern that works for this brand]
- **Date banned:** [YYYY-MM-DD]
- **Banned by:** [whose decision]

### Pattern: [name]
[same structure]

### Pattern: [name]
[same structure]

---

## Common categories of banned patterns

### Hook patterns
- [Pattern 1 — e.g. "Listicle hook ('5 ways to...')" — banned because audience equates with low-effort content]
- [Pattern 2]

### Visual patterns
- [Pattern 1 — e.g. "Bright daytime imagery" — off-brand for nocturnal voice]
- [Pattern 2]

### Caption patterns
- [Pattern 1 — e.g. "Question-only caption" — performed worse than the standard restate-and-prompt format]
- [Pattern 2]

### CTA patterns
- [Pattern 1 — e.g. "Drop a 🔥 if you agree" — feels low-effort; reduced save rate]
- [Pattern 2]

### Topic / angle patterns
- [Pattern 1 — e.g. "Comparison post naming a competitor" — audience reads as adversarial; compliance flag]
- [Pattern 2]

---

## Patterns we tried that almost worked

> Patterns that flopped but we *suspect* could work with adjustment. Document so we don't fully kill them.

### Almost-pattern 1
- **What we tried:** [...]
- **Why it didn't quite land:** [...]
- **What might fix it:** [...]
- **Status:** [parked — revisit if [TRIGGER]]

---

## Banned for compliance reasons

> Distinct from "tried and failed." These patterns are banned because they violate `constraints.md` or trigger legal review.

- [Pattern — e.g. "Specific health-outcome claim" — only allowed with disclaimer]
- [Pattern — e.g. "Comparison vs Reclaim by name" — competitor handling rule]
- [Pattern — e.g. "FDA-approved" mention — we are not FDA-approved]

---

## Refresh cadence

- Quarterly review of all banned patterns. Some may be unbanned if circumstances changed.
- Add new bans whenever a deliverable underperforms in a way the team agrees on.
- Cross-reference monthly reports for new candidates.

---

## How the system uses this file

When generating any output, Claude:

1. Loads this file alongside `voice.md` and `constraints.md`.
2. Treats banned patterns as hard constraints.
3. Surfaces flagged conflicts in `Missing Inputs / Proof Needed` if a brief tempts a banned pattern.

This file compounds. The longer the brand operates, the sharper this file becomes — and the better future content gets.

---

## Refresh log

| Date | What changed |
|---|---|
| [YYYY-MM-DD] | [ban added / removed] |

---

> When this file is filled in, the system avoids repeating known failures and the team's hard-won lessons compound forever.

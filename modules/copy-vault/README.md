# Module — Copy Vault

> Reusable copy patterns for hooks, headlines, CTAs, captions, arguments, objections, transitions, proof copy, launch copy, contrarian copy. Placeholder-driven, brand-agnostic.

---

## What it is

A catalog of frameworks. Not a library of pre-written posts. The Vault gives Claude (and the user) reusable *shapes* — `[BRAND]`, `[AUDIENCE]`, `[PROBLEM]`, etc. — to fill in per brand and per post.

## What it is NOT

- Not magic words that always work. Without specificity, every framework dies.
- Not a substitute for the Brand Pack. Voice and audience come from the brand; the Vault provides structure.
- Not a closed list. Add brand-specific patterns under `brands/[slug]/voice.md`'s `phrases_to_use`.

---

## Files in this module

- `README.md` — this file
- `hooks.md` — first-slide patterns
- `headlines.md` — slide-level headline patterns
- `ctas.md` — call-to-action patterns
- `captions.md` — caption opener and closing patterns
- `arguments.md` — patterns for showing the mechanism
- `objections.md` — patterns for handling audience objections
- `transitions.md` — slide-to-slide transition patterns
- `proof-copy.md` — patterns for citing real proof
- `launch-copy.md` — patterns for product / feature / project launches
- `contrarian-copy.md` — patterns for opinionated, status-quo-challenging content

---

## How to use the Vault

1. Pick the pattern that fits your post's role.
2. Substitute placeholders with brand-specific terms.
3. Run the result through the Brand Pack's `voice.md` filter.
4. If a phrase from `voice.md`'s banlist appears, swap it.

---

## What's banned across all Vault patterns

- Manipulative framing (fake scarcity, fake urgency, fake guarantees).
- Deceptive language ("you'll be SHOCKED").
- Unsupported superiority claims ("the only X").
- Health / financial / legal claims without disclaimers.
- Patterns that contradict the Brand Pack's `constraints.md`.

---

## Mode support

| Mode | Supported? |
|---|---|
| Claude Project Manual | Yes |
| API Production | Yes — Claude pulls patterns by reference |
| Renderable Creative Worker | n/a (used upstream) |

---

## Related

- System: [`system/copywriting-rules.md`](../../system/copywriting-rules.md), [`system/content-system.md`](../../system/content-system.md)
- Often paired with: [`prompts/improve-hook.md`](../../prompts/improve-hook.md) (which uses `hooks.md`)

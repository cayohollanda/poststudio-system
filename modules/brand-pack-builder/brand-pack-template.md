# Brand Pack Template Reference

> The 12 Brand Pack files. The actual templates live under [`brands/_template/`](../../brands/_template/) and the worked example under [`examples/example-brand-pack/`](../../examples/example-brand-pack/).

---

## The 12 files

| # | File | What it captures |
|---|---|---|
| 1 | `brand.md` | Identity, founders, elevator pitch, why we exist, north star |
| 2 | `audience.md` | Primary segment (role + situation), pains, desires, non-audience, personas |
| 3 | `offer.md` | What we sell, mechanism, alternatives, positioning statement |
| 4 | `positioning.md` | One-liner positioning + differentiation matrix + category framing |
| 5 | `voice.md` | Adjectives, register, words to use / avoid, voice examples + anti-examples |
| 6 | `visual-style.md` | Visual mode, palette, typography, composition, lighting defaults, image-prompt blocks |
| 7 | `proof-assets.md` | Customer stories, testimonials (verbatim), metrics, press, what we cannot cite |
| 8 | `constraints.md` | Claims allowed / forbidden, compliance, trigger phrases, competitor handling, visual constraints, topic constraints |
| 9 | `content-pillars.md` | 3-5 content pillars with topic clusters and recurring series |
| 10 | `competitors.md` | Direct, adjacent, do-nothing alternatives with win/lose analysis |
| 11 | `examples.md` | Past content that worked, anchor posts, hook patterns the audience responds to |
| 12 | `rejected-examples.md` | Patterns banned because they hurt the brand |

---

## File-by-file generation order

When the Brand Pack Builder generates the pack, follow this order — each file builds on the previous:

```
1. brand.md           (identity)
2. audience.md        (who we're talking to)
3. offer.md           (what we sell)
4. positioning.md     (depends on 1-3)
5. voice.md           (depends on 1-2)
6. visual-style.md    (depends on 1-2)
7. competitors.md     (depends on 3)
8. constraints.md     (depends on 1-7)
9. proof-assets.md    (depends on 1-3)
10. content-pillars.md (depends on 1-7)
11. examples.md        (filled with what user provided)
12. rejected-examples.md (filled with what user provided)
```

---

## Format conventions

- Each file begins with `# [Title] — \`[brand-slug]\``.
- Each file ends with a one-line "When this file is filled in, you should be able to..." statement (the file's success criterion).
- Use `_(to be added)_` for genuinely unknown items.
- Use `[PROOF_NEEDED: short description]` for proof gaps.
- Use `_(to be validated)_` for things the user gave but flagged as unverified.

---

## Cross-file consistency

Some fields appear in multiple files. They must agree.

| Field | Files |
|---|---|
| Brand name + slug | `brand.md`, used as filename prefix in `brands/[slug]/` |
| Audience role | `audience.md` (primary), `offer.md` (positioning statement), `voice.md` (calibration) |
| Differentiators | `offer.md`, `competitors.md` |
| Voice adjectives | `voice.md`, referenced in `examples.md` and `rejected-examples.md` |
| Visual mode | `visual-style.md`, referenced in carousel image prompts |
| Claims allowed | `constraints.md`, must be backed by `proof-assets.md` |
| Compliance | `constraints.md`, possibly affecting `voice.md` and `claims-allowed` |
| Words to avoid | `voice.md` (banlist), `rejected-examples.md` (real cases) |

If two files disagree, the Brand Pack Builder must reconcile in the final review.

---

## Refresh cadence

- New brand → fill all 12 files.
- Existing brand → refresh quarterly, or after major changes.
- Triggered refreshes:
  - Major reposition → `brand.md` + `positioning.md` + `offer.md` + `voice.md`.
  - New audience segment → `audience.md` + `examples.md`.
  - New compliance requirement → `constraints.md`.
  - New high-performing post → `examples.md` + sometimes `voice.md` (capture audience-adopted vocabulary).

---

## Save location

Save under:

```
brands/<brand-slug>/
  brand.md
  audience.md
  offer.md
  positioning.md
  voice.md
  visual-style.md
  proof-assets.md
  constraints.md
  content-pillars.md
  competitors.md
  examples.md
  rejected-examples.md
```

`<brand-slug>` is lowercase, hyphenated, no spaces.

# Prompt — 19 Ephemeral Brand Intake (runtime, in-memory)

> **The canonical default brand-loading path for PostStudio System.** Run this at the start of any session where the user supplies brand context per-session (website + handle + voice samples + attached logos) and you need to consolidate it into a 12-section brand pack the rest of the session can use.
>
> This prompt produces an **in-memory** brand pack — it is NOT saved to the framework repo. Real brand instances never live in this repo.

---

## When to use

- The user opens a new Claude Project chat and pastes brand info (website content, handle, captions, voice samples) plus attaches logo files.
- The user wants to test the framework with a new brand without committing anything.
- The user has a real brand but keeps the brand pack outside this repo (Notion, Drive, private repo, local folder).

For deeper, interview-driven brand creation see [`prompts/02-generate-brand-pack.md`](02-generate-brand-pack.md). This prompt is the **fast** path.

---

## Inputs the user supplies in the chat

The user pastes (or attaches) some subset of the following. Anything missing → mark `[MISSING_INPUT]` aggressively.

| Input | Format | Required? |
|---|---|---|
| Brand name + slug | text (e.g. "WeAura Tech / weaura-ai") | required |
| Website content | paste of homepage + About page | strongly recommended |
| Instagram handle + bio | "@handle" + bio text | recommended |
| 3-5 sample captions | paste of recent post captions | recommended |
| Voice samples | paste of 3-5 representative sentences from the brand's site/posts | strongly recommended |
| Logo files | uploaded SVG/PNG (mark + variants + lockup) | required for visual delivery |
| Compliance / constraints | text (regulations, words-to-avoid, claims rules) | recommended |
| Existing positioning / offer | paste from internal docs | optional |
| Color palette / fonts | hex codes + font names | optional (otherwise inferred from site) |
| Primary visual mode | one of `system/visual-framework.md` modes 1-9 | optional |

---

## Prompt

```
You are operating PostStudio System.

The user has supplied (or is supplying) brand context per-session for an EPHEMERAL brand pack. You will consolidate that input into a 12-section brand pack held in your conversation context for the rest of this session. You will NOT save anything to the framework repo (the framework is brand-agnostic; brands are runtime data).

# Your job

1. Read whatever the user supplied: pasted website content, Instagram handle + bio + captions, voice samples, attached logo files, compliance text, any other context.
2. Infer / synthesize the 12 brand pack sections (matching `brands/_template/`):
   - 01_brand.md — name, slug, one-line description, mission, founding story (if available), category
   - 02_audience.md — primary + secondary personas with role, situation, jobs-to-be-done, pains, desires
   - 03_offer.md — what the brand sells, packaging, price tier, differentiation
   - 04_positioning.md — category, frame-of-reference, point of difference, "for [audience] who [want X], [brand] is [category] that [unique benefit] unlike [alternative]"
   - 05_voice.md — tone (3-5 adjectives), POV, vocabulary, signature phrases, words-to-avoid, lowercase/uppercase rules, sentence-length defaults
   - 06_visual-style.md — palette (hex), typography (headline + body), accent rule, primary visual mode (1-9 from system/visual-framework.md), mark variants the user supplied, layout principles
   - 07_proof-assets.md — what the user can substantiate (numbers, customers, certifications, testimonials, awards) — mark `[PROOF_NEEDED]` for anything you cannot verify from supplied input
   - 08_constraints.md — words-to-avoid, claims-allowed, claims-forbidden, compliance, imagery do/don't, taboo topics
   - 09_content-pillars.md — 3-5 pillars with one-line descriptions
   - 10_competitors.md — 3-5 competitors + their positioning + how this brand differs (mark `[MISSING_INPUT]` if user didn't supply)
   - 11_examples.md — 2-3 example posts in the brand's voice (synthesize from voice samples; clearly mark as exemplars not real posts)
   - 12_rejected-examples.md — explicit anti-patterns the brand would reject (LinkedIn-bro, generic SaaS speak, etc.) inferred from the brand's voice or stated by the user
3. Surface ALL gaps explicitly under "Missing Inputs / Proof Needed". Do not invent.
4. Confirm to the user which visual mode you chose (or recommend one of the 9 modes from `system/visual-framework.md`).
5. State which logo asset files the user supplied (read the binary attachments) and which mark variant goes on which background. Confirm you will base64-embed them when rendering — never redraw via SVG primitives.
6. End with a 1-paragraph summary of what's now active for this session and what's missing.

# Output format

Return Markdown with 12 H2 sections (one per brand pack file). Use placeholder `[MISSING_INPUT: short description]` wherever you don't have the input. Use `[PROOF_NEEDED: short description]` for anything that would be a claim Claude can't verify.

DO NOT save these files to disk. DO NOT commit anything. The brand pack lives in this conversation only.

After the 12 sections, include:
- ## Summary — 4 bullets: brand slug, primary visual mode chosen, logo assets confirmed, gaps to fill before shipping anything
- ## Missing Inputs / Proof Needed — bulleted list

# Hard rules

- Do NOT invent customer names, awards, certifications, numbers, quotes, or testimonials. If the user didn't supply them, mark `[PROOF_NEEDED]`.
- Do NOT pick a visual mode arbitrarily. If the brand looks editorial, recommend Mode 2 or Mode 9. If cinematic, Mode 1. If technical, Mode 6 or 9. State your reasoning.
- Do NOT claim assets exist that the user didn't attach. If no logo file was supplied, mark `[ASSET_NOT_FOUND]` for that mark variant and ask the user to attach it.
- Do NOT mix two brands in one intake. One brand per session.
- Do NOT recommend `decoded-editorial` (Mode 9) unless the user is planning carousels of 10+ slides AND has supplied enough visual cues to fill the placeholder list (see `system/visual-framework.md` Mode 9 placeholder table).

# Then

After the user confirms the intake (or supplies missing inputs), they will run `prompts/04-generate-carousel.md` or `prompts/13-generate-renderable-carousel.md`. The brand pack you built lives in this conversation's context — pull from it on every subsequent prompt this session.
```

---

## How to use

### Step-by-step

1. Open a new chat in your Claude Project (with `poststudio-system/` loaded as Project Knowledge).
2. Paste this prompt.
3. In the same message (or the next), paste:
   - Brand name + slug
   - Website homepage + about content
   - Instagram handle + bio + 3-5 sample captions
   - Voice samples
   - Compliance / constraints
4. Attach the logo files (SVG preferred, PNG fallback): `mark-primary`, `mark-dark`, `lockup-on-light`, `lockup-on-dark`, etc.
5. Claude returns the 12-section brand pack with explicit `[MISSING_INPUT]` and `[PROOF_NEEDED]` markers.
6. Fill the gaps in your next message, OR proceed to `prompts/04-generate-carousel.md` knowing the gaps.

### Example invocation

```
Brand: WeAura Tech (slug: weaura-ai)

Website (weaura.tech homepage + about):
[paste content here]

Instagram: @weaura.tech
Bio: [paste bio]
Sample captions:
1. [paste caption 1]
2. [paste caption 2]
3. [paste caption 3]

Voice samples:
- "[paste 3-5 representative sentences]"

Compliance: SOC 2 Type II, ISO 27001, GDPR. Avoid claims about specific outcomes.

[Attached: mark-primary.svg, mark-dark.svg, lockup-on-light.svg, lockup-on-dark.svg]

Primary visual mode (preference): decoded-editorial — we want long-form (10-14 slide) tactical carousels.

Color palette (preference): VOLT #CCFF00, DARK #0B0B0A, CREAM #F2EFE8, LIGHT #FAF8F3.
Typography: Manrope SemiBold (headline) / Manrope Regular (body).
Header label left: POWERED BY POSTSTUDIO
Header label right: @poststudio.ai · MAY 2026 ®
```

Claude returns the 12-section brand pack with confirmation of the visual mode + asset slot mapping.

---

## When NOT to use this prompt

- The user wants to commit a brand pack into the framework repo. **Refuse** — `.gitignore` blocks it; brand instances don't live in this framework. Save to a private location instead.
- The user is in Mode 2 or Mode 3 (API / Renderable Worker) — those modes get the brand pack passed structurally via `generation-request.schema.json`, not via this prompt.
- The user already has a complete `brands/[slug]/` folder uploaded into Project Knowledge → just tell Claude "use brands/[slug]/" and skip this prompt.

---

## Pairing with other prompts

The natural Mode 1 sequence:

1. **`prompts/00-start-here.md`** → orient Claude.
2. **This prompt (`19-ephemeral-brand-intake.md`)** → consolidate session brand context.
3. **`prompts/04-generate-carousel.md`** → ship a carousel.
4. **`prompts/18-deliver-carousel-as-png-zip.md`** → render PNG zip.
5. **`prompts/08-critique-output.md`** → adversarial review.

If the brand pack still has `[MISSING_INPUT]` gaps you care about, fill them inline before step 3, OR run [`prompts/02-generate-brand-pack.md`](02-generate-brand-pack.md) for a deeper interview pass.

---

## Why this prompt exists

PostStudio System is a **framework**, not a brand registry. The `brands/` folder in this repo only tracks `_template/` (the scaffold). Real brand instances are **runtime data**, supplied per session. This prompt is the canonical, fastest path to consolidate that runtime data into the framework's 12-section brand pack contract.

The previous default — "name a brand slug, Claude loads `brands/[slug]/` from the repo" — implicitly assumed brand instances should live in this framework repo. They shouldn't. This prompt corrects the default.

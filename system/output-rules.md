# Output Rules — Markdown (Mode 1)

> Formatting rules for human-readable Markdown outputs. Used in Claude Project Manual Mode and any case where the consumer is a human.

The full per-module output contracts are in [`docs/output-contract.md`](../docs/output-contract.md). This file is the **rule layer**: what's enforced regardless of module.

---

## Universal Markdown rules

1. **No preamble.** Don't start with "Here's your carousel!" or "I'd be happy to help." Start with the first heading.
2. **No closing pleasantries.** Don't end with "Hope this helps!" or "Let me know if you need adjustments."
3. **No emojis** unless the Brand Pack explicitly allows them.
4. **Use headings as defined per module.** Don't invent new top-level sections.
5. **Use `—` for empty fields** in slide blocks (never blank).
6. **Mark gaps explicitly.** `[PROOF_NEEDED: short description]` and `[MISSING_INPUT: short description]`.
7. **Quality Checklist scorecard at the end** — every output, every module.
8. **Missing Inputs / Proof Needed at the end** — every output, every module.

---

## Headings hierarchy

- `# Section` — top-level (Strategic Angle, Carousel, Caption, etc.).
- `## Subsection` — slide blocks, beat blocks, post blocks.
- `### Sub-subsection` — rare; only when needed (e.g. inside a campaign output for arcs).

Don't use `#####` or deeper.

---

## Slide / beat / post block format

Every block uses this format with bullets and bold-free fields:

```markdown
## Slide N — [Role]
- Role: [role]
- Headline: [line]
- Support text: [line or —]
- Visual concept: [one sentence]
- Image prompt: [generator-ready]
- Layout instruction: [designer-ready]
```

Don't bold field labels. Don't use tables for slide blocks.

---

## Lists vs prose

- **Use lists** for slides, beats, posts, alternative hooks, checklists, missing inputs.
- **Use prose** for Strategic Angle, Caption, First Comment, Body of any analysis.

Don't mix list-of-prose-paragraphs (looks like a blog post) when a structured block is expected.

---

## Code blocks

- Use fenced code blocks (` ``` `) for image prompts, JSON snippets, config examples, schema fragments.
- Specify language for JSON / HTML / CSS / SVG (` ```json `).
- Don't wrap entire outputs in code blocks.

---

## Quoting

- Use blockquotes (`>`) for actual quotes (testimonials from `proof-assets.md`, audience reactions, founder anecdotes).
- Don't use blockquotes for "callout" decoration.

---

## Tables

Use tables only when the data is genuinely tabular (e.g. score matrices, compliance posture, comparison rows).

```markdown
| Metric | Value | Source |
|---|---|---|
| ... | ... | ... |
```

Don't use tables for slide content.

---

## File-name references

When pointing to repo files inside an output, use Markdown links:

```markdown
[`system/operating-system.md`](../system/operating-system.md)
```

Or relative reference depending on the output location. The `docs/output-contract.md` examples show the pattern.

---

## Length discipline

- Carousel slides → headlines 6-12 words, support text 0-8 words.
- Captions → platform sweet spots (LinkedIn 150-250, Instagram 100-180, TikTok 80-140).
- First comment → 30-80 words.
- Strategic Angle → 1-3 sentences.
- Quality Checklist → standard 30-line scorecard format.

If you're tempted to write more, ask: "what's the *one* thing this paragraph adds?" If you can't answer, cut it.

---

## What's banned in Markdown output

- Emojis (unless Brand Pack allows).
- Decorative dividers (`---` is fine for actual section breaks; not for "fancy spacing").
- Marketing jargon ("synergy," "transform," "supercharge," etc. — see `copywriting-rules.md`).
- "Take your X to the next level" energy.
- Filler intensifiers ("absolutely," "literally," "100%").
- Bold for emphasis on every other word.
- ALL CAPS unless quoting an audience reaction or referenced acronym.
- Numbered lists for things that aren't ordered (use bullets for unordered).

---

## Module-specific overrides

Some modules have additional rules. See:

- Carousel → `docs/output-contract.md`
- Diagnosis → `modules/brand-diagnosis-agent/output-template.md`
- Brand Pack → `modules/brand-pack-builder/output-template.md`
- Strategy → `modules/content-strategy-os/content-pillars-template.md` and related
- Reels → `modules/reels-script-machine/output-template.md`
- Campaign → `modules/campaign-builder/launch-campaign-template.md` and related
- Quality review → `modules/quality-gate/critique-rubric.md`
- Monthly content plan → `outputs/_template/monthly-content-plan.md`

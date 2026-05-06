# Prompt — Generate Brand Pack

> Interview-driven prompt that turns "I have a brand somewhere in my head" into 8 structured Brand Pack files ready to drop into `brands/[your-brand-slug]/`.

The Brand Pack is the load-bearing context for every future carousel. Spending 30-60 minutes here saves hours per post forever after.

---

## How to invoke

Paste this prompt into Claude. It will run a 10-section interview, one section at a time, with crisp follow-up questions when answers are vague. At the end, it returns the 8 Brand Pack files.

---

## Prompt

```
You are operating PostStudio System.

Your job: build a complete Brand Pack for a new brand by interviewing the user.

# Process

You will interview the user across 10 sections, one at a time.
For each section:

1. Ask the section's questions in a single message (4-8 questions max).
2. Wait for the user's answer.
3. If answers are vague, ask 1-2 sharpening follow-ups before moving on.
4. Confirm understanding back in 2-3 sentences ("Here's what I'm hearing...").
5. Move to the next section.

After all 10 sections are complete, generate the 8 Brand Pack files in full and return them in a single message, formatted with file headers, ready to copy into brands/[brand-slug]/.

# The 10 sections

## 1. Brand basics
- What's the brand name?
- One-line description (what is it, in plain language)?
- Category / industry?
- Stage (pre-launch, early traction, scaling, established)?
- Website / product URL (for context only — do not visit)?

## 2. Audience
- Who is the primary audience? (role + situation, not demographics)
- What's their #1 pain in your category?
- What do they want that they can't currently get?
- Who is NOT the audience (explicitly)?
- Where do they hang out online?

## 3. Offer
- What does the brand sell or offer?
- What problem does it solve, in the audience's own words?
- What's the unique mechanism — what makes it actually work?
- What does it explicitly NOT do? (positioning by negation)

## 4. Positioning & differentiation
- What's the one-line positioning statement? ("[BRAND] is the [CATEGORY] for [AUDIENCE] who want [OUTCOME] without [PAIN].")
- Top 3 differentiators?
- Top 3 alternatives the audience considers (these are the "competitors")?
- What's a fair comparison vs alternatives — where do you win, where do you lose?

## 5. Tone of voice
- Describe the brand voice in 5 adjectives.
- Read your favorite 2-3 pieces of brand-aligned content (from anywhere) and tell me what makes them feel right.
- Are there words / phrases you love?
- Are there words / phrases you ban?
- Formal or casual? Lowercase-friendly?
- First person ("I"), plural ("we"), or third person?
- Any reference brands whose voice you admire (without copying)?

## 6. Visual identity
- Primary brand colors (hex codes if you have them)?
- Accent color used sparingly?
- Typography (headline + body)?
- Visual mode preference from system/visual-framework.md (or "I don't know yet")?
- Logo placement preference (corner, center, hidden)?
- Mood references (films, magazines, brands you admire visually)?

## 7. Proof assets
- Best customer story you can share publicly?
- Real quotes / testimonials you can use verbatim?
- Hard numbers (revenue, users, retention, time saved) you can cite publicly?
- Awards, press mentions, public artifacts?
- Things you have but CAN'T cite publicly (mark them so we don't accidentally use them)?

## 8. Claims & constraints
- Claims you ARE comfortable making publicly?
- Claims you are NOT comfortable making (legal, factual, ethical)?
- Compliance constraints (HIPAA, GDPR, SEC, FDA, etc.)?
- Any phrases that could trigger lawyer review (e.g. "guaranteed," "FDA-approved," "AI-powered")?

## 9. Content examples
- Share 1-3 pieces of YOUR existing content that worked well — what was the topic, format, and outcome?
- Share 1-2 pieces that didn't land — what went wrong?
- Any content patterns you've banned internally?
- Any content rituals you've kept (e.g. "every Friday we do a build-in-public update")?

## 10. CTA preferences
- Default CTA goal (save, comment, click, DM, follow, try)?
- Preferred CTA phrasing?
- Any CTAs you ban (e.g. "Sign up now")?
- What action is the *long-term* business goal (the thing the carousels eventually point to)?

# Output

After the interview, return all 8 Brand Pack files inline, one after another, in this order:

1. brand.md
2. audience.md
3. offer.md
4. voice.md
5. visual-style.md
6. proof-assets.md
7. constraints.md
8. examples.md

Each file should follow the templates in brands/_template/ exactly.
Each file should be filled in fully — no "[fill in later]" placeholders unless the user explicitly couldn't answer.
Mark genuinely unknown items as `_(to be added)_` so the user can find them later.

# Behavior

- Do not skip sections. If the user tries to skip, ask why and propose a minimum-viable version.
- Do not invent answers. If the user doesn't have an answer, mark it `_(to be added)_`.
- Be conversational, not bureaucratic. The user is doing real thinking; help them.
- After each section, confirm understanding before moving on.
- If the user is moving fast and answers feel shallow, slow them down with one question: "Can you give me a real example of that in action?"
```

---

## Example session opening

When you run this prompt, Claude will start with:

> Hi — let's build your Brand Pack. We'll do 10 short sections, one at a time. The whole thing usually takes 30-60 minutes if you're decisive, longer if you want to think out loud.
>
> **Section 1 — Brand basics**
>
> 1. What's the brand name?
> 2. In one line, what is it (plain language, no marketing speak)?
> 3. What category / industry would someone search for it under?
> 4. What stage is the brand in — pre-launch, early traction, scaling, or established?
> 5. (Optional, for context only) what's the website or product URL?

You answer; Claude confirms what it heard; moves to Section 2.

---

## What to do with the output

Once Claude returns all 8 files:

1. Pick a `brand-slug`. Lowercase, hyphenated, no spaces. Examples: `lumen`, `acme-co`, `brand-x`.
2. Create the folder `brands/[your-brand-slug]/`.
3. Save each file Claude returned with its exact filename (`brand.md`, `audience.md`, etc.).
4. Re-upload the folder to your Claude Project.
5. From now on, prompts can refer to `brands/[your-brand-slug]/` and the system will load it.

---

## Refresh policy

Brand Packs decay. Plan to refresh:

- Every 90 days at minimum.
- After a major positioning change, repositioning, or rebrand.
- After learning something significant about the audience.
- After shipping a major feature that changes the offer.

To refresh, you can either re-run the full interview or use a targeted version:

```
Refresh Section [N] of the Brand Pack at brands/[slug]/.
Ask me only the questions in that section.
Then update only the relevant Brand Pack file.
```

---

## When NOT to use this prompt

- The brand already has a strong, working Brand Pack at `brands/[slug]/`.
- You have ≤ 5 minutes — minimum-viable Brand Pack still requires ~20 minutes of real input. Don't shortcut this.
- You're testing PostStudio System and don't have a real brand — use `examples/example-brand-pack/` (the fictional Lumen brand) instead.

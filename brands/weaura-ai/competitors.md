# Competitors — `weaura-ai`

> Per `constraints.md`, WeAura does not name specific competitors in social posts. Categories below describe the alternative landscape; specific names are kept in this file for internal use only and never appear in public content.

---

## Direct competitors (do NOT name in posts)

### Generic AI chat assistants
- **Type:** indirect; the current default for engineers experimenting with AI in ops.
- **What they do:** open-ended LLM chat with no infrastructure context.
- **Where they win:** drafting, language tasks, one-off Q&A, brainstorming.
- **Where we win:** anything requiring grounding in *your* infrastructure. Generic AI hallucinates ops answers because it has no signal correlation, no runbook context, no deploy timeline.
- **Audience overlap:** universal — every senior engineer has tried at least one.
- **Off-limits to name in posts:** yes (per `constraints.md`).
- **Last reviewed:** 2026-05-06.

### Observability platforms with bolted-on AI features
- **Type:** direct adjacent.
- **What they do:** add a chat UI on top of their existing metrics / logs / traces dataset.
- **Where they win:** if the customer is already deeply invested in that vendor's stack and wants one more feature.
- **Where we win:** correlation across the *full* tool stack — observability + deploys + runbooks + ticketing + IaC — not just within one vendor's silo. Plus citation contract, plus HITL, plus self-hosted.
- **Audience overlap:** mid-high.
- **Off-limits to name in posts:** yes.
- **Last reviewed:** 2026-05-06.

### SRE / incident-response point tools with AI features
- **Type:** direct adjacent.
- **What they do:** auto-remediation, AI-assisted runbooks, AI alert correlation in single-vertical tools.
- **Where they win:** if the customer wants a single-purpose tool for one part of the workflow.
- **Where we win:** breadth of grounded context (signals + knowledge base + integrations) and the HITL safety contract. We don't auto-remediate; we don't pretend to.
- **Audience overlap:** mid.
- **Off-limits to name in posts:** yes.

---

## Adjacent products

### Internal RAG-over-Confluence projects
- **Type:** DIY alternative.
- **What it is:** a custom-built internal RAG pipeline indexing the team's docs into a vector DB.
- **Why audiences use it:** free; customizable; "we'll just build it ourselves."
- **What we offer over it:** maintained, integrated with *live* ops signals (not just static docs), enterprise compliance posture, semantic re-ranking, citation contract, approval workflow, multi-tenancy. The average internal RAG project lacks all of these and goes stale within 6 months.
- **When the audience picks it over us:** when the team has spare engineering bandwidth and explicitly wants to own the build.

### Notion AI / Confluence AI / built-in vendor "AI features"
- **Type:** adjacent.
- **What they do:** AI search and chat over the docs already in those tools.
- **Where they win:** scope is small; just docs, no infra.
- **Where we win:** correlation with live infra signals (not just docs).

### Runbook-management / playbook tools (with or without AI)
- **Type:** adjacent.
- **What they do:** runbook authoring, versioning, distribution.
- **Where they win:** authoring workflow.
- **Where we win:** WeAura is downstream of authoring — we *use* runbooks. The two coexist.

---

## Internal hacks / DIY

### "I'll just kubectl + grep harder this week"
- **What it is:** willpower + chaining 5 commands.
- **Why audiences use it:** zero install cost; preserves the illusion of operational competence.
- **What we offer over it:** correlation. The 5-command chain doesn't show *why* the deploy 3 hours ago is the proximate cause.

### Custom internal scripts + a Slack bot
- **What it is:** a bash-script wrapper exposing a few common diagnostics in Slack.
- **Why audiences use it:** done in a hackathon; visible to the team; maintained casually.
- **What we offer over it:** grounded retrieval + audit + multi-tenant + the things hackathon scripts never get to.

---

## "Do nothing" — the most important competitor

- **Why audiences default here:** "We've always done it this way." The 5-dashboard pivot during incidents feels like operational competence, not a tax.
- **What it costs them:** measurable hours per engineer per week; un-measurable trust during P0s.
- **The trigger that makes them act:** a P0 RCA that took 6+ hours to compile. Or a SOC 2 audit finding about runbook drift. Or a new senior engineer who quits because they spent their first 3 months in dashboard archaeology.
- **How we surface this in content:** Pillar 3 (The senior-engineer week) — recognizable, specific stories about what the 5-dashboard pivot actually costs.

---

## Competitor monitoring cadence

- **Weekly:** scan public posts and announcements from the categories above (no scraping; manual reading list).
- **Monthly:** update win/lose analysis if anything material changed.
- **Quarterly:** full refresh of this file.

---

## Off-limits framings (per `constraints.md`)

- We do NOT name competitors in posts.
- We do NOT run side-by-side comparison tables with named competitors.
- We do NOT say "we're the [Real Competitor] killer."
- We DO compete on category positioning ("generic AI chat," "observability AI features," "internal RAG-over-Confluence projects," "the 5-dashboard pivot," "do nothing").

---

## Refresh log

| Date | What changed |
|---|---|
| 2026-05-06 | Initial competitive landscape derived from category analysis. Specific competitor names kept internal per `constraints.md`. |

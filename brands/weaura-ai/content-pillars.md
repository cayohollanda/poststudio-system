# Content Pillars — `weaura-ai`

> 4 pillars. Every WeAura post is tagged with one.

---

## Pillar 1 — Grounded AI for ops (the thesis)

**Argument:** AI without grounding is a liability for ops teams. The next layer of ops AI isn't a chat box — it's the correlation layer that knows the customer's specific environment.

**Audience pain it addresses:** generic AI assistants give wrong answers about *their* infrastructure because they have zero context. Engineers have stopped trusting them.

**Belief being challenged:** "If we just plug a better LLM into our docs, we'll have a useful ops assistant."

**Topic clusters:**
1. Why generic AI fails for ops (mechanism — no grounding).
2. The "every answer cites its source" thesis.
3. The case against chat-shaped AI for ops.
4. Grounded vs ungrounded — the actual architectural difference.
5. Why bolted-on observability AI features fall short.
6. The "knowledge base viva" pattern — runbooks as live indexed clusters.
7. RAG done right vs RAG done as a demo.
8. "Hallucination" as a category mistake — the citation contract reframes it.
9. Why the answer to "why is X happening" must be correlated, not assembled by hand.
10. Senior engineers don't trust AI. They trust *citations*.
11. Tribal knowledge as an architectural problem.
12. Onboarding speed as a downstream effect of grounded AI.

**Dominant content type:** carousel + long-form essay.
**Funnel position:** TOFU + MOFU.
**Series anchored:** "Grounded Ops" — proposed weekly carousel series.
**Content-to-offer link:** reader recognizes that grounding is the missing piece → reads architecture page → requests demo.
**Proof currently available:** public-site claims (Citation Contract, immutable audit, semantic re-ranking). Customer-specific proof: `[PROOF_NEEDED]`.

---

## Pillar 2 — HITL approval workflows (the safety thesis)

**Argument:** Write operations in production must require human approval. Auto-remediation without HITL is a category mistake. WeAura is opinionated about this — read-only auto-executes, writes always queue for approval.

**Audience pain it addresses:** engineers and CISOs are scared of AI write operations. The fear is correct *if* there's no approval workflow. WeAura turns the fear into a non-issue.

**Belief being challenged:** "AI in ops means the AI is going to break production."

**Topic clusters:**
1. The HITL pattern — why "read-only auto, writes need approval" is the only safe shape.
2. The audit trail as the safety contract.
3. Role-based gating — who can approve what, with TTL.
4. "What an AI ops tool is and isn't allowed to do" — design principles.
5. Why auto-remediation is a category mistake (not a feature).
6. CISO-friendly AI in ops: the architectural answer.
7. Self-hosted + zero data egress as a non-negotiable.
8. Immutable audit with 7-year retention — what that actually means.
9. SAML/OIDC/SCIM for ops AI — table stakes, not premium.
10. "Pluggable models" — why model lock-in is wrong for ops.

**Dominant content type:** carousel + technical-blueprint slides.
**Funnel position:** MOFU (decision-stage; CISOs and engineering leaders).
**Series anchored:** "Architecture Notes" — proposed bi-weekly engineering essay series.
**Content-to-offer link:** decision-makers see HITL + audit + self-hosted → security review passes → procurement.
**Proof currently available:** public-site `/context/` and `/features/` (HITL design, audit retention, SSO, self-hosted).

---

## Pillar 3 — The senior-engineer week (the audience-recognition pillar)

**Argument:** Senior platform / SRE / DevOps engineers lose hours per week to context-switching, runbook archaeology, and the 5-dashboard pivot during incidents. The week is a story of correlation tax.

**Audience pain it addresses:** the recognizable Tuesday afternoon — "why is X restarting" → 6 hours later, finally a working theory.

**Belief being challenged:** "More dashboards / more docs / more discipline will fix this."

**Topic clusters:**
1. The 5-dashboard pivot tax, in concrete terms.
2. Runbook drift — why your docs are 3 months stale.
3. Tribal knowledge as a single point of failure.
4. The cost of context-switching during P0s.
5. What a 1.4-second RCA actually looks like.
6. Onboarding new SREs — why it takes 3-6 months and how to shrink that.
7. Post-mortem culture vs post-mortem hygiene.
8. The deploy timeline as the most under-used signal.
9. PromQL bridges, Loki traces, and the correlation gap.
10. Senior engineers don't have more focus — they have fewer pivots.

**Dominant content type:** carousel + reel (30s) for the most relatable pains.
**Funnel position:** TOFU (pure recognition + audience-building).
**Series anchored:** "Operator Notes" — proposed weekly carousel series in the senior-engineer voice.
**Content-to-offer link:** recognition → trust → follow → demo.
**Proof currently available:** the public-site `/context/` "Marina / payment-api" example is citable as a marketing illustration.

---

## Pillar 4 — Build-in-public engineering essays

**Argument:** WeAura's architecture is opinionated and worth showing. The "no chat" decision, the multi-tenant row-level-security choice, the pgvector + semantic re-ranking decision — each is a teachable artifact for the engineering community.

**Audience pain it addresses:** engineering leaders curious about how a grounded AI platform actually works under the hood.

**Belief being challenged:** "AI products are black boxes."

**Topic clusters:**
1. Why we use pgvector (not a dedicated vector DB).
2. Multi-tenant row-level security with implicit `tenantId` carry — pattern + tradeoffs.
3. Approval workflows as a state machine.
4. The `@aura/agents` + `@aura/tools` separation.
5. How we handle model pluggability (OpenAI / Anthropic / vLLM).
6. The "knowledge base viva" indexing pipeline.
7. Why we ship Helm chart + native Kubernetes (not a SaaS-only path).
8. The semantic re-ranking pipeline that prevents hallucinations.
9. SSE for streaming chat — what we got right and wrong.
10. Lessons from building HITL approvals into a UI engineers actually use.

**Dominant content type:** technical-blueprint carousels + long-form engineering essays.
**Funnel position:** TOFU + retention (compounds developer trust).
**Series anchored:** "Architecture Notes" (shared with Pillar 2).
**Content-to-offer link:** engineer reads → respects the architecture → recommends WeAura when peers ask "what AI ops tool actually works."
**Proof currently available:** the public site's `/context/` page documents the architecture publicly; citable.

---

## Pillar coverage targets (monthly)

| Pillar | % of monthly content |
|---|---|
| Pillar 1 — Grounded AI for ops | 35% |
| Pillar 2 — HITL approval workflows | 20% |
| Pillar 3 — The senior-engineer week | 30% |
| Pillar 4 — Build-in-public engineering | 15% |

---

## Audience belief shifts

**Before:** "AI in ops is either useless (generic chat) or dangerous (auto-remediation)."
**After:** "Grounded AI with citation contract and HITL approval is the only safe shape — and it's measurably faster than the 5-dashboard pivot."
**The shift:** *grounded* + *cited* + *HITL* = the new default for ops AI.

---

## Refresh cadence

- Quarterly review.
- Triggered refresh after major product announcements.
- Pillar pruning if a pillar consistently underperforms across 2+ months.

---

## Refresh log

| Date | What changed |
|---|---|
| 2026-05-06 | Initial 4-pillar structure derived from public-site positioning at https://weaura.ai. |

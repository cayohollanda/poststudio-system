# Constraints — `weaura-ai`

> What WeAura may and may not say, do, or claim. Hard boundary for the system.

---

## Claims allowed

| Claim | Supporting evidence | Notes |
|---|---|---|
| "Self-hosted with zero data egress" | public site `/features/` | exact phrasing only |
| "SOC 2 Type II certified" | public site footer + `/features/` | always include "Type II" — never just "SOC 2" |
| "ISO 27001 certified" | public site footer + `/features/` | always include the standard number |
| "GDPR compliant" | public site footer | always allowed |
| "Every response cites its sources" | public site `/features/` "Citation Contract" | always allowed; brand-signature claim |
| "Read-only operations execute in seconds; write operations require human approval" | public site `/context/` | always allowed; HITL is core to the product |
| "Native integrations with Kubernetes (EKS/GKE/AKS/on-prem), Prometheus, Loki, Tempo, ArgoCD, GitHub/GitLab, Confluence/Notion, PagerDuty, Slack" | public site `/features/` | use the integration list verbatim |
| "Immutable audit trail with 7-year retention" | public site `/features/` | always allowed |
| "Data residency in BR / US / EU / custom" | public site `/features/` | always allowed |
| "Pluggable models: OpenAI, Anthropic, or open-source via vLLM" | public site `/features/` | always allowed |
| "User data never trains models" | public site `/features/` | exact phrasing only |
| "PromQL bridge with 14-day indexed retention" | public site `/features/` | always allowed |
| "Deploy in minutes, not weeks" / "Helm chart deploy" | public site | always allowed |

## Claims forbidden

| Forbidden claim | Why |
|---|---|
| "10x your incident response" | unprovable; banned vocabulary; not how we measure value. |
| "AI-powered" (as a generic descriptor) | generic; replace with specific mechanism ("grounded in your infra," "with citation contract," "with HITL approval"). |
| "Industry-leading" | no defensible third-party ranking. |
| "Faster than [Real Competitor]" | we don't run head-to-head benchmarks publicly; competitive sensitivity. |
| "Saves you X hours per week" — specific number | sample size insufficient until benchmark study completes. |
| "Used by [number] companies" | until customer count is public, do not state. |
| "Zero hallucinations" | over-claim; replace with "every answer cites its source" + the audit-trail facts. |
| "Replaces your SREs" | over-claim; demoralizing; off-brand. The product augments senior engineers; it does not replace them. |
| "All-in-one ops platform" | contradicts positioning by negation. |
| "AI assistant" / "chatbot" / "copilot" | wrong category framing. |
| Quotes from customers we do not have explicit permission to attribute | always banned, even anonymized when the quote could identify a specific customer. |
| "WeAura will [auto-fix / auto-remediate / auto-deploy] X" | we explicitly do NOT auto-execute writes; misrepresentation. |
| "Trusted by Fortune 500" | until we have explicit permission and a citable customer, never. |

---

## Compliance posture

- **Frameworks WeAura is held to:** SOC 2 Type II, ISO 27001, GDPR.
- **Statement to use publicly:**
  - "SOC 2 Type II certified."
  - "ISO 27001 certified."
  - "GDPR compliant."
  - When discussing data residency: "BR, US, EU, or custom."
  - When discussing data egress: "Self-hosted with zero data egress" (per the public-site phrasing).
- **Required disclaimers:** none for general posts. For posts that touch regulated industries (finance, health) where the customer is in that vertical, defer to the customer's compliance posture and avoid making claims about *their* compliance from *our* product.
- **Topics requiring legal review before publishing:**
  - Anything implying we hold a certification we don't have (e.g. HIPAA — _not currently certified per public site_).
  - Any direct comparison naming a competitor.
  - Any new metric not already in `proof-assets.md`.
  - Any claim about customer outcomes without customer-quoted evidence.

---

## Trigger phrases for legal / leadership review

If a draft contains any of these, the system flags `LEGAL REVIEW: [phrase]` in Missing Inputs:

- "guaranteed"
- "scientifically proven"
- "the only [category]"
- "100%" (in any AI / ops claim)
- "zero hallucinations"
- "auto-remediate" / "auto-fix" / "self-heal" (we explicitly do not)
- "FDA" / "HIPAA" / "PCI-DSS" / "FedRAMP" — only if a future certification is added to `proof-assets.md`
- Direct named-competitor adjectives ("[Competitor] is slow / unsafe / wrong")
- "best AI ops platform"
- "outperforms" without a published methodology

---

## Competitor handling

- **Competitors we may name in posts:** none directly. We compete on category positioning ("generic AI chat," "observability AI features," "internal RAG-over-Confluence projects"), not by name.
- **Competitors we may NOT name:**
  - General-purpose AI chat assistants (we describe by category).
  - Observability-platform AI features (we describe by category).
  - SRE / incident-response point tools (we describe by category).
- **How to discuss competitors when not naming them:**
  - Acceptable: "generic AI chat," "observability platforms with bolted-on AI features," "internal RAG projects," "the 5-dashboard pivot," "do nothing."
  - Acceptable: "the most common alternative is..." (without naming).
  - Unacceptable: thinly disguised name-drops, sneer language, false equivalences.

### Off-limits framings

- "We're the [Real Competitor] killer." → never.
- Side-by-side comparison tables naming competitors. → never.
- "[Competitor] users are switching to WeAura." → never (no source).

---

## Image / visual constraints

- **People in photography:** no faces, no people by default. Object-led, diagram-led visuals only.
- **Faces of real customers:** never publish a customer face.
- **Real product UI screenshots:** allowed only if the data shown is sample / redacted; never real-customer data; never anything that could identify a specific tenant.
- **Real competitor UI screenshots:** never.
- **Brand-adjacent imagery (e.g. partner logos):** none. We do not show partner logos in social content.
- **Stock photography:** banned. Render or commission.
- **AI-typical visual artifacts:** must be removed or hidden before shipping (eyes, hands, garbled text). Easy in our case because no people.

---

## Topic constraints

### On-brand topics (green light)
- The interrupt-heavy ops week.
- Why generic AI fails for ops.
- Citation contracts and audit trails.
- HITL approval workflows.
- Knowledge-base drift in real ops teams.
- Self-hosted vs SaaS for AI tools in regulated environments.
- Architecture deep-dives (multi-tenant, RAG, pgvector, vLLM).
- Build-in-public engineering essays from the team.
- The "5-dashboard pivot" tax on RCA.

### Off-brand topics (never publish)
- Macro-political commentary.
- Funding / valuation gossip about WeAura or competitors.
- "Hot takes" about other founders / public figures.
- AI-doomer / AI-utopian content. We're a product company; we ship.
- Generic "AI productivity" hot takes that don't connect to grounded ops.

### Topics requiring case-by-case review
- AI safety / alignment commentary (we have a position via HITL; we publish only if framed concretely around our product).
- Customer industry call-outs (we don't say "fintech customers do X" without permission).
- Competitive moves (we discuss the category, not the company).

---

## Tone constraints

- **Sarcasm / cynicism level:** dry, sparing. Never directed at users, customers, or named competitors. Sarcasm about *the category* (chat-shaped AI, dashboard pivots) is allowed.
- **Self-deprecation:** allowed when an engineering essay is about a real WeAura mistake / lesson learned. Never about WeAura-the-product as a self-deprecating joke.
- **Profanity:** none.
- **References to current events:** only when directly relevant to the audience's work (e.g. an industry-wide AI shift, a major incident in the ecosystem). Never political, cultural, or trending hashtag bait.
- **Humor at the expense of customers / users / "users who don't know better":** never.
- **Humor at the expense of competitors:** never. The category yes; specific brands no.

---

## Refresh log

| Date | What changed |
|---|---|
| 2026-05-06 | Initial constraints derived from public-site signal at https://weaura.ai. Compliance claim phrasing locked. Competitor handling rule set. |

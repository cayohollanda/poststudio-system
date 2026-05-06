# Positioning — `weaura-ai`

> WeAura is the AI platform for ops teams that *knows* the customer's infrastructure — grounded, cited, self-hostable. Not a generic chat. Not another dashboard.

---

## Positioning statement

**WeAura AI is the grounded AI platform for SRE / DevOps / Platform teams running Kubernetes who want correlated, cited answers about their own infrastructure — without their data leaving their perimeter, and without write operations executing without human approval.**

Compressed: *"A IA que conhece sua infraestrutura."* / "The AI that knows your infrastructure."

---

## Why this positioning

The "AI for ops" category is forming around two failure modes: (1) generic AI assistants that hallucinate because they have no grounding in the customer's actual stack, and (2) observability vendors bolting on chat over the same data they already had. WeAura positions against both. Its mechanism is *grounding plus citation plus HITL* — three things generic AI cannot provide and observability platforms don't bundle. The category we partly invent is "grounded AI for ops."

---

## Category framing

- **Category we claim:** grounded AI for SRE / DevOps / Platform operations.
- **Category we are NOT in:** generic AI chat, observability dashboards, customer-facing chatbots, code copilots, traditional RPA, runbook-management tools.
- **Category-level argument we make:** AI without grounding is a liability. Generic AI doesn't know your infrastructure; it can't safely answer ops questions. The next layer of AI for ops won't be chat-shaped over fragmented data — it will be the correlation layer that knows the customer's specific environment.

---

## Differentiation matrix

| Dimension | WeAura AI | Generic AI Chat | Observability "AI features" | Internal RAG-over-Confluence | "Do nothing" (5-dashboard pivot) |
|---|---|---|---|---|---|
| Grounding | grounded in your infra (live signals + your docs) | none | partial — within one vendor | static docs only | n/a |
| Citation contract | every answer cites sources, configurable threshold | rare / unreliable | sometimes | weak | n/a |
| Write operations | HITL approval workflow, role-based, audit-logged | none | inconsistent | none | manual |
| Data residency | self-hosted, zero egress, BR/US/EU/custom | vendor cloud only | vendor cloud only | self-hosted possible | n/a |
| Native ops integrations | K8s, Prometheus, Loki/Tempo, ArgoCD, GitHub, Confluence, PagerDuty | webhooks at best | within one stack | manual ingestion | n/a |
| Compliance posture | SOC 2 Type II, ISO 27001, GDPR, immutable audit | varies | varies | depends on builder | n/a |
| Deploy time | minutes (Helm chart) | n/a (SaaS only) | weeks (vendor onboarding) | months (internal project) | none |
| User data trains model | never | sometimes (per provider) | varies | depends | n/a |

---

## What we promise (the public-facing version)

1. **Every answer cites its source.** Configurable minimum threshold per tenant. No exceptions.
2. **Read-only operations execute in seconds.** No more pivoting through five dashboards.
3. **Write operations always require human approval.** No auto-remediation surprises.
4. **Your data never trains the model.** Self-hosted with zero data egress is first-class.
5. **Deploy in minutes, not weeks.** Helm chart, native Kubernetes, no professional-services lock-in.

---

## What we will not become

- We will never be a generic chat tool. The grounding *is* the product.
- We will not auto-execute write operations without human approval.
- We will not require data to leave the customer's perimeter.
- We will not return answers without cited sources.
- We will not become a customer-facing chatbot product. WeAura is internal-ops only.

---

## Audience-version positioning

> "WeAura is the AI that finally answers 'why is `payment-api` restarting?' in 1.4 seconds — with the metric, the deploy, and the runbook line cited. Self-hosted if you need it. Read-only by default. Writes need a human."

(Said in the audience's own technical vocabulary.)

---

## Investor-version positioning *(internal — never published in social)*

> _(to be added — investor framing not present on the public site; capture in a separate internal doc when raising or reporting.)_

DO NOT use any future investor framing in social content. Audience and investor framings stay distinct.

---

## Refresh log

| Date | What changed |
|---|---|
| 2026-05-06 | Initial brand pack derived from public site content at https://weaura.ai. |

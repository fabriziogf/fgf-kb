---
title: Engineering fluency for AI PMs
updated: 2026-08-03
sources:
  - source: study notes — engineering concepts and stakeholder collaboration
    added: 2026-08-03
    summary: systems, data, and ML lifecycle concepts a PM needs, plus how to work with engineers without doing their job
---

# Engineering fluency for AI PMs

The bar is **fluency, not expertise theater**. Engineers trust the PM who asks the right
question, not the one who pretends to know the answer. You own the why, the requirements,
and the trade-offs; they own the how. If asked something you don't know, say so and
reason about how you'd find out — that builds more trust than bluffing.

## Systems and infrastructure

- **API / contract** — the agreed interface: what you send, what you get back.
- **Latency vs. throughput** — time per request vs. requests handled per second.
- **p50 / p95 / p99** — tail latency is what users actually feel. For interactive experiences, p99 is the number that matters.
- **Synchronous vs. asynchronous** — wait now, or fire and handle the result later.
- **Caching** — a primary lever for both LLM cost and latency.
- **Scalability** — horizontal (more machines) vs. vertical (bigger machine).
- **SLA / SLO / SLI** — uptime commitments and the metrics behind them.
- **Idempotency** — a repeated action has the same effect as doing it once. Critical for safe retries on consequential actions, and it underpins reversibility.
- **Rate limiting** — protecting a system from overload or abuse.

## Data and pipelines

- **ETL / data pipeline** — move and transform data into usable form.
- **Batch vs. streaming** — scheduled chunks vs. continuous near-real-time. Streaming is what keeps user context fresh.
- **Feature store** — a managed place to compute, store, and serve model inputs consistently.
- **Data freshness / lag** — how current the data is when used. Stale context is a trust failure, not a latency one.
- **Online vs. offline** — serving in real time vs. computing in the background.
- **Data governance** — access patterns, PII handling, consent, lineage, retention. Internal platforms must solve this or every consumer solves it differently.

## ML lifecycle

- **Training vs. inference** — learning the model vs. using it at request time.
- **Model serving / endpoint** — hosting a model so applications can call it.
- **Prompting vs. fine-tuning vs. RAG** — see [[adapting-llms]].
- **Efficient adaptation (LoRA) and preference methods (RLHF, DPO)** — ways to adapt a model to a task or behavior.
- **Offline vs. online eval** — see [[offline-vs-online-eval]].
- **Model drift / monitoring** — performance degrading as the world changes.
- **Embeddings and vector search** — retrieval by meaning rather than keywords.
- **Context window / tokens** — cost and latency scale with it.

## Putting a model safely into production

Offline evaluation against a benchmark → guardrails and quality thresholds → staged
rollout with monitoring → drift detection → a rollback path. This sequence *is* a launch
readiness framework, and it's worth having one written down that applies across model
releases rather than negotiating it each time.

## Engineering process

- **Tech debt** — a tool, not a sin. Take it for speed or learning when it's reversible and tracked; pay it down when it slows the team or threatens reliability. Keep it visible on the roadmap, not hidden.
- **MVP** — the smallest thing that tests the riskiest assumption and is safe for users. **Cut scope, not quality bars.** Define what "good enough to learn" means up front.
- **Build vs. buy** — weigh control, cost, speed, and whether it's actually differentiating.
- **Observability** — logging, metrics, tracing. For agents this means tracing across every hop, with per-hop latency and cost.
- **Feature flags / canary rollout** — release to a slice, monitor, expand. The mechanism for expanding agent autonomy safely.
- **Postmortem** — blameless write-up to fix the system, not assign fault.

## Working with engineers

- **Write requirements they can use** — crisp problem and user outcome, constraints and explicit **non-goals**, success metrics, edge cases. Leave the how to them. Co-write the spec rather than tossing it over the wall.
- **When you hear "that's not possible" or "that's six months"** — get curious, not adversarial. Understand the real constraint, separate must-have from nice-to-have, look for a smaller first slice, re-scope together. Never overrule on feasibility.
- **Reliability and platform work are first-class roadmap items** with their own metrics and a protected allocation — tied to the leverage they unlock for other teams.
- **Explaining technical concepts to non-technical stakeholders** — use an analogy, drop the jargon, and tie it to the decision they care about.
- **Translate in both directions** — business goals into technical constraints, and technical reality into options and trade-offs. Be the person who removes ambiguity both ways.

See also [[platform-vs-feature-pm]], [[adapting-llms]], [[autonomy-and-handoff]].

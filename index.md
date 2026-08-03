---
title: Knowledge base index
updated: 2026-08-03
---

# Index

A working knowledge base on building and measuring AI products. Everything here is
generalized — concepts and frameworks, not company-specific material.

## AI agents

How agentic systems are built and where they fail.

- [Agent architecture fundamentals](topics/ai-agents/agent-architecture.md) — what makes something an agent, the ReAct loop, termination conditions, prompt injection
- [Memory and context management](topics/ai-agents/memory-and-context.md) — the four memory types, context as the primary bottleneck, memory freshness
- [Tool design and function calling](topics/ai-agents/tool-design.md) — schema design, routing, least privilege, schemas as API contracts
- [Orchestration patterns](topics/ai-agents/orchestration-patterns.md) — single-agent first; router, supervisor, and peer patterns
- [Guardrails and safety](topics/ai-agents/guardrails-and-safety.md) — the three layers, and when to require human approval
- [Cost, latency, and the capability frontier](topics/ai-agents/cost-latency-tradeoffs.md) — why cost grows super-linearly, and when a simpler architecture wins

## Evaluation

The hardest PM problem in agentic products.

- [Why agent eval is different](topics/ai-evaluation/eval-fundamentals.md) — the evaluation paradox, the three-layer model, attribution debugging
- [Agent quality metrics](topics/ai-evaluation/agent-quality-metrics.md) — step accuracy, golden traces, regression thresholds
- [Retrieval quality metrics](topics/ai-evaluation/retrieval-quality-metrics.md) — precision@k through NDCG, topical vs. contextual relevance
- [End-to-end experience quality](topics/ai-evaluation/e2e-experience-metrics.md) — task vs. goal completion, implicit and explicit signals
- [LLM-as-judge](topics/ai-evaluation/llm-as-judge.md) — calibration, the four known biases, why the rubric is the product
- [Offline vs. online evaluation](topics/ai-evaluation/offline-vs-online-eval.md) — what each catches, and the gap between them
- [Eval infrastructure as a product](topics/ai-evaluation/eval-infrastructure-as-product.md) — the six components, eval debt, eval as a moat

## Retrieval and context

- [RAG architecture](topics/retrieval-and-context/rag-architecture.md) — the pipeline, and where RAG breaks
- [Retrieval strategies and optimization](topics/retrieval-and-context/retrieval-strategies.md) — dense, sparse, hybrid, and six optimization techniques
- [Context engineering and prompt governance](topics/retrieval-and-context/context-engineering.md) — prompt anatomy, schema design, prompts as code
- [Three ways to adapt an LLM](topics/retrieval-and-context/adapting-llms.md) — prompting vs. RAG vs. fine-tuning, and the quality/latency/cost triad

## Platform PM

- [Platform PM vs. feature PM](topics/platform-pm/platform-vs-feature-pm.md) — developer empathy, alignment without authority, the paved road
- [Platform migration playbook](topics/platform-pm/platform-migration-playbook.md) — audit, contract, parallel run, sunset
- [Stakeholder management and resistance](topics/platform-pm/migration-stakeholders.md) — four sources of resistance and their distinct remedies
- [Prioritizing competing requests](topics/platform-pm/prioritization-under-constraint.md) — triage, scoring, batching, the reversibility test
- [Communicating decisions to leadership](topics/platform-pm/leadership-communication.md) — the template, audience calibration, the written summary
- [Engineering fluency for AI PMs](topics/platform-pm/engineering-fluency.md) — systems, data, and ML concepts, and how to work with engineers

## Measurement

- [Experimentation and causal inference](topics/measurement/experimentation-and-causal-inference.md) — what to do when you can't randomize
- [Metric design and traps](topics/measurement/metric-design-and-traps.md) — leading vs. lagging, Goodhart's Law, vanity metrics
- [Measuring AI support agents](topics/measurement/ai-support-metrics.md) — why containment must never be read alone
- [Measuring personalization](topics/measurement/personalization-measurement.md) — the context on-off test, and heterogeneous effects as proof

## AI product strategy

- [Agent autonomy and the human handoff](topics/ai-product-strategy/autonomy-and-handoff.md) — reversibility × confidence × risk; capability ≠ authority
- [Personalization patterns](topics/ai-product-strategy/personalization-patterns.md) — the four layers, signal selection, cold start
- [GenAI model strategy](topics/ai-product-strategy/genai-model-strategy.md) — provenance as a moat, first-party vs. partner, inference cost
- [Product case and strategy frameworks](topics/ai-product-strategy/product-case-frameworks.md) — case scaffolds, frameworks worth naming, presenting a case

## How this works

Notes are captured as images, dropped into `inbox/`, and incorporated by
[NoteKB](https://github.com/fabriziogf/noteKB). Nothing is merged automatically —
each incorporation lands as a pull request for review.

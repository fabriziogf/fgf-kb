---
title: Why evaluating agentic products is different
updated: 2026-08-03
sources:
  - source: study notes — evaluation framework for agentic products
    added: 2026-08-03
    summary: four properties that make agent eval hard, the evaluation paradox, the three-layer model, and the attribution debugging workflow
---

# Why evaluating agentic products is different

Evaluating traditional software is simple: write a test, define the expected output, run
it, pass or fail. Agentic evaluation is categorically harder — the system is
non-deterministic, outputs are long-form with no single right answer, failures are
silent, and quality depends on at least three independent subsystems that can each
degrade on their own.

The better analogy is a **clinical trial, not a unit test**. A drug does not fail by
crashing; it fails by producing the wrong outcome in the patient. Measuring that needs
study design, a control group, blinded evaluation, and statistics over time.

## Four properties that make it hard

- **Non-determinism** — the same input produces different outputs across runs. A single run is not evidence; you need statistical aggregation to separate real quality change from noise.
- **Silent failures** — no exceptions thrown. The agent produces a fluent, confident, wrong response. Detecting this needs a separate evaluation system: an LLM judge or a human.
- **Error accumulation** — in a multi-step loop, a wrong tool selection at step 2 contaminates steps 3–5. The final response can be wrong for a reason invisible in the final output. Evaluating only the final response misses this entirely.
- **No ground truth** — for most agent tasks there is no single correct answer. Evaluating quality requires a rubric, not a reference answer.

> **The evaluation paradox.** You need an eval framework before you ship in order to
> know when the agent is good enough — but you don't know the full distribution of
> failure modes until you've shipped. Resolve it in two phases: build a conservative
> **offline** framework pre-launch covering the failure modes you can anticipate, then
> invest heavily in **online** eval to discover the ones you couldn't. Most teams get
> phase one approximately right and severely underinvest in phase two.

## The three-layer model

The single most important eval concept is **decomposition**. When end-to-end quality
degrades, the natural but wrong response is to treat the system as a black box and try
to improve it overall. The right response is to measure each layer independently and
isolate where degradation actually is.

```
        End-to-end experience quality
   (task success, goal completion, satisfaction)
                    ↑ feeds into
   ┌────────────────┼────────────────┐
   Retrieval        Agent            UX
   quality          quality          quality
   precision@k,     step accuracy,   latency, clarity,
   recall, MRR,     tool selection,  guardrail accuracy,
   NDCG             plan validity,   trust signals
                    grounding
```

These are **independent failure modes**. Retrieval can fail while reasoning is sound
(garbage in, sound reasoning, garbage out). Reasoning can fail while retrieval is
excellent. Both can be fine while UX degrades — the right answer delivered too slowly,
formatted confusingly, or blocked by a guardrail false positive. Each combination needs
a different intervention.

> **The aggregate metric is the enemy of root cause analysis.** Most teams track one or
> two aggregate quality metrics. When those degrade, they can't tell whether the problem
> is retrieval, reasoning, or UX — so they guess and discover slowly. The difference
> between a team that diagnoses quality degradation in days and one that takes weeks is
> almost entirely the quality of their eval decomposition.

## Attribution debugging workflow

When end-to-end quality drops:

1. **Check retrieval** — run the retrieval eval suite independently (precision@k, recall, relevance). If poor, retrieval is the root cause; fix the pipeline or knowledge base.
2. **Check agent quality** — if retrieval is fine, run golden-trace eval (step accuracy, tool selection, grounding). If poor, fix prompts, model, or tool schemas.
3. **Investigate UX** — if both are fine, check latency, guardrail trigger rate, formatting, and trust signals.

## Layer ownership and signal latency

| Layer | Primary owner | Signal latency | Primary method |
|---|---|---|---|
| Retrieval quality | ML / Search | Low — measurable offline pre-ship | Automated metrics |
| Agent quality | ML + PM (shared) | Medium | Golden traces + LLM judge |
| End-to-end experience | PM | High — needs real traffic | Implicit + explicit production signals |

## The distribution shift problem

Even a well-designed framework can mislead. Golden datasets reflect the query
distribution at the time they were built. As user behavior evolves, production drifts
away from it — an agent can score 95% offline while performing poorly in production.
This isn't solvable, it's manageable: continuous golden-dataset refresh, production
distribution monitoring, and healthy distrust of offline metrics not validated against
production signal.

See also [[agent-quality-metrics]], [[retrieval-quality-metrics]],
[[e2e-experience-metrics]], [[offline-vs-online-eval]], [[llm-as-judge]].

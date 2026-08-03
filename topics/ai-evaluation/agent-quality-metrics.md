---
title: Agent quality — measuring the reasoning, not the answer
updated: 2026-08-03
sources:
  - source: study notes — evaluation framework for agentic products
    added: 2026-08-03
    summary: step accuracy and related metrics, golden trace datasets, regression detection thresholds
---

# Agent quality metrics

Agent quality measures the soundness of the **reasoning process** — independent of
whether the input information was good or the final response satisfied the user. It
answers: *given the information available, did the agent reason correctly?*

A math teacher doesn't only mark the final answer. A student who reaches the right answer
by an incorrect method hasn't demonstrated mastery — and will fail the next, slightly
different problem. A correct response reached through flawed reasoning is a **fragile**
correct response.

## Metrics

| Metric | What it measures | How measured |
|---|---|---|
| **Step accuracy rate** | % of individual reasoning steps that are correct and necessary | Golden trace dataset + LLM judge or deterministic checks on tool parameters |
| **Tool selection accuracy** | Did it pick the right tool for each subtask, given the available set? | Deterministic where a ground-truth tool exists; LLM judge for ambiguous cases |
| **Plan validity** | Does the overall approach make structural sense — logical, complete sequence? | LLM judge with a holistic rubric; needs human calibration |
| **Grounding accuracy** | When citing retrieved information, is the citation accurate? Did it actually use what it claims? | Compare claims against actual tool outputs. A source in the response that was never retrieved is a hallucination signal |
| **Hallucination rate** | % of responses containing claims not grounded in retrieved context or known facts | LLM judge with a faithfulness rubric, calibrated against human labels |
| **Unnecessary step rate** | % of steps that added no value — redundant calls, restating known information, dead ends | Measured from traces. A rising rate often signals prompt drift or degraded planning |

> **Step accuracy is a leading indicator; final response quality is lagging.**
> Teams evaluate only the final response because it's easier. But by the time flawed
> reasoning has produced a wrong final response, the user is already lost. Step-level
> evaluation is more expensive to build and it is the eval investment that catches
> problems earliest.

## Building a golden trace dataset

A golden trace is a complete labeled example of correct execution: the input query, each
reasoning step with its expected tool call and result, and the final response. It is the
backbone of agent quality evaluation.

1. Select a **representative** sample of query types.
2. Have domain experts label the correct reasoning path for each.
3. Validate labels for internal consistency.
4. Version the dataset so quality can be tracked against a fixed benchmark.

The hardest part is not the labeling — it's the representative sampling. A golden set
that over-represents easy queries gives inflated scores, and production is where the hard
queries live.

## Regression detection thresholds

A 1% drop on 100 examples is almost certainly noise. A 1% drop on 10,000 examples is
meaningful. Set the threshold based on:

- the **statistical power** of your evaluation dataset,
- the **severity** of the degradation (1% on hallucination rate matters more than 1% on unnecessary steps), and
- the **cost of false alarms** — teams stop reading alerts that fire too often.

Define these thresholds *before* launching the eval framework, not after the first
regression.

See also [[eval-fundamentals]], [[llm-as-judge]], [[retrieval-quality-metrics]].

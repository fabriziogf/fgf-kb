---
title: Retrieval quality metrics
updated: 2026-08-03
sources:
  - source: study notes — evaluation framework for agentic products
    added: 2026-08-03
    summary: precision@k, recall, MRR, NDCG, topical vs contextual relevance, and human relevance grading as calibration
---

# Retrieval quality metrics

Retrieval quality measures whether the agent's information-fetching surfaces the right
context. It is the foundation layer: given bad information, even perfect reasoning
produces bad output. The methodology borrows directly from decades of information
retrieval research.

The analogy: a research librarian who returns books on climate *science* when you asked
about climate *policy*, or policy books from the 1990s. Your reasoning can be flawless
and your paper will still be wrong.

## Metrics

| Metric | What it measures | Notes |
|---|---|---|
| **Precision@k** | Of the k documents retrieved, what fraction are relevant? | Needs relevance labels. The most intuitive metric; precision@5 is common where the agent reads the top 5 |
| **Recall** | Of all relevant documents that exist, what fraction were retrieved? | Requires knowing the full relevant set — expensive to label. Matters most when *missing* a document is catastrophic (a policy exception that overrides the standard answer) |
| **MRR** | On average, how highly ranked is the first relevant document? | Mean of (1 / rank of first relevant result). Useful when the agent reads in rank order |
| **NDCG** | Rewards placing highly relevant documents near the top, discounting relevance further down | Captures degree of relevance *and* position. The industry standard; more sophisticated than precision@k |
| **Contextual relevance** | Was the information situationally appropriate, not merely topically related? | Needs an LLM judge or human raters evaluating relevance *in context* |
| **Retrieval latency** | How long retrieval takes and its share of end-to-end latency | A highly accurate but slow retriever can produce a worse experience than a slightly less accurate fast one |

> **Topical vs. contextual relevance is where most retrieval systems fail.**
> A system can score well on a benchmark that tests *topical* relevance and fail badly
> in production, which demands *contextual* relevance. A user asks "can I cancel my
> booking?" Topically relevant: any article about cancellation policy. Contextually
> relevant: the specific policy applying to *this* booking type, date, and cancellation
> window. That gap is the most common source of confident wrong answers in agentic
> support systems.

Published production work in this area has converged on structured knowledge
representations that encode intent, context, and the action to take — rather than
retrieving prose documents and hoping the model extracts the right situational rule.

## Human relevance grading: the gold standard and its cost

Human grading is the most accurate method and the slowest. The practical approach is to
use it to **calibrate** automated metrics, then run the automated metric at scale:

1. Label 200–500 query/document pairs with human relevance scores.
2. Run your automated metric on the same pairs.
3. Measure agreement.

Without the calibration step you cannot trust your automated scores at all. Agreement of
80%+ with human labels is generally considered production-grade for retrieval evaluation.

See also [[eval-fundamentals]], [[retrieval-strategies]], [[llm-as-judge]].

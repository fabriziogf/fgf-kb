---
title: RAG architecture
updated: 2026-08-03
sources:
  - source: study notes — AI personalization and retrieval
    added: 2026-08-03
    summary: the RAG pipeline components, chunking through generation, and where RAG breaks
---

# RAG architecture

Retrieval-Augmented Generation is the dominant pattern for giving a model access to
external, dynamic, or private knowledge without fine-tuning.

## The pipeline

| Component | What it does | Key trade-off |
|---|---|---|
| **Chunking** | Splits source documents into segments for embedding | Fixed-size vs. semantic. Too small loses context; too large dilutes the relevance signal |
| **Embedding model** | Converts chunks into dense vectors | Dimension vs. speed vs. cost |
| **Vector store** | Stores embeddings for approximate nearest-neighbor search | Latency, cost, managed vs. self-hosted |
| **Retriever** | Embeds the query, fetches top-k similar chunks | k (how many chunks) and the similarity threshold |
| **Re-ranker** | Optional second pass: a cross-encoder re-scores retrieved chunks before injection | Accuracy gain vs. added latency |
| **Generator** | The model receiving retrieved context plus the query | Context size vs. cost and latency |

## Where RAG breaks

This is the part worth knowing cold, because it's where production failures come from:

- **Poor retrieval** — the right document was never fetched. Everything downstream is then wrong, no matter how good the reasoning.
- **Stale data** — the knowledge base is current as of six months ago. Produces confident, wrong, well-cited answers.
- **Context limits** — retrieved content exceeds the window and gets truncated, often silently.
- **Over-trusting retrieved-but-wrong content** — the model treats anything in context as authoritative. Retrieval precision is the guardrail here.

## Retrieval latency budget

Retrieval adds to total response time. For interactive support-style experiences,
keeping the retrieval step under a few hundred milliseconds is a common working target —
the exact number should come from your end-to-end latency SLA, working backwards.

See also [[retrieval-strategies]], [[retrieval-quality-metrics]], [[adapting-llms]], [[context-engineering]].

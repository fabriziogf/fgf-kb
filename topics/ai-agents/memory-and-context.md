---
title: Agent memory and context management
updated: 2026-08-03
sources:
  - source: study notes — agentic AI and platform migration
    added: 2026-08-03
    summary: the four memory types, context window management as a product problem, retrieval strategies, memory freshness
---

# Agent memory and context management

Memory determines what the agent knows at any moment. Getting it wrong is the single
most common cause of agent performance degradation as task complexity grows.

## The four memory types

| Type | What it is | Constraint |
|---|---|---|
| **In-context (working)** | Everything in the context window: system prompt, history, tool results, injected facts | Fast, immediately available, bounded. When it fills, content must be truncated or summarized — both lossy |
| **External / episodic** | Stored outside the model in a vector or key-value store, queried at runtime | Enables memory across sessions. Retrieval quality directly determines agent quality |
| **Semantic (knowledge base)** | Structured domain facts, retrieved via RAG | The challenge is keeping knowledge fresh and structured so the model can use it reliably |
| **Procedural** | How to do things — encoded in the system prompt and tool descriptions | **The PM's primary lever.** You own the system prompt, so you own the agent's procedural memory |

The human analogy holds: working memory is what you're actively thinking about,
episodic memory stores specific past events, semantic memory stores general facts,
procedural memory holds how to do things.

> **Context window management is the primary performance bottleneck.**
> As the loop adds tool results, every new piece of context competes with older
> context for the model's attention. Models weight recent and structurally
> prominent content more heavily, so early context can be effectively ignored even
> though it is technically present. What to include, summarize, and discard is a
> product design decision, not just an infrastructure one.

## The "lost in the middle" problem

Models have degraded attention to content in the middle of very long contexts. The
correct response is to **retrieve only the most relevant context at each step** — not
to maximally populate the window and trust the model to find what it needs.

## Retrieval strategies

- **Sparse (BM25)** — keyword matching. Fast and interpretable; misses synonyms and semantic relationships.
- **Dense (vector search)** — semantic similarity via embeddings. Captures meaning beyond keywords; can retrieve semantically similar but contextually wrong content.
- **Hybrid** — both, in sequence (dense retrieval then sparse re-ranking, or the reverse). Current best practice for production.

A common production shape: vector retrieval fetches a few dozen candidate documents,
then an LLM-based re-ranker selects the most relevant. Two-stage hybrid approaches of
this kind have reached >90% precision in published production systems.

See [[retrieval-strategies]] for the fuller treatment.

## Memory freshness: the silent degrader

External memory is only as good as it is current. A knowledge base accurate six months
ago may now hold outdated policies or deprecated schemas. Unlike a bug that crashes,
**stale memory produces confident incorrect responses** — far harder to detect.

Build a freshness SLA from day one: how often is the knowledge base updated, who owns
it, and how do you detect that staleness has degraded quality? This is PM work.

See also [[agent-architecture]], [[context-engineering]], [[retrieval-quality-metrics]].

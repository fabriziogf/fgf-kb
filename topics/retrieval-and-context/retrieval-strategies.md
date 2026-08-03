---
title: Retrieval strategies and optimization
updated: 2026-08-03
sources:
  - source: study notes — AI personalization and retrieval
    added: 2026-08-03
    summary: dense vs sparse vs hybrid retrieval and the six main retrieval optimization techniques
---

# Retrieval strategies and optimization

## Dense vs. sparse

| Dense (semantic) | Sparse (keyword / BM25) |
|---|---|
| Finds semantically similar content across different wording | Excels at exact-match lookup (policy numbers, IDs, error codes) |
| Requires an embedding model and vector store | No embedding needed; fast and interpretable |
| Can miss precise factual matches | Misses paraphrase and synonyms |
| Best for: "what policy applies to my situation?" | Best for: "find record #ABC123" |

**Hybrid combines both, and most production RAG systems are hybrid.** The two failure
modes are complementary, which is exactly why combining them works.

## Optimization techniques

- **Query rewriting** — the model expands or reformulates the query before retrieval to improve recall.
- **HyDE (hypothetical document embeddings)** — generate a hypothetical answer, embed *that*, and use it to retrieve. Works because a hypothetical answer is closer in embedding space to real answers than the question is.
- **MMR (maximal marginal relevance)** — balances relevance against diversity so you don't retrieve five near-identical chunks.
- **Contextual compression** — after retrieval, extract only the relevant sub-sections of each chunk. Cuts tokens without cutting information.
- **Metadata filtering** — pre-filter on structured fields (user tier, date range, document type) before semantic search. Often the cheapest large accuracy win.
- **Re-ranking** — a cross-encoder scores retrieved chunks before injection. More expensive per candidate, so it runs on a shortlist.

A common production shape: dense retrieval fetches a few dozen candidates, then a
re-ranker selects the handful that go into context. Two-stage approaches of this kind
have reached >90% precision in published production systems.

See also [[rag-architecture]], [[retrieval-quality-metrics]], [[memory-and-context]].

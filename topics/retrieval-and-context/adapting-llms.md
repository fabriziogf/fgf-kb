---
title: Three ways to adapt an LLM
updated: 2026-08-03
sources:
  - source: study notes — AI personalization and retrieval
    added: 2026-08-03
    summary: prompting vs RAG vs fine-tuning, when each applies, and the quality/latency/cost triad
---

# Three ways to adapt an LLM

| Approach | What it does | Best for |
|---|---|---|
| **Prompting / in-context learning** | Instructions, examples, and context in the prompt. No weight changes | Rapidly evolving use cases. Fastest to iterate; bounded by the context window |
| **RAG** | Augments the model with retrieved external knowledge at inference. No weight changes | Dynamic data, policy lookups, user-specific context. Keeps knowledge fresh without retraining |
| **Fine-tuning** | Updates model weights on domain data | Style, tone, domain vocabulary. Stable use cases with high call volume where per-call latency and cost matter. Expensive to iterate |

These are not mutually exclusive, and the usual production answer is a **hybrid**:
fine-tune for tone and domain style, RAG for facts and user-specific context. The
governing question is *how often does the knowledge change?* Anything that changes
faster than your retraining cycle belongs in retrieval, not in weights.

Efficient adaptation methods (LoRA and similar) and preference-based methods (RLHF, DPO)
reduce the cost of the fine-tuning path, but don't change the fundamental division: fast-
changing knowledge does not belong baked into weights.

## The quality / latency / cost triad

Every model decision is a point in this space, and the PM holds the pen on where to sit:

- **Quality vs. latency** — larger models are better and slower. Distillation and caching claw some back.
- **Quality vs. cost** — more context tokens improve accuracy and raise cost per call. Segment use cases rather than paying the maximum everywhere.
- **Latency vs. cost** — streaming responses improve *perceived* latency without reducing compute.

**Model routing** is the highest-leverage lever: send easy queries to a cheap model and
reserve the frontier model for hard ones. This is how large deployments run many models
cost-effectively rather than paying frontier prices on every call.

See also [[cost-latency-tradeoffs]], [[rag-architecture]], [[context-engineering]], [[genai-model-strategy]].

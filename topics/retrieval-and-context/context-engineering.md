---
title: Context engineering and prompt governance
updated: 2026-08-03
sources:
  - source: study notes — AI personalization and retrieval
    added: 2026-08-03
    summary: prompt anatomy, the more-vs-less context trade-off, schema design for context payloads, and treating prompts as code
---

# Context engineering and prompt governance

## Anatomy of a production prompt

- **System prompt** — persona, tone, scope, rules.
- **User context block** — structured payload: account state, entitlements, current session state, prior interactions, sentiment flags.
- **Knowledge context block** — retrieved policies, documents, precedents.
- **Few-shot examples** — 2–3 examples of ideal responses for this user type and issue category.
- **User message** — the actual request, verbatim or lightly cleaned.
- **Instruction suffix** — formatting requirements, length constraints, citation requirements.

## The context trade-off

| More context | Less context |
|---|---|
| Higher accuracy on complex, nuanced cases | Lower latency, lower cost per call |
| Risk of overload — model attends to the wrong parts | Risk of missing facts needed for an accurate answer |
| More tokens → more cost and latency | Demands higher retrieval precision (garbage in, garbage out) |
| Better for: high-stakes, escalation-prone cases | Better for: high-volume, routine queries |

**The PM's job is to segment use cases and design context payloads per segment.** It is
not one-size-fits-all.

## Schema design for context payloads

You define what fields go into the context object injected into every prompt:

- **Structured over unstructured** — JSON fields are parsed and cited more reliably than free-text paragraphs.
- **Relevance-ranked** — put critical context first; model attention degrades toward the middle and end.
- **Fresh over stale** — timestamp dynamic fields, and signal to the model when data may be outdated.
- **Compact** — every token costs latency and money. Strip boilerplate ruthlessly.

## Treat prompts as code

- **Version control and review** — prompts change behavior; they deserve the same review as code.
- **Prompt regression testing** — automated suites checking changes against golden outputs.
- **Shadow deployment** — run the new prompt in parallel before cutover; compare at scale.
- **Rollback triggers** — if a quality metric drops below threshold, auto-revert.

Prompts belong in editable files, not inline strings, so they can be iterated without
touching code.

> Prompts are the agent's **procedural memory** — the encoded knowledge of how to do
> things. Owning the system prompt means owning that memory. See [[memory-and-context]].

See also [[rag-architecture]], [[adapting-llms]], [[personalization-patterns]].

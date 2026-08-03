---
title: Personalization patterns for AI products
updated: 2026-08-03
sources:
  - source: study notes — AI personalization
    added: 2026-08-03
    summary: the four layers of full-stack personalization, support vs feed personalization, signal selection, and cold start
---

# Personalization patterns for AI products

## The four layers

"Full-stack personalization" spans:

1. **Signal layer** — collecting and enriching user context: history, preferences, in-flight state.
2. **Storage / schema layer** — how user state is persisted, versioned, and made queryable.
3. **Retrieval layer** — surfacing the right context at inference time.
4. **Generation layer** — injecting that context into prompts and shaping responses.

Most teams are strong at one or two and treat the rest as someone else's problem. The
platform version of this work is making all four coherent and reusable.

## Support-style vs. feed-style personalization

A distinction worth being precise about, because the engineering looks similar and the
product requirements are not:

| Feed / recommendation | Support / task completion |
|---|---|
| Optimize for engagement | Optimize for resolution accuracy |
| Failure = irrelevant content | Failure = wrong advice, eroded trust, escalation |
| Tolerates probabilistic, soft personalization | Often requires **hard, factual** context (account state, entitlements) |
| Context = tastes and history | Context = case history, account status, emotional state |
| Freshness matters, seconds are fine | Real-time context is often critical |

The consequence: a support-style system cannot treat retrieved context as a soft
suggestion. Getting the wrong booking, wrong policy, or wrong account state isn't a
slightly worse recommendation — it's a confident wrong answer.

## Signal selection

The signals that actually move outcomes tend to be:

- **Tier / status signals** — affects tone and policy latitude.
- **Prior interaction history** — did this person escalate last time, and what resolved it?
- **Current transaction context** — dates, state, payment status.
- **In-session signals** — what they typed, where they are in a flow.
- **Sentiment inference** — language cues indicating frustration, urgency, or vulnerability.
- **Locale / cultural signals** — language preference, norms around directness.

Run **ablation experiments** to find which fields actually drive quality. Most context
payloads carry fields that cost tokens and change nothing; you only find them by removing
them one at a time and measuring.

## Cold start

Thin signals for new or low-activity users. Three approaches, usually combined:

- **Segment-level defaults** — infer from what you do know about the category.
- **Progressive enrichment** — personalization improves as the session accumulates context.
- **Intent-based fallback** — lean on in-session signals when history is absent.

## The platform reframe

The highest-leverage move in this space is usually to stop treating context as *richer
input for one answer* and start treating it as **a shared context service that every
agent and surface reads from**. That reframe — feature to platform — is what turns a
one-time quality improvement into compounding leverage, and it is the argument you have
to make explicitly, because it costs more up front.

The dependency to be honest about: a shared context layer's value rests on the claim that
downstream consumers will actually use it. Size that claim rather than asserting it.

See also [[context-engineering]], [[personalization-measurement]], [[autonomy-and-handoff]], [[platform-vs-feature-pm]].

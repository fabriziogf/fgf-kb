---
title: End-to-end experience quality
updated: 2026-08-03
sources:
  - source: study notes — evaluation framework for agentic products
    added: 2026-08-03
    summary: task vs goal completion, implicit and explicit signals, and why implicit signals need a causal interpretation layer
---

# End-to-end experience quality

This is the only layer reflecting real user value. Retrieval and agent quality are
leading indicators — proxies that help you understand and improve the system. End-to-end
quality is the lagging indicator that tells you whether it actually works for users. It
requires real production traffic, so it has the highest signal latency and the highest
strategic importance.

## Task completion vs. goal completion

A support agent who reads every question, follows every procedure, and gives a
technically accurate answer has **completed the task**. If the customer hangs up still
unable to solve their problem, **goal completion** failed.

- **Task completion rate** — did the agent technically finish: produce a response, not error out, stay within guardrails? Necessary but not sufficient.
- **Goal completion rate** — did the user accomplish what they needed? Measured by repeat contact, downstream engagement, and explicit post-interaction ratings.

Optimizing task completion while ignoring goal completion is one of the most common
quality failures in production agentic systems.

## Implicit signals — the most scalable quality signal

Collected automatically at scale; reflect actual user judgment rather than survey
responses.

| Signal | What it measures |
|---|---|
| **Downstream engagement** | Did the user take the action the agent recommended? Did they play the recommendation, complete the booking, act on the guidance? |
| **Repeat contact rate** | Did the user come back about the same issue within N days? The gold-standard implicit signal for support agents — a high rate means the task was completed but the problem wasn't solved |
| **Session abandonment rate** | What % abandoned mid-session without resolution? Segment by step count to find *where* in the loop users drop |
| **Escalation rate** | What % required a human? A leading indicator of failure. Track by query category — a rise in one category often localizes a specific retrieval or reasoning failure |
| **Time-to-resolution** | First message to resolution; compare against the human baseline |

## Explicit signals — high quality, low volume

- **Thumbs up/down** — low friction, but binary and recency-biased: users rate the last thing said, not overall quality.
- **Post-session ratings** — higher signal, much lower completion. Rates below 5% are common; segment completers to check for sampling bias.
- **"Was this helpful?"** — placed right after a resolution attempt. Better response rates; best as a leading indicator, not a definitive score.
- **Explicit negative feedback / flagging** — rare but high-signal. Review every one manually, especially early in a product's life.

> **Implicit signals beat explicit signals at scale — but they need interpretation.**
> "User acted on the recommendation within 30 seconds" is more reliable than a
> thumbs-up because it measures behavior, not stated preference. But an implicit signal
> requires a **theory of causality**: you need confidence that the signal is caused by
> agent quality and not a confound. Building that causal interpretation layer — via A/B
> testing and counterfactual reasoning — is the part most teams underinvest in.

See also [[eval-fundamentals]], [[ai-support-metrics]], [[experimentation-and-causal-inference]], [[metric-design-and-traps]].

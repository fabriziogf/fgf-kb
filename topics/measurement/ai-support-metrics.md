---
title: Measuring AI support and conversational agents
updated: 2026-08-03
sources:
  - source: study notes — analytics and measurement
    added: 2026-08-03
    summary: the standard support metric set, why containment must never be read alone, and measuring correctness vs closure
---

# Measuring AI support and conversational agents

## The metric set

| Metric | What it is | Why it matters |
|---|---|---|
| **Self-solve / containment rate** | Share of contacts the agent resolves with no human | The core efficiency metric — but it can rise while quality drops. **Never read it alone** |
| **Deflection rate** | Share of contacts kept from reaching a human | A cost metric, easy to celebrate and easy to game. Can climb as trust falls |
| **Re-contact rate** | Same person returns about the same issue | The stickiness check: did the resolution hold, or did they give up in the moment? The single best trust guardrail |
| **First-contact resolution** | Solved in one interaction | Classic quality signal; high FCR usually means the answer was right and complete |
| **Time-to-resolution / handle time** | How long to close | Speed and cost matter, but fast ≠ correct. Always pair with quality |
| **Wrong-action / false-action rate** | How often the agent takes an incorrect action | The key trust guardrail as autonomy grows. One bad action can outweigh many good ones |
| **CSAT** | Post-contact satisfaction survey | Captures felt experience; response bias and recency make it noisy and gameable |
| **Cost per resolution** | Fully loaded cost to resolve | The business case, and the number reported upward — which is exactly why it needs a trust guardrail beside it |

## Correctness is not closure

Closure metrics tell you the contact ended. They do not tell you the answer was right.
Measure correctness separately:

- **Grounding / citation accuracy** — is the answer backed by a real, relevant source? This *gates the claim*.
- **Correctness** — was it the right outcome for this specific person? A well-cited answer can still be the wrong action. Grounding and correctness must be measured and gated separately.
- **Human-labeled samples** — calibrated review with a rubric, dual-labeled. Route genuinely contested two-sided cases to humans rather than auto-scoring them.
- **Downstream action** — did they do the thing the resolution pointed them to?

## Catching failures at scale without reviewing everything

- Grounding checks on **every** answer (cheap, automated).
- LLM-as-judge on a **sample**, audited by humans.
- Anomaly detection on action patterns.
- Tight, near-total monitoring on **irreversible** actions specifically.

The principle: sample smart, gate the risky. Review intensity should scale with
reversibility, not with volume.

## Seeing trust erosion under a good containment number

Track re-contact, escalation-after-bot, downstream complaints and cancellations, and the
retention of contained users against a holdout. If containment is rising while any of
these move the wrong way, the efficiency gain is being paid for out of trust.

## Multi-turn success

Single-reply metrics miss the conversation. Measure session-level resolution,
turns-to-resolution, abandonment, sentiment trajectory, and **whether the user had to
repeat themselves**. Outcome over per-message.

## Defining "wrong action" without bias

Define it *narrowly* so it can be labeled consistently: a policy violation, a factually
unsupported claim, or an irreversible action taken without consent. Broad definitions
("unhelpful") can't be dual-labeled reliably and turn the metric into noise.

See also [[e2e-experience-metrics]], [[metric-design-and-traps]], [[autonomy-and-handoff]].

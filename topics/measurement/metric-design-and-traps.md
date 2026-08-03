---
title: Metric design and traps
updated: 2026-08-03
sources:
  - source: study notes — analytics and measurement
    added: 2026-08-03
    summary: leading vs lagging indicators, pre-registered decision rules, Goodhart's Law, attribution, and avoiding vanity metrics
---

# Metric design and traps

## Leading vs. lagging indicators

The true outcome is usually slow and noisy — retention, repeat purchase, long-term trust.
You still need to show progress inside a quarter, which means pairing the lagging truth
with fast leading reads that plausibly cause it.

State the causal chain explicitly. If you can't say why the leading indicator should move
the lagging one, it's a proxy you're hoping about, not a leading indicator.

## Pre-registered decision rules

Ship / kill / iterate criteria set **before** the test runs. This is what prevents reading
the result you wanted into the data. If a plan has metrics but no explicit gates, the
gates will be invented after the fact to match the outcome.

## Goodhart's Law

*When a measure becomes a target, it stops being a good measure.* This is not an
abstraction in AI products — it's the dominant failure mode:

- **Containment/deflection as a goal** — rises while trust falls. The system gets better at ending conversations, not at solving problems.
- **Over-optimized citation accuracy** — verbose, over-cited, unhelpful responses.
- **Coverage masking quality** — injecting user context that doesn't actually improve the response, because coverage is the number being watched.

**Mitigation:** pair every primary metric with a guardrail that moves in the opposite
direction when the metric is being gamed, and keep a qualitative review cadence — human
sampling of real outputs — running alongside the dashboard.

## Avoiding vanity metrics

Start from the **user behavior the feature should change**, pick the outcome that
captures it, then pair it with a guardrail. Vanity metrics (raw clicks, surface
engagement) rise without the real outcome moving. Name both the leading indicator and the
lagging truth.

## Attribution

Correctly assigning an outcome to its cause is the hard part of linking any intervention
to a downstream business result. A matched holdout is usually the cleanest available
answer. See [[experimentation-and-causal-inference]].

## Structuring a metric set

A workable hierarchy for an AI product:

- **Primary** — the scarce outcome that justifies building the thing. Usually lagging.
- **Guardrails** — watch, don't optimize. Must not degrade: quality, trust, latency, error rates. A win that moves the primary while breaking a guardrail is not a win.
- **Build-health / diagnostic** — the fast-moving inputs that tell you whether the machine is working: coverage, retrieval precision, context utilization, token usage.

The hierarchy should be visually and verbally unmistakable when you present it. If a
reader can't tell which number you'd ship on and which you'd only ever defend, the metric
set isn't finished.

See also [[e2e-experience-metrics]], [[ai-support-metrics]], [[personalization-measurement]].

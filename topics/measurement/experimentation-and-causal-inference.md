---
title: Experimentation and causal inference
updated: 2026-08-03
sources:
  - source: study notes — analytics and measurement
    added: 2026-08-03
    summary: methods for when you cannot run a clean A/B test, core statistical concepts, and what makes A/B testing AI systems different
---

# Experimentation and causal inference

## When you can't run a clean A/B test

Reasons you can't randomize: ethics (no degraded experience during a real crisis),
network effects, shared-system interference, and small populations. The alternatives:

- **Matched holdout** — build a control by matching treated units to similar untreated ones on observed traits. The cleanest fallback for measuring an intervention's downstream effect.
- **Difference-in-differences** — compare the change over time in a treated group against an untreated one. Gives a causal-style read from observational data.
- **Switchback / time-split** — alternate treatment on and off over time across the whole system. Handles marketplace and shared-system effects where splitting users would leak between arms.

## Concepts that determine whether a test can answer the question

- **Interference (SUTVA violation)** — one unit's treatment affects another's outcome. Breaks a normal A/B: in a marketplace, treating some buyers moves sellers and contaminates the control.
- **Confounding** — a third variable drives both treatment and outcome. The core reason correlation isn't causation, and what randomization and matching exist to defeat.
- **Novelty and primacy effects** — behavior shifts because something is *new*, not because it's better or worse. Short experiments read high or low for the wrong reason; let effects settle.
- **Minimum detectable effect (MDE)** — the smallest effect the test can reliably catch at a given sample and power. Tie it to **the smallest effect worth acting on**, then size for that. Refuse underpowered tests.
- **Statistical significance** — how likely the result would be if there were no true effect. Guards against calling noise a win; says nothing about magnitude or importance.
- **Type I / Type II error** — false positive vs. missing a real effect. Every test trades these off.
- **Multiple comparisons** — testing many metrics or slices inflates false positives. Pre-register one primary metric or correct for it, or you will find a fake winner.

## What makes A/B testing AI systems different

- **Unit of randomization** — user vs. session vs. individual interaction. Each creates different spillover risks.
- **Treatment definition** — unlike a UI change, the treatment is often a model or prompt version. Rollout control and versioning discipline matter more.
- **Novelty effects** — users behave differently because the response is different, not better.
- **Measurement lag** — resolution quality may not be observable immediately; some issues resurface days later.
- **Covariate shift** — user mix changes over time (seasonality, new features). Holdout calibration is needed.

## Reading a result

When a metric moves: **check instrumentation first** — bug before insight. Then
significance and the confidence interval, then segment to localize, then rule out
seasonality and confounds, then confirm guardrails held.

A null is information. Separate *truly flat* from *underpowered*, segment for
heterogeneous effects, and form the next hypothesis. Don't p-hack.

## Designing so the experiment changes a decision

Agree the decision and the decision rule **before** running. Pre-commit owners to act on
each outcome. Report results tied to that action. Design for the decision, not the report.

See also [[metric-design-and-traps]], [[personalization-measurement]], [[e2e-experience-metrics]].

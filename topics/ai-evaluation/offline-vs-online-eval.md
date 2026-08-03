---
title: Offline vs. online evaluation
updated: 2026-08-03
sources:
  - source: study notes — evaluation framework for agentic products
    added: 2026-08-03
    summary: what each catches and misses, the gap between them as a warning signal, and the right eval posture per deployment phase
---

# Offline vs. online evaluation

Not alternatives — two phases of one measurement system, on different timescales,
catching different classes of failure. A mature framework uses both and manages the gap
between them deliberately.

## What each catches and misses

| | Offline | Online |
|---|---|---|
| **Catches** | Regressions on known failure modes in the golden dataset; effects of prompt, model, or tool schema changes | Distribution shift — real queries users ask right now; production-specific failures; UX issues needing real behavior |
| **Misses** | New failure modes not in the dataset; distribution shift; production failures (rate limits, latency spikes) | Fast feedback — signal takes days to weeks; low-volume failure modes affecting <1% of queries |
| **Tools** | Golden trace datasets, LLM judge pipeline, held-out test sets, regression detection | A/B testing, canary releases, production monitoring, shadow mode comparison |

> **The gap between offline and online is where quality surprises live.**
> The most dangerous failure is offline green, online degraded. It happens when the
> offline dataset is stale, the failure mode isn't represented, or production
> conditions differ from the eval environment. Track the **correlation** between
> offline and online metrics over time. A widening gap — offline improving while online
> is flat or declining — is a clear signal to redirect eval investment toward
> refreshing the golden dataset and improving production monitoring.

## The right posture by deployment phase

1. **Pre-launch** — offline is your primary tool. Golden traces, LLM judge, automated regression checks on every candidate build. Set a minimum offline threshold as a launch gate, and be honest about what the dataset cannot cover.
2. **Canary / shadow mode** — first exposure to real production data. Run the new system in shadow before cutting over; compare shadow responses against current production on matched query pairs.
3. **Graduated rollout** — online eval comes alive as implicit signals accumulate. Define rollback triggers *before* starting. Track the offline/online gap from day one of rollout.
4. **Full production** — continuous monitoring with both. Refresh the golden dataset quarterly. Regression checks on every deploy. Feed production failures back into the golden dataset to close the distribution gap over time.

See also [[eval-fundamentals]], [[eval-infrastructure-as-product]], [[platform-migration-playbook]].

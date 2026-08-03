---
title: Measuring personalization
updated: 2026-08-03
sources:
  - source: study notes — analytics and measurement
    added: 2026-08-03
    summary: the context on-off test, metrics that only move if personalization works, and heterogeneous effects as proof
---

# Measuring personalization

The hard question about any personalization investment is not "did metrics improve?" but
**"did personalization specifically cause it, or did the automation just get better?"**
Generic quality metrics cannot distinguish those two. These can.

## The context on-off test

Hold the system constant — same agent, same actions, same model — and toggle the user
context on versus off, matched on task type. **The lift between arms is personalization's
value.** This is the single strongest design and it answers most of the challenge on its
own.

Its failure modes, which you should name before someone else does:

- **Contamination across arms** — shared state or caching leaking context into the off arm.
- **Low power** — personalization effects are often small; size for the MDE deliberately.
- **Novelty effects** — a different-feeling experience reads as better before it settles.
- **Coverage differences confounding the read** — if the on arm also has richer coverage, you're measuring two things.

## Metrics that only move if personalization works

| Metric | Why it's specific |
|---|---|
| **Personalization coverage** | Share of interactions where the system had full user context. An input metric only personalization moves, and the denominator for any conditioned lift |
| **Lift conditioned on coverage** | Compare outcomes on high-context vs. low-context interactions. Direct evidence the context is doing work; the gap should widen as coverage grows |
| **Re-establish rate** | How often the user still has to repeat who they are or what they're dealing with. Arguably the most honest personalization metric — it is the pain personalization exists to remove, and it should fall |
| **Right-action-for-this-user** | Whether the action taken was correct for *this* person, not merely a resolved interaction |

**Incrementality** — the causal value added over what would have happened anyway —
separates "personalization caused it" from "they would have come back regardless." A
matched holdout is what measures it.

## Heterogeneous treatment effects as proof

If personalization is real, **gains should concentrate where context is richest** — a
returning user mid-journey should benefit more than a first-timer with no history.

This is the sharpest available test. Segment lift by context coverage and richness:

- **Concentrated gains** → personalization is genuinely doing the work.
- **Even gains across all segments** → you probably just built better automation and attributed it to personalization.

## When the north-star outcome is noisy

Long-horizon outcomes (repeat purchase, retention over 30–60 days) are the scarce results
that justify building, and they're swamped by base-rate variation. Handle it by conceding
the noise rather than defending the metric:

- Use a **matched holdout** and report **incremental**, not absolute, effects.
- Scope the read to **high-volume, reversible** interaction types where you have power.
- Lean on faster leading indicators (re-contact, re-establish rate) to show progress before the lagging metric reads out.

See also [[experimentation-and-causal-inference]], [[metric-design-and-traps]], [[personalization-patterns]].

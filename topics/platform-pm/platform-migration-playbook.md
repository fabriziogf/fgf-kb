---
title: Platform migration playbook
updated: 2026-08-03
sources:
  - source: study notes — agentic AI and platform migration
    added: 2026-08-03
    summary: why migrations happen, the four phases (audit, contract, parallel run, sunset), and the metrics that keep one accountable
---

# Platform migration playbook

## Why migrations happen

The root cause of most is **runtime fragmentation**: teams build their own
implementations before a shared platform exists, and you end up with N independent
versions of the same core patterns. The migration is the work of consolidating them.

Four root causes:

- **Runtime fragmentation** — five teams, five approaches to schemas, context management, eval, and observability.
- **Technical debt accumulation** — early shortcuts (hardcoded prompts, ad hoc retrieval, no eval) become load-bearing and expensive.
- **Capability gaps** — a new capability needs to reach all teams, but each custom implementation would need independent upgrade.
- **Architectural misalignment** — original assumptions no longer hold (a single-agent design that can't scale to multi-agent; an internal-only schema that now needs external developers).

> **Migrations fail socially before they fail technically.** The common failure isn't
> incompatibility — it's resistance from teams rationally optimizing for their own
> roadmaps. Teams have deadlines; migration is extra work. Without a compelling reason,
> they de-prioritize, drag, and find reasons their use case is "special." A migration
> strategy that ignores human dynamics isn't a strategy, it's a wish.

**Make the cost-of-not-migrating case concrete.** Not every fragmentation justifies a
migration. If each team maintaining its own implementation burns 2 engineers of redundant
work per quarter, and the migration costs 20 engineer-weeks, break-even is 10 quarters —
possibly too long to justify. Quantified, not moral.

---

## Phase 1: Audit

The most underinvested phase of almost every migration. Teams rush past it to start
building.

1. **Implementation inventory** — map each team's current setup: runtime, tools, models, context management, eval. Via interviews and code review, *not* documentation, which is usually stale.
2. **Gap classification** — per team, classify against the new contract: capability gaps, schema gaps, eval gaps, operational gaps.
3. **Dependency mapping** — if Team A's sub-agent is called by Team B's orchestrator, A migrates first. Dependencies define sequencing.
4. **Risk ranking** — high production traffic, complex custom implementations, tight roadmaps, and genuinely unusual use cases.

> **The audit is a trust-building exercise, not just data gathering.** You're asking
> teams to reveal technical debt they aren't proud of. They'll be defensive if they fear
> judgment or a timeline that ignores their reality. Frame it as "help us understand
> what you need so we can build a platform that works for you," not "we're checking how
> far you are from our target." The trust built here is the cooperation you need later.

**Outputs:** a team-by-team implementation map, a dependency graph, a list of platform
capability gaps to close before specific teams can migrate, a set of representative use
cases the platform must handle, and a migration complexity score per team.

---

## Phase 2: Contract definition

The most important artifact. The formal specification of the interface between platform
and consumers. Without it you cannot run a parallel migration, cannot say what "done"
means, and cannot hold the platform accountable for regressions.

The contract is a **building code**: individual architects have freedom within it, but it
defines the floor.

Five components:

- **Input schema** — what callers must provide: user context, session context, task description, pre-fetched signals.
- **Output schema** — the response, structured metadata (confidence, sources, step trace), status codes. Lets teams build without coupling to internals.
- **Error contract** — what errors can be raised and what callers should do. Distinguishing retryable / non-retryable / recoverable is critical for resilient integrations.
- **Observability hooks** — what telemetry the platform exposes and what teams must emit: request IDs for trace correlation, per-step latency, tool success/failure, eval metric submissions.
- **SLAs and obligations** — latency (p50/p95/p99), availability, throughput. And the reciprocal: teams stay within token budgets and don't bypass rate limiting.

> **Write the contract before writing the first line of platform code.** It seems
> backwards, but contract negotiation is what surfaces the real constraints. Presenting
> a draft and asking "can you build to this?" tells you immediately what's missing,
> underspecified, or infeasible — before implementation, saving months of rework. A
> contract written *after* the platform reflects what was built, not what teams need.

**Breaking vs. non-breaking:**

```
NON-BREAKING (no consumer changes needed)
  - adding an optional output field
  - improving reasoning quality on existing tasks
  - adding a new error type (callers ignore unknown errors)
  - increasing an SLA guarantee

BREAKING (requires a versioned migration)
  - renaming or removing an input/output field
  - changing the semantics of an existing field
  - adding a required input field
  - changing how errors are structured or typed
  - removing a tool from the registry
  - tightening an SLA
```

---

## Phase 3: Parallel run

Run old and new side by side, comparing outputs on identical inputs, before replacing the
old. Same logic as a dual-read database migration: write to both, read from the old,
switch reads only once consistency is confirmed.

1. **Shadow mode (0% new traffic)** — all production traffic goes to the old platform; a copy also goes to the new one. Responses are captured and compared, but users only see the old. No user impact from new-platform failures. Run until parity metrics hold.
2. **Canary (1–5%)** — a small share is handled by the new platform and served to real users. Monitor for user-facing regressions. Define the parity threshold *before* this step.
3. **Graduated rollout (10% → 50% → 100%)** — with monitoring and automatic rollback triggers at each stage. Each stage runs long enough for meaningful signal — not 24 hours for a feature with weekly usage patterns.

**Parity metrics:** task completion rate, quality score (LLM judge, calibrated against
human labels), latency p50/p95, cost per request, and safety metrics (guardrail trigger
rate — a higher rate means either better enforcement or, more concerning, more false
positives).

> **Parity thresholds are a product negotiation, not a technical calculation.** In
> theory it's a measurement. In practice: the platform team is incentivized to set a low
> bar and finish faster; consuming teams are incentivized to set a high bar and delay.
> The PM who owns the migration must set a threshold that is *objectively defensible* —
> grounded in the user impact of the quality difference — not one that satisfies the
> path of least resistance.

**Rollback planning** is not low confidence, it's engineering maturity. Before the
parallel run starts, specify: the metrics that trigger automatic rollback, the time
horizon they're evaluated over, who can trigger a manual rollback, and what happens to
in-flight requests. Making rollbacks safe and unstigmatized is the PM's job.

---

## Phase 4: Sunset and forcing functions

Where migrations go to die. After a successful parallel run, the temptation is to declare
victory and let teams migrate on their own schedule. Without a hard deadline and credible
enforcement, the last 20% never migrate and you maintain two systems indefinitely — worse
than not having migrated.

A sunset date is a **lease expiry**: both parties know it in advance and plan around it.

**Setting a credible date:**

- In writing, early — not after the parallel run. Teams need at least one planning cycle between announcement and sunset.
- Based on realistic migration complexity, not on how fast you want it done. A missed date destroys credibility for the next migration.
- With leadership alignment *before* announcement. A date leadership will override under pressure is a suggestion, not a deadline.
- Concrete about consequences: old endpoints return errors. Soft shutdowns (degraded responses) create an incentive to stay forever.

**The carrot:** capability unlock (ship at least one high-value capability exclusive to
the new platform before the window opens — if the answer to "what can we do now that we
couldn't?" is "nothing yet," no one moves early), operational simplification, migration
tooling (schema converters, comparison dashboards, worked examples), and white-glove
support for the hardest teams.

> **The last-mile problem consumes disproportionate resources.** 80% of teams migrate
> smoothly; 20% consume 80% of the effort. They're last because their use case is
> genuinely unusual, their team is under real roadmap pressure, or there's institutional
> resistance. **Each needs a different intervention** — diagnosing which before
> deploying resources is the senior skill. Throwing engineering support at an
> institutional resistance problem does not work; that one needs a leadership
> conversation.

---

## Success metrics

**Adoption:** team migration rate (weighted by traffic, tracked weekly — a declining rate
is the early warning to activate the forcing function), traffic share, time-to-migration
per team (calibrates estimates and tells you whether tooling investment is working), and
support escalation rate.

**Platform quality:** task completion rate (at or above the old platform — a regression
here is the most critical negative signal), quality score against the parity threshold
tracked *by team and task type* (a regression in one category disappears in an
aggregate), latency against SLA, and eval regression rate per deploy.

**Operational risk:** rollback rate (more than 1–2 per month means insufficient
pre-production eval), schema drift incidents, incident time-to-detection, and old-platform
dependency (teams re-creating dependencies after migrating signals the new platform is
missing something).

> **Document the migration as a reusable playbook.** Every migration generates hard-won
> knowledge — friction points, effective interventions, what the parity threshold should
> have been, what documentation gaps cost in support hours. It evaporates unless
> captured. A retrospective written as a playbook makes the next migration faster and
> cheaper. That's building organizational capability, not just completing a task.

See also [[migration-stakeholders]], [[platform-vs-feature-pm]], [[tool-design]], [[offline-vs-online-eval]].

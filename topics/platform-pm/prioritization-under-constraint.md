---
title: Prioritizing competing platform requests
updated: 2026-08-03
sources:
  - source: study notes — product strategy and prioritization
    added: 2026-08-03
    summary: triage before scoring, the four scoring dimensions, the batching move, trade-off framing, and the reversibility test
---

# Prioritizing competing platform requests

The setup: several teams want something from your platform, there's a hard deadline, and
you can't do everything. This looks like a prioritization problem. Prioritization is
maybe 30% of it — the rest is making trade-offs explicit and communicating a decision to
people who won't all be happy.

> **The process of the decision matters as much as the decision.** A clear framework,
> honest engagement with trade-offs, and a real plan for the teams who didn't get what
> they wanted beats a confident announcement with no visible reasoning — even if the
> confident call was the same one.

## Never start with the scoring framework

The most common mistake is jumping to a matrix before framing the problem. Pulling out a
scoring rubric first signals pattern-matching to a memorized framework rather than
thinking about the actual situation.

**Five questions before scoring anything:**

1. **What type is each request?** Triage into a category first — category determines urgency independent of business value.
2. **What are the dependencies?** If Team C's migration needs a capability Team A also requested, that changes sequencing. Map dependencies before scoring, or you'll build a plan that is sequentially impossible.
3. **What is realistic capacity?** Account for planning, review, QA, and the fires that will happen. For most platform teams, **two to four meaningful deliverables in six weeks** is realistic.
4. **Are there batching opportunities?** Can one investment address several requests? Often the highest-leverage option, and invisible when requests arrive independently.
5. **What is the consequence of deferral, per team?** "Defer" is not neutral. For some it blocks a roadmap; for others it delays a nice-to-have. Assess qualitatively through individual conversations first.

## Triage categories

| Level | Label | Meaning |
|---|---|---|
| **P0** | Blocking | Team cannot ship or migrate without it. Must be addressed this cycle regardless of other priorities |
| **P1** | Accelerating | Team can ship, but this meaningfully accelerates them or removes real friction |
| **P2** | Strategic | Important for long-term platform health. No immediate urgency, compounding value |
| **P3** | Opportunistic | Nice to have. Deferred without significant cost. Acknowledge and queue, don't schedule |

## The four scoring dimensions

Each scored 1–3:

- **Platform leverage** — 1: only the requester benefits. 2: two or three teams. 3: four or more, or a foundational capability others build on.
- **Delivery confidence** — 1: scope unclear, dependencies unknown, no prior art. 2: scope understood, some unknowns. 3: well-scoped, prior art exists.
- **Strategic alignment** — 1: tangential to the architectural direction. 2: somewhat aligned, not on the critical path. 3: directly advances the core goal — the investment you'd make anyway.
- **Cost of delay** — 1: they have a workaround. 2: slowed but not stopped; creates debt or friction. 3: blocks an entire roadmap or compounds over time.

Multiply leverage × confidence × alignment for a base score, weight by cost of delay. The
numbers matter less than the relative ordering and the conversation.

> **Platform prioritization is multiplier math, not feature math.** Feature PMs ask "how
> much does Team A benefit?" Platform PMs ask "how many teams benefit, and by how much?"
> A capability that unblocks three teams almost always beats a bespoke feature for one,
> even when the bespoke feature scores higher on that one team's impact metric. Name the
> multiplier explicitly.

> **The numbers aren't the answer — the disagreements are.** If everyone agrees on every
> score, the framework hasn't done its job. The valuable moments are where the PM says
> delivery confidence is 2 and the engineer says 1. That disagreement surfaces a real
> uncertainty that needs resolving before the investment. Present the framework as a
> tool for generating productive disagreement, not as a calculator.

## The batching move

Before finalizing, spend real time looking for places where one investment serves two or
more requests. This is usually the highest-leverage option on the board and is almost
never visible when teams submit independently. The signal: when two requests both require
new infrastructure, ask whether it's the *same* infrastructure. If so, building it once
serves both better than two bespoke solutions — and changes the delivery math enough that
both can land inside the window.

## Making trade-offs explicit

For every request you're not doing, structure the thinking around three questions: what do
we gain by deferring, what do we lose, and what risk are we accepting?

**Three trade-off types:**

- **Speed vs. quality** — ship fast and unblock now, or take longer and create less debt. *The reversibility test: if you ship the fast version, how hard is it to migrate to the better one later? If "very hard," the fast version isn't actually faster.*
- **Breadth vs. depth** — serve all teams partially at MVP quality, or one or two completely. Breadth is usually right for platforms when the shared need is genuine, but MVP quality on a shared component creates a support burden that depth doesn't.
- **Short-term vs. long-term** — fix the immediate blocker, or invest in preventing the next one. Short-term choices that create structural debt are sometimes correct — but only when you name the debt and commit to paying it next cycle.

> **The reversibility test is the most underused decision tool.** Before finalizing, ask:
> if this is wrong in six weeks, how reversible is it? Easily reversible decisions
> deserve less deliberation. Deferring a breaking schema change is reversible. *Shipping*
> a breaking schema change without a versioning strategy is not — it creates migration
> obligations for every consumer immediately. That asymmetry should explicitly change how
> much confidence you require before each call.

## Managing under a hard deadline

A deadline is a **decision constraint**, not a delivery constraint. Engineering capacity
determines what you can build; the deadline just prevents you from deferring the
prioritization conversation until you have more information.

The wrong question is "what can we finish?" — it optimizes for completion and produces a
list of local optima that leave overall capability unchanged. The right question is **"what
creates the most compounding value?"** — which may mean shipping a foundational shared
component that isn't fully polished but unblocks everyone and makes the next cycle faster.

Build in explicit margin: plans using 100% of available engineering time fail. **80%
utilization is a plan that ships.**

And if the process reveals that even the top priorities can't be done at acceptable
quality in the window, surface it immediately. The correct action is not to compress
timelines or lower quality, but to escalate with a recommendation: scope this to an MVP
covering the core use case, defer the full implementation.

> Never let a deadline create quality debt you can't pay back. A rushed breaking change
> with inadequate versioning creates migration obligations that take months to unwind.

## Handling the teams that didn't get what they wanted

Meet individually **before** any group announcement:

1. **Understand the real concern under the stated request.** Most stated requests are solutions, not problems. "We need a new schema" may mean "our retrieval quality is degraded and we think the schema is the cause." The underlying problem sometimes has a cheaper solution.
2. **Ask: if we can't do this, what's your fallback?** This surfaces flexibility you'd never find in a group meeting. "There is no fallback, we're blocked" changes the urgency. "We can work around it but it costs us X" gives you a concrete cost-of-delay number.
3. **Share where their request sits, and why, before the announcement.** No one should learn they're deferred in a meeting where they can't ask questions.
4. **Give every deferred team a specific path forward.** "Deferred" is the worst version. "Deferred to next cycle; here's what would move it up" respects their roadmap and keeps the relationship.

**Four archetypes:** the *blocked team* whose P0 was deferred (explore partial delivery;
if impossible, escalate as a capacity problem rather than papering over it as a
prioritization decision), the *disappointed but cooperative team* (direct conversation,
honest criteria, written timeline commitment, follow up in four weeks), the
*institutionally resistant team* (a leadership conversation, not a roadmap one), and the
*team that disagrees with the criteria* (engage the specific disagreement — if they can
show you weighted a dimension wrong, update the scoring; if not, acknowledge the
disagreement rather than pretending it away).

> **Disagree-and-commit is a tool, not a last resort.** Name the distinction rather than
> papering over it: "I understand you still have reservations and I don't want to pretend
> otherwise. What I'm asking for is your commitment to execute in this direction this
> cycle, and I'm committing that if we see the outcome you predicted, we revisit." A team
> given that framing executes far more faithfully than one told the decision is right and
> left feeling dismissed.

See also [[leadership-communication]], [[migration-stakeholders]], [[platform-vs-feature-pm]].

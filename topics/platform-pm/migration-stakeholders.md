---
title: Stakeholder management and resistance
updated: 2026-08-03
sources:
  - source: study notes — agentic AI and platform migration
    added: 2026-08-03
    summary: the four sources of resistance and their distinct remedies, plus the communication cadence for a multi-month migration
---

# Stakeholder management and resistance

Platform work requires cooperation from teams you have no authority over. Managing those
relationships across a multi-month migration is the hardest and most differentiating part
of the role.

> **You are building a coalition, not winning a vote.** A migration isn't decided by
> majority — you need near-unanimous cooperation, because the old platform can't be shut
> down until the *last* team migrates. So the strategy can't be "convince most and force
> the rest." It's "build a coalition, address concerns individually, and reserve the
> forcing function for genuine holdouts."

## Diagnose the source of resistance before intervening

| Source | What's actually happening | The remedy |
|---|---|---|
| **Roadmap pressure** (most common) | They want to migrate but can't prioritize it against current commitments | Help them find the work in their existing roadmap, offer engineering support in their next free sprint, and shrink the migration through better tooling |
| **Technical concern** | They believe the platform can't support their use case or will degrade quality | Take it seriously. Either demonstrate it's invalid with parallel-run evidence, or fix the real capability gap |
| **Institutional resistance** | A lead has decided it's the wrong investment — not a capacity problem | Escalate to their leadership with a clear cost-of-non-migration framing. Requires pre-aligned executive sponsorship |
| **Legitimate exception** | Their use case genuinely can't be served by the platform as it stands | Decide whether to accommodate it, set a timeline for platform support, and negotiate a conditional migration commitment |

Applying the wrong remedy is worse than doing nothing: engineering support aimed at an
institutional resistance problem burns the resource and confirms the holdout's view that
the migration team doesn't understand them.

> **Build a feedback channel and act visibly on what you hear.** The most effective
> communication strategy is two-way: teams surface concerns, get fast answers, and see
> their feedback change the platform. Saying "Team X raised concern Y, so we changed the
> contract" signals that cooperation produces results. Teams that feel heard cooperate;
> teams that feel steamrolled find creative ways to delay.

## Communication cadence

1. **Announcement** — at the start of a planning cycle. Include rationale, timeline, available support, and the escalation path for concerns. Never announce without a support offer.
2. **Monthly progress review** — share publicly which teams have migrated, are in progress, and are pending. Visibility creates positive peer pressure without individual call-outs.
3. **Bilateral check-ins** — meet individually with leads who haven't started after month one. These conversations surface the real reasons for delay.
4. **Pre-sunset escalation** — six weeks out, a formal escalation to teams not started or not finished, including the consequence of missing the date and an offer of emergency support.

See also [[platform-migration-playbook]], [[prioritization-under-constraint]], [[leadership-communication]].

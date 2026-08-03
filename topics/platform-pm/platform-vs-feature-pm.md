---
title: Platform PM vs. feature PM
updated: 2026-08-03
sources:
  - source: study notes — platform PM and agent platform strategy
    added: 2026-08-03
    summary: the core distinctions, platform metrics, developer empathy as a discipline, and alignment without authority
---

# Platform PM vs. feature PM

| Feature PM | Platform PM |
|---|---|
| Builds experiences for end users | Builds infrastructure for internal teams and downstream products |
| Success = engagement, conversion | Success = other teams shipping faster and better |
| One primary customer | Multiple customers with conflicting needs |
| API contracts internal and implicit | API contracts explicit, versioned; breaking changes are costly |
| Roadmap = feature pipeline | Roadmap = capabilities, abstractions, extensibility, scalability |

## Platform metrics

Adoption rate, integration velocity (time-to-integration for a new team), p99 latency,
error rates, infra cost per query, and the number of internal teams actually building on
the platform. **Adoption and abandonment are leading indicators** — they move before any
satisfaction survey registers a problem.

## Developer empathy as a product discipline

Internal engineers are among the most demanding and least vocal user groups. They will
not file a support ticket — they will work around your platform or fork it.

- **Shadow the on-call.** Spend a rotation with the engineers who get paged when the platform breaks. Four hours there beats four months of user interviews.
- **Watch adoption curves, not NPS.** When a team adopts and usage plateaus or drops, that's a signal: a capability gap, or onboarding that broke down.
- **Documentation is developer UX.** If engineers read your source to understand the platform, the docs have failed. Treat doc quality as a launch criterion.
- **The golden path principle.** For any common use case, make the obvious path also the correct path. Don't rely on engineers reading the fine print — design so that default behavior is best-practice behavior.
- **Advocate in resourcing conversations.** "This saves engineering X hours per sprint across Y teams" is a real business case. Internal users have no seat at the resourcing table; you are their voice.

## Alignment without authority

Platform PMs rarely have authority over the teams they serve, so alignment *is* the
product. Three mechanisms that work:

- **Shared language** — a common vocabulary for quality, eval, and migrations, so everyone argues from the same definitions.
- **Shared metrics** — when teams see the same dashboard, disagreements become empirical instead of political.
- **Transparent decision-making** — teams align more readily when they understand the criteria, even when they disagree with the outcome.

## Making ambiguity visible

Ambiguity is most dangerous when invisible — when teams hold different mental models of
who owns what. Make it visible, then reduce it: a RACI on every major component as a
forcing function for the alignment conversation, explicit "provisional owner"
declarations when ownership is contested, and a written decision log teams can reference
when context is lost in transitions.

## Being opinionated

Platforms succeed by being opinionated. Know how to say no to one-off requests while
serving 80% of use cases — a **paved road** that most teams take willingly, without the
platform becoming a bottleneck for the rest.

See also [[prioritization-under-constraint]], [[platform-migration-playbook]], [[eval-infrastructure-as-product]].

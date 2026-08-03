---
title: Single-agent and multi-agent orchestration patterns
updated: 2026-08-03
sources:
  - source: study notes — agentic AI and platform migration
    added: 2026-08-03
    summary: when single-agent is right, the router/supervisor/peer patterns, and why pattern choice is a product architecture decision
---

# Orchestration patterns

## Start single-agent

One LLM, one reasoning loop, one tool set, one context window, handling the task end to
end. For many use cases this is correct — reaching for multi-agent complexity before you
need it is one of the most common design mistakes.

**Single-agent works well when:**

- The task fits in a single context window.
- Steps are sequential, each depending on the last.
- The domain is coherent — deep knowledge in one area beats shallow knowledge across many.
- Latency requirements tolerate sequential tool calls.
- The failure blast radius is contained to the current task.

**The natural boundary** is context: every tool result and reasoning step adds tokens.
Eventually the system must truncate (losing information), summarize (lossy), or fail.
Tasks requiring large information accumulation are where single-agent becomes inadequate.

> **Start single-agent, justify the upgrade.** Engineering bias runs toward multi-agent
> because it is technically exciting. The PM bias should run the other way: ship
> single-agent, run it in production, and let observed failure modes justify the
> architectural upgrade. Multi-agent systems carry dramatically higher complexity,
> observability cost, and failure surface. Every multi-agent design should trace to a
> specific observed limitation of the single-agent approach it replaces. "We thought we
> might need it" is not justification.

## The three multi-agent patterns

### 1. Router

A lightweight classifier sits at the front and dispatches each request to one specialist
agent. Specialists never interact.

- **Best for** — requests that fall into distinct, well-defined categories mapping cleanly to distinct capabilities.
- **Watch out** — edge cases between categories get forced into a poor fit. Routing errors are *silent*: the failure shows up as response quality, not an error log.

> **The router is a product taxonomy problem.** Every router instantiates a claim:
> these are the categories of request we handle, and this is how they map to
> capabilities. As the product evolves, categories need to be added, split, or merged.
> That is PM work.

### 2. Supervisor / orchestrator

A central orchestrator decomposes the task, dispatches sub-tasks to specialized
sub-agents (often in parallel), and synthesizes the results.

- **Best for** — complex tasks requiring multiple kinds of expertise in combination. Multi-agent research systems commonly use this: decompose a question, fan out parallel search sub-agents, synthesize findings.
- **Watch out** — the orchestrator is a single point of failure and a latency bottleneck. If its decomposition is wrong, all sub-agent work is wasted. Observability requires tracing across the orchestrator and every sub-agent at once.
- **Parallelism is the main advantage** — a research task needing ten searches takes ~10× longer under a single sequential agent. But the benefit only accrues on *independent* sub-problems; where sub-task B needs A's result, parallelism buys little.

### 3. Peer / mesh

Agents invoke each other directly with no central coordinator.

- **Best for** — genuinely emergent workflows where the sequence of invocations cannot be determined at design time. Rarely the right first choice.
- **Watch out** — circular invocations cause infinite loops, debugging means tracing an arbitrary graph, and cost is unpredictable. Adopt only after the supervisor pattern has been ruled out by demonstrated need.

> **Pattern choice is a product architecture decision.** Router says: our requests are
> categorically distinct. Supervisor says: our tasks have internal structure that
> benefits from specialization and parallelism. Peer says: our tasks are emergent and
> cannot be pre-structured. Those are product assumptions. Be able to articulate why
> the chosen pattern matches the actual shape of your tasks — and what evidence would
> justify migrating to a more complex one.

See also [[agent-architecture]], [[cost-latency-tradeoffs]], [[platform-vs-feature-pm]].

---
title: Cost, latency, and the capability frontier
updated: 2026-08-03
sources:
  - source: study notes — agentic AI and platform migration
    added: 2026-08-03
    summary: why agent cost grows super-linearly, optimization levers, and when a simpler architecture is strategically superior
---

# Cost, latency, and the capability frontier

Agentic systems are fundamentally more expensive and slower than single-call LLM
systems. A system with 10 reasoning steps and 10 tool calls does not cost 10× a single
call — it can cost 30×, because each step's output is included in every subsequent
step's context.

## Where the cost comes from

- **Input token growth** — at step N, context holds the original prompt plus all N−1 prior reasoning steps and tool results. This grows **quadratically** with step count.
- **Output token cost** — every reasoning step generates chain-of-thought text before the tool call. Output tokens typically cost 2–5× input tokens per unit. Verbose reasoning is often higher quality *and* more expensive, creating a cost-quality tension that has to be calibrated deliberately.

> **The capability-cost-latency frontier is your product design space.**
> The most sophisticated agent is not always the right agent. A 20-step multi-agent
> system at $0.50 and 30 seconds per task may be objectively more capable than a
> 3-step agent at $0.02 and 3 seconds — but if the user is waiting synchronously, the
> latency *is* the product failure. The right architecture delivers sufficient quality
> within the latency and cost envelope the product context allows. "Sufficient" is a
> product definition, not an engineering one: you set the quality bar, engineering
> finds the architecture that hits it inside the constraints.

## Optimization levers

- **Model routing** — a smaller, cheaper model for simple reasoning steps; the frontier model reserved for complex synthesis.
- **Caching** — cache deterministic tool calls with identical parameters.
- **Step budgeting by task type** — lower limits for simple tasks so the agent cannot over-reason on easy problems.
- **Context compression** — summarize accumulated context periodically instead of letting it grow unbounded. Accept the quality trade-off consciously.
- **Prompt optimization** — shorter, denser system prompts. A 500-token reduction is trivial per call and significant at millions of calls per day.

## When a simpler architecture wins

Sometimes the right answer to "should we use an agent?" is **no**. A prompt-chaining
pipeline — a fixed sequence of LLM calls, each with a predefined role — is more
predictable, cheaper, easier to debug, and easier to evaluate than a dynamic agentic
loop, whenever the steps are known in advance.

Agents earn their cost when:

- the number and type of steps cannot be determined ahead of time,
- the task benefits from self-correction based on observed results, or
- different subtasks need different tools a fixed pipeline cannot accommodate.

Every other case is a candidate for something simpler.

See also [[orchestration-patterns]], [[agent-architecture]], [[adapting-llms]].

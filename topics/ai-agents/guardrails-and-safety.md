---
title: Guardrails and safety for agentic systems
updated: 2026-08-03
sources:
  - source: study notes — agentic AI and platform migration
    added: 2026-08-03
    summary: the three guardrail layers, PM ownership of guardrail definition, and when to require human approval
---

# Guardrails and safety

In traditional software, bugs cause crashes — visible failures caught in testing. In
agentic systems, failures are usually invisible: a fluent, confident, plausible output
that is factually wrong, harmful, or policy-violating. Guardrails are the defense
against that class of silent failure.

The right analogy is the compliance layer in a trading system: hard limits — position
limits, risk parameters, regulatory rules — that the trader cannot override even when
convinced the trade is correct. Guardrails are constraints the agent's own reasoning
cannot override.

## The three layers

**1. Input guardrails** (before the model sees anything)
Applied to the user's message. Check for jailbreak attempts, prompt injection embedded
in user input, PII that should not reach the model, and policy-violating content.
Fast and cheap — typically rule-based or a lightweight classifier.

**2. Intermediate guardrails** (in-loop, before tool execution)
Applied to tool selections and parameters before they execute. Check for actions outside
the agent's authorized scope, parameter values violating business rules (a refund above
a threshold), and call sequences that suggest a runaway loop.

**3. Output guardrails** (before the user sees the response)
Applied to the final response. Check for hallucinated claims, policy violations,
citation accuracy, and PII that should not be surfaced. Typically an LLM-based judge or
a structured classifier.

> **The PM owns the guardrail definition, not just the implementation.**
> What counts as harmful, out-of-scope, or a policy breach is a product decision.
> Engineering implements enforcement; the PM defines the boundaries. That requires a
> taxonomy of violation types, a calibrated threshold per type, and a process for
> updating the taxonomy as product and policy evolve. Defined once at the platform
> level, guardrails prevent each product team from implementing them inconsistently —
> or not at all.

## Human-in-the-loop: when to require approval

The spectrum runs **fully automated** (agent acts without review) → **assisted** (agent
proposes, human approves) → **human-led** (human acts, agent assists).

Fully automate only when accuracy is high enough that the cost of wrong automated
actions is lower than the cost of human review. For **high-consequence, low-volume**
actions — large refunds, account terminations, dispute resolutions — human-in-the-loop is
almost always correct until the agent's accuracy *on those specific action types* is
demonstrably very high. Aggregate accuracy is not the relevant number.

The related decision framework — how to throttle autonomy as an agent earns trust — is in
[[autonomy-and-handoff]].

See also [[agent-architecture]], [[tool-design]], [[eval-fundamentals]].

---
title: Tool design and function calling
updated: 2026-08-03
sources:
  - source: study notes — agentic AI and platform migration
    added: 2026-08-03
    summary: how function calling works, schema design, deterministic vs learned routing, least privilege, schemas as API contracts
---

# Tool design and function calling

Tools are what separate an agent from a chatbot. Every meaningful action an agent takes
is mediated by a tool, which makes tool design one of the highest-leverage decisions in
agent architecture — and squarely in the PM's domain, because tools define the contract
between the platform and the teams building on it.

Think of the tool set as the agent's **job description**: here is what you can do, here
are the systems you can reach, here are the rules. A confusing job description produces
confused work.

## How function calling works

1. **Schema registration** — each tool is defined by a JSON schema: name, natural-language description, typed parameters with their own descriptions. Passed to the model as part of system context.
2. **Tool selection** — during reasoning the model reads the schemas and selects the best fit, generating the tool name and parameter values as structured output.
3. **Execution** — the runtime validates parameters against the schema and executes the function.
4. **Result injection** — the return value or error is formatted into context as an observation. The agent reasons again.

## Schema design

```jsonc
// POOR — ambiguous, unhelpful
{
  "name": "search",
  "description": "Search for things",
  "parameters": { "q": { "type": "string" } }
}

// GOOD — specific, self-documenting, says when NOT to use it
{
  "name": "search_help_articles",
  "description": "Search Help Center articles by semantic similarity.
    Use when the user has a policy or procedure question.
    Do NOT use for booking lookup or reservation status.",
  "parameters": {
    "query": {
      "type": "string",
      "description": "Natural language description of the user's issue.
        Be specific — include intent and context."
    },
    "max_results": { "type": "integer", "description": "Default 5.", "default": 5 }
  }
}
```

The negative instruction ("do NOT use for…") does as much work as the positive one.

> **Tool schemas are API contracts — treat every change as a migration event.**
> Renaming a parameter, adding a required field, or changing a description in a way
> that alters behavior can break every prompt downstream. Schema versioning deserves
> the same discipline as API versioning: define breaking vs. non-breaking changes,
> set deprecation windows, and name who updates downstream agents when schemas
> evolve. On a shared agent platform, a schema change is a migration event for every
> product team building on it.

## Deterministic vs. learned routing

| | How it works | Failure mode | Good for |
|---|---|---|---|
| **Deterministic** | Rules-based dispatch: condition X → tool Y | Brittleness — edge cases fall through | Well-understood, high-volume, low-ambiguity tasks |
| **Learned** | Model reads all tool descriptions and picks | Unpredictability — selections vary between calls | Edge cases and novel task types |

Most production systems are hybrid: deterministic routing for common high-confidence
cases, learned routing for the rest.

## Least privilege is a quality principle, not just security

An agent should have only the tools its specific task requires. An agent with 50 tools
makes **worse** selections than one with 10 well-chosen tools, because the model must
reason over a larger, noisier action space. Tool access should be role-based and
context-specific: a support agent handling a lost-item report should not have write
access to financial records, even if the same platform serves agents that do.

See also [[agent-architecture]], [[guardrails-and-safety]], [[platform-migration-playbook]].

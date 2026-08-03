---
title: LLM-as-judge
updated: 2026-08-03
sources:
  - source: study notes — evaluation framework for agentic products
    added: 2026-08-03
    summary: how to build and calibrate an LLM judge, its four known biases, and why the rubric matters more than the judge model
---

# LLM-as-judge

Using a separate model to evaluate the outputs of the model under test. It is the closest
thing to a universal answer for the "no ground truth" problem: it scales to thousands of
evaluations a day, assesses multi-dimensional quality with a structured rubric, and can
be calibrated to approximate human judgment.

It is also subject to systematic biases that are poorly understood and routinely
underestimated.

The analogy: a senior engineer reviewing a junior's code. Knowledgeable, consistent,
capable of judging several quality dimensions at once — and also carrying stylistic
preferences, favoring approaches like their own, and going easier on familiar styles.

## How to build one

1. **Define a rubric** — what you're evaluating and how. Relevance might score 1–5 with specific criteria per level. Reasoning quality might score plan validity, tool selection, and grounding *separately*. The quality of your evaluations is entirely determined by the quality of your rubric.
2. **Design the judge prompt** — give it the rubric, query, context, and output, and ask for a structured score. Include few-shot examples of high, medium, and low quality with their correct scores. This is the single most impactful investment.
3. **Calibrate against human labels** — label 200–500 examples with human scores, run the judge on the same, measure agreement (Cohen's kappa or Spearman correlation). **70%+ is generally production-grade. Below 60%, redesign the rubric** before trusting the judge.
4. **Run at scale** — then track agreement over time. Drift means the rubric or judge model needs recalibration.

## Known failure modes

| Bias | What happens | Mitigation |
|---|---|---|
| **Position bias** | Prefers the response presented first in a pairwise comparison | Evaluate both orderings and average, or score each response independently |
| **Verbosity bias** | Rates longer, more detailed responses higher even when brevity is better | Make length an explicit rubric criterion, with examples of concise responses scored highly |
| **Self-serving bias** | A judge from the same model family as the agent may prefer its own family's outputs | Use a judge from a different family, or ensure calibration data covers both |
| **Rubric drift** | If the judge model updates, its interpretation of the rubric shifts, breaking historical comparisons | Pin the judge model version; update deliberately with a re-calibration run |

> **The rubric is the product — not the judge model.**
> Teams spend real effort choosing a judge model and miss the higher-leverage
> investment. A mediocre judge with an excellent rubric beats an excellent judge with a
> vague one, every time. The rubric is the codification of your quality theory: what
> "good" means for this product, this task type, this user population. Writing one
> forces PM and ML to sit down and agree on what quality means *before* they have data.
> That conversation is hard, valuable, and the PM's job to drive.

## What makes a rubric work

**Good:** specific (each score level has concrete criteria, not "somewhat helpful"),
calibrated (levels are evenly spaced in quality — a 3 is meaningfully better than a 2),
multi-dimensional (accuracy, completeness, tone, and policy compliance scored
separately, not collapsed), and validated (human-labeled examples at each level that the
judge reliably reproduces).

**Bad:** "rate the overall quality from 1–5." Vague, inconsistently spaced,
single-dimensional. Most teams ship this and then wonder why the pipeline is unreliable.

See also [[eval-fundamentals]], [[agent-quality-metrics]], [[eval-infrastructure-as-product]].

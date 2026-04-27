---
type: workflow
id: cross-provider-review-pipeline
title: Cross-Provider Technical Review
description: "Draft with one AI model, critique with another, revise until the reviewer passes — better output through model diversity"
tags: [Production, Customer-Facing, Content, Writing, Loop, Cross-Provider]
connections:
  - target: technical-drafting
    type: uses
  - target: adversarial-review
    type: uses
  - target: language-polish
    type: uses
  - target: consistency-check
    type: uses
  - target: llm-service
    type: runs_on
metadata:
  estimated_duration: "5-15 minutes"
  trigger: manual
  loop_modes: ["until_pass"]
loops:
  - id: "draft-review"
    mode: "until_pass"
    steps:
      - "technical-drafting"
      - "adversarial-review"
    verifier: "adversarial-review"
    maxIterations: 4
    freshContextPerIteration: true
output_step: "language-polish"
composite_steps:
  - "technical-drafting"
  - "adversarial-review"
  - "language-polish"
  - "consistency-check"
execution:
  - skill: "technical-drafting"
    step_type: "generation"
    prompt: "draft-technical-content"
  - skill: "adversarial-review"
    step_type: "validation"
    prompt: "review-adversarially"
  - skill: "language-polish"
    prompt: "polish-language"
    step_type: "content"
  - parallel:
    - skill: "consistency-check"
      prompt: "check-consistency"
      step_type: "review"
---

## Overview

This workflow demonstrates **cross-provider review** — the practice of using different AI models for different roles in the same pipeline. One model drafts, a different model critiques, the first model revises based on that feedback, and the cycle repeats until the reviewer passes.

The result is better than either model produces alone. Different models have different blind spots, different strengths, and different perspectives. Using them adversarially — one creates, one challenges — produces output that neither would produce in isolation.

## Why Cross-Provider Produces Better Output

When one model reviews its own work, it tends to confirm what it already wrote. It has the same biases, the same blind spots, the same patterns. Cross-provider review breaks this:

- **Different training data** means different perspectives on what constitutes good technical writing
- **Adversarial tension** forces the drafter to actually address criticism rather than dismissing it
- **Iteration history** shows concrete improvement — you can see exactly what the reviewer caught and how the drafter fixed it

## How to Set Up Cross-Provider

In the execution setup, each step shows a provider selector. Assign different providers:

| Step | Recommended | Why |
|------|-------------|-----|
| **Technical Drafting** | Claude | Strong at structured, thorough technical writing |
| **Adversarial Review** | GPT-4 | Different perspective, catches different blind spots |
| **Language Polish** | Either | Surface-level editing, less model-sensitive |

The key is that the drafter and reviewer are different models. Which specific models you use matters less than the fact that they're different.

## Pipeline Stages

### Stage 1: Technical Drafting (Loop)

**Input:** Topic, target audience, key points, word count

Invoke the **technical-drafting** skill via the **draft-technical-content** prompt. On the first iteration, produces a complete draft. On subsequent iterations, revises based on the adversarial review's feedback.

If a Voice Profile is set, the draft matches the creator's writing style.

**Output:** Complete technical blog post.

### Stage 2: Adversarial Review (Loop Verifier)

**Input:** The draft from Stage 1

Invoke the **adversarial-review** skill via the **review-adversarially** prompt. Critiques the draft across five dimensions: argument structure, technical accuracy, completeness, clarity, and evidence quality.

Returns a structured verdict:
- **pass** — the draft is strong enough for final polish
- **continue** — specific issues need addressing, with detailed revision instructions

If the verdict is "continue", the loop returns to Stage 1 with the review feedback. The drafting model addresses each point in its revision.

**Output:** Structured review with issues, strengths, and verdict.

### Stage 3: Language Polish

**Input:** The approved draft

Invoke **language-polish** to clean up grammar, spelling, and sentence clarity. If a Voice Profile is set, the polish respects the creator's vocabulary and banned words.

**Output:** Polished, publication-ready post.

### Stage 4: Consistency Check (Parallel)

Invoke **consistency-check** to verify terminology and internal coherence throughout the post.

## What to Look For in the Results

After a run, check the execution history:

1. **Iteration count** — how many draft-review cycles it took. More iterations usually means the first draft had significant issues that were systematically addressed.
2. **Review feedback** — expand the adversarial review steps to see exactly what was caught. Compare early reviews (many issues) to later reviews (fewer, minor issues).
3. **Draft evolution** — compare the first draft to the final version. The differences show the concrete value of cross-provider review.

## Error Handling

- If the loop reaches maxIterations (4) without passing, the best draft proceeds to polish. The review feedback from the last iteration is available in the execution history.
- If no second provider is configured, the workflow still runs — but with the same model for both steps, losing the adversarial benefit. A note in the output recommends configuring a second provider.

## Inputs

| Name | Required | Description | Example |
|------|----------|-------------|---------|
| `{{input.topic}}` | Yes | The technical topic | `Why most database migration strategies fail at scale` |
| `{{input.target_audience}}` | Yes | Who this is for | `Senior backend engineers and engineering managers` |
| `{{input.key_points}}` | No | Specific points to include | `1. Dev-safe vs prod-safe migrations. 2. ALTER TABLE locking.` |
| `{{input.word_count}}` | No | Target word count | `1500` |

## Outputs

| Name | Description |
|------|-------------|
| Technical blog post | Publication-ready post that has survived adversarial review |
| Review history | Per-iteration review feedback showing what was caught and fixed |

## Setup

1. Configure at least two AI providers in Settings (e.g., Claude + GPT-4, or Claude + Gemini).
2. Import this workflow.
3. In the execution setup, assign different providers to the drafting and review steps.
4. Optionally, set up a Voice Profile for personalised output.

## Provider Notes

- **Drafting** benefits from the strongest model available — it's doing creative, structural work.
- **Review** works well with any capable model — the value comes from the different perspective, not raw capability.
- The loop runs 1-4 iterations. Budget for 2-3 iterations on average.
- Total token usage is higher than single-model workflows — the quality improvement justifies the cost.

## Example Input

```
Topic: "Why most database migration strategies fail at scale — and what to do instead"
Target Audience: "Senior backend engineers and engineering managers who've dealt with production migration incidents. They know SQL, they've used migration tools, but they may not have thought deeply about the failure modes at scale."
Key Points: "1. The difference between dev-safe and prod-safe migrations — what works on your 50-row dev database can lock a 400M-row production table. 2. Why ALTER TABLE is dangerous at scale and what alternatives exist (pt-online-schema-change, gh-ost, blue-green). 3. The case for separating schema changes from deploy pipelines entirely. 4. Load-testing migrations against production-scale data."
Word Count: "1500"
```

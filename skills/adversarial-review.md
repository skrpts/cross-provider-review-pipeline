---
type: skill
id: adversarial-review
title: Adversarial Review
description: "Critiques technical content from a different perspective — the review side of the cross-provider loop"
tags: [Production, Review, Quality, Loop]
connections:
  - target: llm-service
    type: runs_on
---

## Capability

Provides adversarial critique of technical writing — finds weak arguments, missing context, unclear explanations, unsupported claims, and structural problems. Designed to run on a different model from the drafting step, bringing a genuinely different perspective rather than self-review.

The review is constructive but rigorous. It identifies specific problems and explains why they matter, giving the drafting model actionable feedback to address.

## When to Use

- As the review/verification step in a cross-provider review loop
- When a draft needs external critique before publication
- Paired with a different model from the one that generated the draft

## What It Does

1. **Structural review** — is the argument well-organised? Does it flow logically?
2. **Accuracy check** — are technical claims correct? Are there oversimplifications?
3. **Completeness** — what's missing that the reader would need?
4. **Clarity** — where would a reader get confused or lost?
5. **Evidence quality** — are claims supported? Are examples concrete enough?
6. **Verdict** — pass (ready for polish) or continue (needs revision) with specific instructions

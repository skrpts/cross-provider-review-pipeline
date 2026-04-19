---
type: skill
id: technical-drafting
title: Technical Drafting
description: "Drafts or revises technical content — the generation side of the cross-provider review loop"
tags: [Production, Content, Writing, Loop]
connections:
  - target: llm-service
    type: runs_on
context_params:
  voice_profile:
    label: "Voice Profile"
    description: "Creator's writing style for personalised drafting"
    required: false
  audience_profile:
    label: "Audience Profile"
    description: "Target audience for tone and depth calibration"
    required: false
---

## Capability

Generates or revises technical writing — blog posts, documentation, technical proposals, architecture decision records. Designed to be one half of a cross-provider review loop, where this skill drafts and a different model reviews.

On the first iteration, produces a complete draft from the user's topic and requirements. On subsequent iterations, revises the draft based on adversarial review feedback from the reviewing model.

## When to Use

- As the generation step in a cross-provider review loop
- For any technical writing that benefits from iterative refinement
- When paired with a different model for adversarial review

## What It Does

1. **First pass** — produces a complete technical draft from topic, audience, and style inputs
2. **Revision passes** — incorporates specific feedback from the adversarial review, addressing each point while preserving what already works
3. **Voice application** — if a Voice Profile is provided, maintains the creator's writing patterns throughout

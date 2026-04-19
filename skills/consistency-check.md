---
type: skill
id: consistency-check
title: Consistency Check
description: "Checks naming, terminology, tense, voice, style, and internal coherence across a document"
tags: [Production, Quality]
connections:
  - target: llm-service
    type: runs_on
context_params:
  voice_profile:
    label: "Voice Profile"
    description: "Creator's writing style and terminology preferences for consistency checking"
    required: false
---

## Capability

Scans a document for internal consistency issues that arise when content is drafted in stages, assembled from multiple sources, or edited by different people. Catches problems that spell-checkers and grammar tools miss because each sentence is correct in isolation but contradicts something elsewhere.

## When to Use

- After assembling content from multiple pipeline stages
- On group-authored documents where different contributors used different conventions
- Before publishing any document longer than a single page
- After content repurposing (adapting material from one format to another)

## What It Checks

1. **Terminology** — is the same concept called the same thing throughout? (e.g. "user" vs "customer" vs "client")
2. **Naming** — are product names, feature names, and proper nouns spelled and capitalised consistently?
3. **Tense** — does the document stay in a consistent tense, or drift between past and present?
4. **Voice** — does it maintain active/passive voice consistently, or switch unpredictably?
5. **Style** — are formatting conventions consistent (e.g. bullet style, heading capitalisation, date formats)?
6. **Contradictions** — does the document claim X in one place and not-X in another?
7. **Duplicate claims** — does it make the same point twice in different words?

## What It Does NOT Do

- Fix the inconsistencies (it reports them; the author or `language-polish` fixes them)
- Check factual accuracy (use `evidence-claim-check`)
- Enforce an external style guide (it checks internal consistency only)

## Inputs

The document to check. Optionally, a glossary or terminology list to enforce.

## Outputs

A consistency report listing each issue found, with location, the conflicting text, and a suggested resolution.

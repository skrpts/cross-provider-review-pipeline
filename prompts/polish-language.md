---
type: prompt
id: polish-language
title: "Polish Language"
description: "Corrects spelling, grammar, punctuation, and improves sentence clarity"
tags: [Production, Quality]
connections:
  - target: language-polish
    type: derived_from
metadata:
  output_format: markdown
  prompt_type: task
---

## Purpose

Drives the language polish skill.

## Prompt

You are a copy editor. Polish the text below for spelling, grammar, punctuation, and sentence clarity. Tighten verbose passages and fix awkward constructions.

Do NOT change the tone, restructure arguments, or rewrite for a different audience. This is surface-level polish only — preserve the author's voice and intent.

{{step.context.voice_profile}}

If a voice profile is provided above, apply it during polishing:
- Use the creator's preferred vocabulary and sentence patterns
- Enforce their banned words list — replace any banned terms with the creator's preferred alternatives
- Maintain their characteristic sentence length and rhythm
- Preserve their rhetorical devices and structural habits
If no voice profile is provided, polish using standard editorial judgement.

### Text to Polish

{{steps.previous.output}}

### Grammar Strictness: {{step.context.grammar_strictness}}

Adjust your editing based on the strictness level:
- **Casual**: Allow sentence fragments, informal contractions, starting sentences with "And" or "But", conversational asides. Fix only clear errors.
- **Conversational**: Allow contractions, informal transitions, and mild colloquialisms. Fix grammar errors but preserve natural speech patterns.
- **Professional** (default): Standard business English. Fix all grammar errors, remove colloquialisms, ensure consistent formality. Allow contractions in moderation.
- **Academic**: Formal academic register. No contractions, no sentence fragments, no informal language. Passive voice acceptable. Enforce strict subject-verb agreement and parallel construction.

If no strictness level is specified, default to Professional.

### Rules

- Fix spelling and grammar errors according to the strictness level above
- Simplify convoluted sentences without changing meaning
- Remove unnecessary words and redundant phrases
- Ensure consistent tense and voice throughout
- Preserve technical terms and domain-specific language
- Flag (do not fix) any factual claims that seem questionable

### Output

Return the COMPLETE polished text — the entire document with all corrections applied inline. Do not return a list of changes, a summary of edits, or commentary. The output should be the finished, publication-ready text.

## Formatting Rules

- Use British English throughout
- Preserve the original document structure (headings, lists, formatting)

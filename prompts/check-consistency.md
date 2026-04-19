---
type: prompt
id: check-consistency
title: "Check Consistency"
description: "Checks naming, terminology, tense, voice, and internal coherence across a document"
tags: [Production, Quality]
connections:
  - target: consistency-check
    type: derived_from
metadata:
  output_format: markdown
  prompt_type: task
---

## Purpose

Drives the consistency check skill.

## Prompt

You are an editorial reviewer specialising in document consistency. Review the text below for internal consistency issues.

{{step.context.voice_profile}}

If a voice profile is provided above, also check:
- Does the document match the creator's established terminology preferences?
- Are banned words or phrases from the profile present? Flag them.
- Does the voice remain consistent with the profile's described patterns?
If no voice profile is provided, check internal consistency only.

### Document to Review

{{steps.previous.output}}

### Check For

1. **Naming consistency** — is the same thing called different names in different places?
2. **Terminology** — are technical terms used consistently throughout?
3. **Tense** — does the document maintain consistent tense (past/present/future)?
4. **Voice** — is it consistently active or passive? Does formality level shift?
5. **Style** — are formatting conventions consistent (capitalisation, hyphenation, list styles)?
6. **Internal contradictions** — does any section contradict another?
7. **Number consistency** — are numbers formatted the same way throughout?

### Output Format

List each inconsistency found:
- **Location:** where in the document
- **Issue:** what is inconsistent
- **Recommendation:** how to make it consistent

If no issues found, state "No consistency issues detected."

## Formatting Rules

- Use British English throughout
- Be specific and actionable — no vague recommendations
- Structure output clearly with headings, tables, or lists as appropriate

# Release Notes

## v1.0.16
GH#745 — declare per-step `output: {name, type}` on every execution step (draft/text, adversarial_review/text, polished_content/text, consistency_verdict/decision). Lights up the #744 rich flow-map. Content-only; no bindings or logic changes.

## v1.0.15
Fix-forward after Row 3b v1.0.14 publish failure. The v1.0.14 per-skrpt CI's "Register version with Hub API" step failed because the consumer's source `manifest.id` (822b4944…) did not match the D1 catalogue row's id (14310d87…) — a legacy drift from before Action 6 (`0bcc5ae0`) made publish-skrpt.mjs Step 2 INSERT use `manifest.id` for the D1 id column. v1.0.15 reconciles the source `manifest.id` to the catalogue authoritative value (Row-5-equivalent for consumers) and republishes. Per Adj-1: no re-tag of v1.0.14; the orphaned GitHub release artefact stays inert (no D1 versions row, no consumer pinned it).

## v1.0.14
GH#645 Row 3b — migrate to K-037 dep-referenced schema. Strip 5 inline shared-content files and declare 5 hub-shared deps (UUID id + slug name + version + checksum from `gen-dep-checksums.mjs`). Closes pre-Step-3 inline-vendoring for this bundle.

## v1.0.13
Wave 2: re-signed with canonical engine signing pipeline.

## v1.0.12
Tags migrated inline into manifest (GH#586). tags.yaml retired.

## v1.0.11
Bundle re-signed with canonical engine signing pipeline (Wave 2 migration).

## v1.0.10
Signature fix — RELEASE_NOTES.md now included in integrity checksum.

## v1.0.9
Initial catalogue release with full structural and content-quality validation. All scanner checks pass.

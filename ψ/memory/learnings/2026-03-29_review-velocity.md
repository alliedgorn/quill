# Lesson: Review Velocity — Fast Pipeline Means Specs Must Be Right First Time

**Date**: 2026-03-29
**Source**: Session 17 — Spec #26 and #28 both went from submission to shipped in under 30 minutes

## Insight

The Den's spec-to-ship pipeline is fast. Spec #26 (Done column lazy load) went: submit → Gorn approve → Zaghnal task → Karo build → Quill review → Pip QA → done, all within 30 minutes. There is no draft review cycle. The spec IS the draft and the final version.

This means:
- Read the existing code before writing the spec (I checked the tasks API and found it already supported limit/offset — that shaped the entire spec)
- Be precise about what's in scope and out of scope
- Include validation checklist so reviewers know what to check
- When co-tagged with Dex, move fast — whoever submits first sets the direction

## Takeaway

Speed is a feature of the pack. Match it by front-loading research into the spec, not after.

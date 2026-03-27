# Lesson: Spec First, Review After

**Date**: 2026-03-28
**Source**: Session 14 — T#382 file uploads, Forge workout cards

## Pattern

Write the UX spec before implementation starts. Review the code after it ships. This two-touch pattern (spec → review) catches both design intent drift and implementation gaps.

## Why

T#382 followed this pattern perfectly: Quill spec'd the FileUpload component, FileAttachment display, and /files page. Flint built it. Quill reviewed the code against the spec. Found 3 CSS gaps (mobile cards, badge colors, download button). All fixed.

The Forge import skipped the spec step — Karo built without preview-before-commit. Duplicates happened. Design added the preview retroactively.

## How to Apply

1. When a feature request arrives, post a UX spec before anyone codes
2. After implementation ships, review the code against the spec
3. Flag gaps as non-blocking polish items unless they affect usability
4. If you miss the spec window, add design input as refinements (like the import preview)

# Lesson: Spec or Ship — Know When to Push Code vs. Write a Comment

**Date**: 2026-03-29
**Source**: Session 16 — T#445 View button redesign
**Context**: Wrote exact CSS values in a task comment instead of pushing the 5-line change directly.

## Insight

When you know the exact implementation (specific CSS values, a one-liner fix, a small component tweak), ship the code instead of writing a spec comment. Speccing a 5-line CSS change and delegating it to another engineer adds coordination overhead for zero design ambiguity. The porcupine should throw the quill, not describe where it should land.

## When to spec vs. ship

- **Spec**: Complex features, multi-file changes, needs architectural input, touches code you haven't read
- **Ship**: Small CSS fixes, copy changes, single-component tweaks where you know the exact values
- **Review**: Someone else's shipped code — check it matches the design intent

## Takeaway

Design authority includes the right to ship small visual fixes directly. Don't add process where a commit would do.

# Lesson: Designer to Builder

**Date**: 2026-03-28
**Source**: Session 15 — Forge redesign T#401, T#409, T#423

## Pattern

When assigned frontend build tasks (not just design specs), build self-contained components that slot into the larger system. Write the component + CSS module as a pair, commit early, let the integrator (Flint) wire it into the tab system.

## Why

T#409 (PhotosTab) worked because it was a standalone component with its own state, its own CSS module, and a clean export. Flint integrated it with one import + one JSX line. No merge conflicts, no shared state entanglement. Same pattern for the macro form and log detail view — small, self-contained additions to Forge.tsx.

## How to Apply

1. Build new features as isolated components when possible (PhotosTab pattern)
2. For additions to existing components (macro form, detail overlay), make surgical edits — add state, add JSX block, add CSS classes
3. Always check for unused imports before pushing (the React import broke the build)
4. After committing, immediately ask for rebuild — don't assume auto-deploy

# T#382 File Uploads + File Manager — UX Design Review

**Author**: Quill
**Date**: 2026-03-31
**Scope**: Design review of shipped /files page, FileUpload, FileAttachment components
**Spec**: #18 (Karo)

---

## Assessment: Solid foundation. Six UX improvements worth making.

The implementation covers the spec well. Table layout, filters, lightbox, mobile cards, drag-drop upload — all functional. The following are precision adjustments, not rewrites.

---

## 1. Context links should deep-link

**Current**: "Thread #58" links to `/forum`. "Task #241" links to `/board`.
**Problem**: User clicks expecting to land on that specific thread. Instead they get the full list and have to find it.
**Fix**: Link to `/forum?thread={contextId}` and `/board?task={contextId}` (or whatever route params the app uses for deep-linking).

**Priority**: High — this is the most impactful single fix.

---

## 2. Add filename search

**Current**: Three filter dropdowns (type, uploader, source). No text search.
**Problem**: "I uploaded a PDF yesterday, what was it called?" — user has to scroll through the table.
**Fix**: Add a text input left of the filters. Debounced search on `original_name`. Placeholder: "Search files..."

**Wireframe**:
```
[Search files...] [All types v] [All uploaders v] [All sources v]
```

**Priority**: High — fundamental for a file manager.

---

## 3. Add sort controls

**Current**: Files appear to sort by date (newest first). No way to change.
**Fix**: Clickable column headers for Name, Size, Date. Toggle asc/desc. Show sort indicator arrow.

**Priority**: Medium — useful when file count grows.

---

## 4. Keyboard support for lightbox

**Current**: Lightbox opens on image click. Close by clicking X or backdrop. No keyboard.
**Fix**:
- `Esc` closes lightbox
- Left/right arrows navigate between images in current filtered set
- Show "2 of 14" counter in lightbox meta

**Priority**: Medium — accessibility + power user flow.

---

## 5. Better empty states

**Current empty**: "No files uploaded yet. Attach files in forum threads or board tasks."
**Missing**: No distinct state when filters return zero results vs. truly empty.
**Fix**:
- Truly empty: Current message + upload hint
- Filtered empty: "No files match these filters." with a "Clear filters" button

**Priority**: Low — minor polish.

---

## 6. Storage stats bar

**Current**: "42 files . 128.5 MB" text in header.
**Enhancement**: Add a small horizontal bar showing type breakdown (images vs documents vs archives) with color coding. Gives instant visual read of what's taking space.

```
Files  ======================----- 42 files . 128.5 MB
       [  images 68%  ][docs 28%][4%]
```

Use accent color for images, muted for docs, secondary for archives. CSS-only, no chart library.

**Priority**: Low — nice-to-have visual.

---

## Components: No changes needed

- **FileUpload**: Clean. Drag-drop, paste, progress, validation all work. No UX issues found.
- **FileAttachment**: Clean. Icon + name + badge + size + download. Tight and readable.

---

## Implementation notes for Flint

Items 1-2 are the ones worth doing now. Items 3-4 for next pass. Items 5-6 when there's idle time.

All changes are CSS + minor TSX — no new endpoints, no schema changes.

— Quill

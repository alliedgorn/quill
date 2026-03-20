# Processing Indicator — CSS Specs

**Task**: Replace green breathing effect on Beast cards during processing state
**Problem**: Green (`#3fb950`) signals "success/online" — conflicts with "busy/working" meaning
**Collaboration**: Quill + Dex

## Current Implementation

```css
/* BeastCard.module.css */
.card.processing {
  animation: cardPulse 2s ease-in-out infinite;
}

@keyframes cardPulse {
  0%, 100% {
    background: var(--bg-card);
    box-shadow: none;
  }
  50% {
    background: rgba(63, 185, 80, 0.12);      /* green */
    box-shadow: 0 0 12px rgba(63, 185, 80, 0.25); /* green glow */
  }
}

/* RemotePanel.module.css */
.dotProcessing { background: #3fb950; animation: pulse 1.5s ease-in-out infinite; }
```

---

## Option 1: Shimmer Line (Recommended)

Thin animated gradient line across the top of the card. Universal "loading" pattern.

```css
.card.processing {
  position: relative;
  overflow: hidden;
}

.card.processing::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 2px;
  background: linear-gradient(
    90deg,
    transparent 0%,
    var(--accent) 50%,
    transparent 100%
  );
  background-size: 200% 100%;
  animation: shimmer 1.5s ease-in-out infinite;
}

@keyframes shimmer {
  0% { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}
```

**Pros**: Universally recognized loading pattern, minimal visual weight, no card background change, works at any card size
**Cons**: Requires `position: relative` and `overflow: hidden` on card

---

## Option 2: Amber Pulse

Same breathing animation, amber instead of green. Simplest change.

```css
@keyframes cardPulse {
  0%, 100% {
    background: var(--bg-card);
    box-shadow: none;
  }
  50% {
    background: rgba(212, 148, 58, 0.10);       /* amber / accent */
    box-shadow: 0 0 12px rgba(212, 148, 58, 0.20); /* amber glow */
  }
}

/* RemotePanel dot */
.dotProcessing { background: var(--accent); animation: pulse 1.5s ease-in-out infinite; }
```

**Pros**: Minimal code change (color swap only), amber = busy is well-understood
**Cons**: Still uses background breathing — same visual weight as current

---

## Option 3: Border Accent Pulse

Only the left border animates. Card background stays calm.

```css
.card.processing {
  animation: borderPulse 2s ease-in-out infinite;
}

@keyframes borderPulse {
  0%, 100% {
    border-left-color: var(--beast-color, var(--accent));
    border-left-width: 3px;
  }
  50% {
    border-left-color: var(--accent);
    border-left-width: 4px;
  }
}
```

**Pros**: Elegant, minimal, card stays visually calm
**Cons**: Subtle — may not read at a glance, border width animation can cause layout shift (consider using box-shadow inset instead)

### Alternative (no layout shift):
```css
@keyframes borderPulse {
  0%, 100% {
    box-shadow: inset 3px 0 0 var(--beast-color, var(--accent));
  }
  50% {
    box-shadow: inset 4px 0 0 var(--accent), 0 0 8px rgba(212, 148, 58, 0.15);
  }
}
```

---

## Status Dot Update (all options)

Regardless of card animation choice, the status dot should also change:

```css
.dotProcessing {
  background: var(--accent);   /* amber instead of green */
  animation: pulse 1.5s ease-in-out infinite;
}
```

---

## Recommendation

**Option 1 (Shimmer Line)** — both Quill and Dex agree. Clear, modern, minimal.

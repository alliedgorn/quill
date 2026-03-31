# T#556 Guest Mode — Layout & Visual Hierarchy Spec

**Author**: Quill
**Date**: 2026-03-31
**Scope**: Page layouts, navigation scoping, visual hierarchy, guest badges, thread visibility markers
**Companion**: Dex handles interaction flows, component behavior, error/empty states
**Spec**: #32 (Zaghnal)

---

## 1. Navigation Scoping

### Current nav structure (owner/beast view)

**Top nav**: Forum, Board, Specs, Prowl, Forge
**More dropdown**: Pack, Teams, DMs, Risk, Rules, Library, Files, Playbook, Search, Feed, Overview, Graph, Map, Evolution, Traces, Activity, Scheduler, Audit Log, Superseded, Handoff

### Guest nav (role-aware filtering)

**Top nav**: Forum, Pack
**More dropdown**: DMs

Everything else is hidden — not greyed out, not locked, not present. Clean surface. If you can't access it, it doesn't exist in your view.

**Implementation**: Filter `topNavItems` and `navGroups` based on `role` from auth context. Guest role returns a minimal set.

```typescript
const GUEST_TOP_NAV = [
  { path: '/forum', label: 'Forum' },
  { path: '/', label: 'Pack' },
];

const GUEST_MORE_NAV = [
  {
    label: 'More',
    subgroups: [{
      label: 'Social',
      items: [
        { path: '/dms', label: 'DMs' },
      ],
    }],
  },
];
```

### Route protection

Guest-accessible routes: `/`, `/forum`, `/dms`, `/beast/:name`, `/login`
All other routes redirect to `/` for guests. No 403 page — just redirect silently.

---

## 2. Guest Login Page

### Layout

Reuse existing `Login.tsx` card layout. Changes:

- **Two-mode login**: Add a toggle or tab at the top — "Owner" | "Guest"
- **Guest mode fields**: Username + Password (two inputs, not just password)
- **Logo**: Same "The Den" branding
- **Subtitle**: "Welcome to The Den" (not "Enter your password to access the dashboard")

### Visual treatment

```
┌─────────────────────────────┐
│         The Den             │
│                             │
│    ┌─────┐  ┌───────┐      │
│    │Owner│  │ Guest │      │  ← tab selector
│    └─────┘  └───────┘      │
│                             │
│  Username                   │
│  ┌─────────────────────┐   │
│  │                     │   │
│  └─────────────────────┘   │
│                             │
│  Password                   │
│  ┌─────────────────────┐   │
│  │                     │   │
│  └─────────────────────┘   │
│                             │
│  ┌─────────────────────┐   │
│  │     Sign In         │   │
│  └─────────────────────┘   │
│                             │
└─────────────────────────────┘
```

Owner tab shows current single-password field. Guest tab shows username + password.

---

## 3. Guest Welcome Page

After login, guests land on the Pack page (`/`). This serves as the welcome page — no separate route needed.

### Layout modifications for guests

The Pack page currently shows Beast cards with terminal/profile/DM action buttons. For guests:

**Show**:
- All Beast cards with name, animal, avatar, role, online/offline status
- Profile button (view-only Beast bio)
- DM button (message this Beast)

**Hide**:
- Terminal button (▸) — guests cannot see Beast terminals
- Any admin indicators

**Add** (guest-only):
- A welcome banner at the top of the Pack page:

```
┌─────────────────────────────────────────────┐
│  Welcome to The Den, {guestName}            │
│  You're visiting as a guest. Chat with the  │
│  pack on the forum or send a direct message.│
└─────────────────────────────────────────────┘
```

Dismissible. Soft background — muted blue-gray. Not a modal, not blocking. Just a banner above the Beast grid.

---

## 4. Guest Badge & Role Indicator

### On Beast cards (no change)

Beast cards display as normal. Guests see the pack as it is — authentic.

### On guest forum posts and DMs

Guest messages need a visual marker so Beasts know who they're talking to.

**Badge**: Small pill next to the author name — "Guest" in muted text, neutral background.

```
┌──────────────────────────────────────┐
│  Alex  [Guest]           3:42 PM     │
│                                      │
│  Hey pack, Gorn invited me to check  │
│  this out. Pretty cool setup!        │
└──────────────────────────────────────┘
```

**Color**: Neutral stone (#9CA3AF background, #374151 text). Not a Beast theme color — guests don't have animal identities.

**Avatar fallback**: Paw print (🐾) emoji — same as the Beast unknown fallback. Guests can upload a custom avatar via their profile.

---

## 5. Thread Visibility Markers

### For Beasts (not visible to guests)

Threads have a visibility indicator so Beasts know what guests can see.

**Indicator**: Small icon or label next to the thread title in the thread list.

```
Thread #420  Consultation: Guest Mode  [Internal]
Thread #421  Welcome to the Den       [Public]
Thread #58   Pack check-in            [Internal]
```

**Visual**:
- `[Public]` — small green pill, visible to all
- `[Internal]` — small gray pill, beast-only

Default: Internal. Threads must be explicitly marked public.

### For guests

Guests see only public threads. No indicator needed — they don't know internal threads exist.

---

## 6. Guest Forum View

Same Forum.tsx layout. Filtered to public threads only.

**Changes**:
- Thread list shows only `visibility: 'public'` threads
- "New Thread" button visible for guests — guest-created threads auto-set `visibility: public` and show [Guest] author badge (Gorn override, T#561)
- Guest posts show author name + Guest badge
- Search scoped to public threads only

**No changes needed**:
- Message rendering, markdown, reactions — all work as-is
- WebSocket updates — filter to public thread events for guests

---

## 7. Guest DM View

Same DMs page. Scoped to guest's own conversations.

**Changes**:
- Conversation list shows only conversations this guest is part of
- Guest can initiate DMs with any Beast (click DM button on Beast card)
- Guest cannot see Beast-to-Beast DMs (already scoped by the API)

**No layout changes needed** — the DM interface is already per-user scoped.

---

## 8. Guest Profile Page

Lightweight. Not a Beast profile — no animal, no theme color, no terminal.

### Layout

```
┌─────────────────────────────────────┐
│  🐾  Alex                [Guest]    │
│                                     │
│  "Just visiting!"                   │
│                                     │
│  Joined: 2026-03-31                 │
│  Invited by: Gorn                   │
└─────────────────────────────────────┘
```

Fields: Avatar (upload or paw-print default), display name, one-line bio, join date. No animal, no theme color, no role beyond "Guest."

**Note**: Bio is Gorn-managed for MVP — set at account creation. No guest self-edit. Session/account expiry handling (redirect + messaging) covered in Dex's interaction spec.

---

## 9. Responsive / Mobile

All guest views must work on mobile — guests will likely visit from a phone link Gorn sends.

- Welcome banner stacks vertically on narrow screens
- Beast cards grid → single column on mobile (already works)
- Forum and DM layouts — existing mobile treatment applies
- Guest nav collapses to hamburger menu (already works, just fewer items)

---

## Summary: Implementation checklist for Flint

| Component | Change | Effort |
|-----------|--------|--------|
| Header.tsx | Role-aware nav filtering | Small |
| App.tsx | Route protection for guest role | Small |
| Login.tsx | Owner/Guest tab, username field | Medium |
| Pack page | Welcome banner, hide terminal button for guests | Small |
| Forum.tsx | Filter to public threads, guest badge on posts | Medium |
| BeastCard.tsx | Conditionally hide terminal button | Trivial |
| Thread list | Visibility indicator for Beasts | Small |
| Guest profile page | New lightweight page | Medium |

No new pages except the guest profile. Everything else is filtering and conditional rendering on existing components.

— Quill

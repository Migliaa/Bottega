# Acme Notes — Sprints

> Planned work only. `[O]`/`[S]` = model hint · `[ ]`/`[x]` = open/done.

## S1 — Search & filtering

### S1.A — Full-text search — ✅ ready-to-test (validating)
- [x] S1.A.1 `[O]` SQLite FTS5 virtual table + sync triggers on `notes` (2026-03-10)
- [x] S1.A.2 `[S]` `/api/search?q=` route returning ranked matches (2026-03-10)
- [x] S1.A.3 `[S]` search box + results list in the UI (2026-03-10)
- **Definition of done:** `npx tsc --noEmit; npm test` green; search returns correct matches for the seed set. ✅

### S1.B — Tag filtering (micros: defined just-in-time, not yet)

---

## ▶️ WORK ORDER — S1.B Tag filtering (for an execution session)
- **Files in scope:** `app/api/notes/`, `app/components/tag-filter.tsx`, `lib/db.ts`. **Out of scope:** search (S1.A, done).
- **Micros (in order):**
  1. `[O]` `note_tags` join table + migration; backfill from existing inline `#tags`.
  2. `[S]` `?tags=` filter on the notes list API (AND semantics).
  3. `[S]` tag-chips filter UI.
- **Invariants:** filtering never mutates notes; empty filter = all notes.
- **Definition of done:** `npx tsc --noEmit; npm test` green; filtering by 2 tags returns the intersection.
- **Rules:** work on `main`; **don't commit** (manager does); never touch `**/.env.local`.

# 04 · The INTERFACE protocol — async mailbox

The `INTERFACE.md` files are how the manager and a **persistent** department communicate **without** anyone having to *remember* to update anyone. It's a deterministic, file-based mailbox read and written at fixed session boundaries.

## Why it exists

The committente is the message bus, but a bus you have to *remember to ride* is fragile. The interface protocol removes the human reminder: each side reads its mailbox at a fixed moment (session start, and — for the manager — before every gate) and writes to it at a fixed moment (session close). Nobody says "remember to sync"; the protocol *is* the syncing.

## One file per department, not a hub

Each persistent department has its **own** `INTERFACE.md`:

```
validation/INTERFACE.md
content/publishing/INTERFACE.md
compliance/INTERFACE.md
```

Why per-department and not one shared hub: departments run and **push independently**, so a single hub would cause merge conflicts. One small file each means the manager reads N small files at start (its only "inbox"), and pushes never collide. Disposable execution sessions have **no** interface — they use work-orders in `Sprint.md`.

## The three sections

Every `INTERFACE.md` has exactly three sections:

```markdown
## 📍 Digest — <DEPT>        ← 1–3 lines of current state, kept by the department
## 📥 From Manager           ← requests / decisions / answers from the manager
## 📤 From <DEPT>            ← updates / requests / questions from the department
```

- Entries are checkboxes: `[ ]` open, `[x]` closed.
- IDs make threads referenceable: `#M<n>` for manager items, `#<letter><n>` for department items (e.g. `#V3` from validation).
- The **digest** is a 1–3 line pointer; the **detail** lives in `<dept>/STATUS.md`. Don't duplicate state across the two.

## Channel vs register

> The `INTERFACE.md` carries **messages** (what we need from each other). The `STATUS.md` carries **facts** (what is validated / published / covered).

Keep them distinct. The interface is the conversation; the status is the ledger. The digest links to the ledger for detail.

## The discipline at session boundaries

**Department at START** (after reading its charter + `STATUS.md`): read `## From Manager`, do the open `[ ]` requests as part of the work.

**Department at CLOSE** (before stopping): update `## 📍 Digest`, write what it did + what it needs in `## From <Dept>`, check off `[x]` what it closed, then **commit + push** its folder (including the interface).

**Manager at START of every session and BEFORE every gate:** pull and read the digests + open `## From <Dept>` of all persistent departments, triage, answer/order in `## From Manager`, check off `[x]`. *This fixed step replaces "remind me to check."*

**When the committente says a department just worked:** the manager pulls and reads that `INTERFACE.md` immediately, before deciding.

## A worked example

```markdown
## 📍 Digest — VALIDATION
- 2026-06-29 — 8 dossiers done. signal-X: V1 ✓ / V3 ✓, only V2 (committente) left.
  Detail: validation/STATUS.md

## 📥 From Manager
- [x] #M3 — Close the cross-month accumulation gap in signal-X; propose the fix as a diff.
  - ✅ DONE (validation): root cause is anchoring, not look-ahead; recommended a daily reset; 2 tests proposed.

## 📤 From VALIDATION
- [ ] #V4 — Which is the next priority dossier, A or B? (A is heavier: many new citations.)
```

The manager, at its next start, reads `#V4`, answers it under `## From Manager`, checks the box, and the loop continues — no human had to chase either side.

Next: **[05 · Constraints](05-constraints.md)**.

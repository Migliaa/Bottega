# 04 · The INTERFACE protocol — async mailbox

The `INTERFACE.md` files are how the manager and a **persistent** department communicate **without** anyone having to *remember* to update anyone. It's a deterministic, file-based mailbox read and written at fixed session boundaries.

## Why it exists

The committente only **triggers** the sessions; the files are the bus. But a channel someone has to *remember to check* is fragile. The interface protocol removes the human reminder: each side reads its mailbox at a fixed moment (session start, and — for the manager — before every gate) and writes to it at a fixed moment (session close). Nobody says "remember to sync"; the protocol *is* the syncing.

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

Because the facts live in `STATUS.md` and the artifacts, a forgotten `INTERFACE.md` update is **recoverable**: the manager reconciles the digest from disk at its next start. The channel is a convenience over the disk truth, not a single point of failure — so a department slipping on its update never blocks the manager. (The one thing only the department can supply is its *questions*; those are worth the discipline of writing them down.)

Keep them distinct. The interface is the conversation; the status is the ledger. The digest links to the ledger for detail.

## Two rules that keep the channel lean

`INTERFACE.md` is the **hottest read in the system** — the manager reads it at every session start *and* before every gate. Two rules keep that hot path cheap and conflict-free:

1. **Single-writer sections + originator-closes.** Write **only in your own outbound section** (`📤 From <Dept>` for the department, `📥 From Manager` for the manager). To answer a thread, don't edit the other side's section — **reply by referencing its id in your own** (`↳ re #V4 — …`). Only the thread's **originator** flips it to `[x]`, at its next start, after reading the reply. This gives `[x]` one exact meaning — *the originator acknowledges closure* (an ACK) — and makes each section single-writer, so two independent pushes to the same file merge cleanly instead of colliding on the bus itself.

2. **Prune-on-close.** At CLOSE, each actor **deletes from its own sections the threads that were already `[x]` at the previous close.** The hot file stays O(open items) instead of growing forever. Nothing is lost: **git history is the archive** — `git log -p <dept>/INTERFACE.md` replays every thread ever closed, and the facts already live in `STATUS.md`. (Deleting on the *next* close, not the same one, leaves a one-session trail that a thread was closed before it disappears.)

## The discipline at session boundaries

**Department at START** (after reading its charter + `STATUS.md`): read `## 📥 From Manager`, do the open `[ ]` requests as part of the work; flip to `[x]` any of its **own** threads (in `## 📤 From <Dept>`) that the manager has now answered.

**Department at CLOSE** (before stopping): update `## 📍 Digest`; in `## 📤 From <Dept>` **only** — write what it did + what it needs, and reply to any manager thread by id (`↳ re #M3 — …`); prune its own threads that were already `[x]` at the previous close; then **commit + push** its folder (including the interface).

**Manager at START of every session and BEFORE every gate:** pull and read the digests + open `## 📤 From <Dept>` of all persistent departments; triage; in `## 📥 From Manager` **only** — answer/order and reply by id; flip to `[x]` any of its **own** threads a department has now answered; prune its own already-closed threads. *This fixed step replaces "remind me to check."*

**When the committente says a department just worked:** the manager pulls and reads that `INTERFACE.md` immediately, before deciding.

## A worked example

```markdown
## 📍 Digest — VALIDATION
- 2026-06-29 — 8 dossiers done. signal-X: V1 ✓ / V3 ✓, only V2 (committente) left.
  Detail: validation/STATUS.md

## 📥 From Manager
- [ ] #M3 — Close the cross-month accumulation gap in signal-X; propose the fix as a diff.

## 📤 From VALIDATION
- [ ] #V4 — Which is the next priority dossier, A or B? (A is heavier: many new citations.)
- ↳ re #M3 — root cause is anchoring, not look-ahead; recommend a daily reset; 2 tests proposed. Diff in validation/STATUS.md.
```

Each side wrote **only in its own section**. Next round: the **manager** reads `↳ re #M3`, flips **its own** `#M3` to `[x]` (ACK), and replies to `#V4` with `↳ re #V4 — start with A` under `## 📥 From Manager`; the **department**, at its next start, reads that and flips **its own** `#V4` to `[x]`. No human chased either side, and no one edited the other's section — so concurrent pushes never collide.

Next: **[05 · Constraints](05-constraints.md)**.

# 02 · The method — cycle, sprints, gates

This is how work actually moves through a Bottega.

## The work cycle

```
   ┌──────────────────────────────────────────────────────────────┐
   │ 1. You bring the manager a goal / a decision / a dept output  │
   │ 2. Manager plans → writes a work-order in Sprint.md           │
   │ 3. You launch the right session (slash command)               │
   │ 4. That session executes in its lane → hands output to you    │
   │ 5. You relay it back → manager triages, updates files, commits│
   └──────────────────────────────────────────────────────────────┘
                              ↺ repeat
```

The loop is intentionally boring. The intelligence is in *who does what* and *where state lives*, not in clever orchestration.

## Three levels of planning: Macro → Sub → Micro

Bottega plans at three granularities so you commit detail **just in time**, not up front:

- **Macro-sprint** (`S1`, `S2`, …): a large, coherent objective.
- **Sub-sprint** (`S1.A`): a substantial, independently **testable/validatable** block inside a macro. This is the unit **you** validate.
- **Micro** (`S1.A.1`): the largest operation a single model pass can do safely with margin for error. A clear, self-contained, verifiable deliverable — the unit work is actually done in.

**When each is defined:**
- Macro → Sub: sketched **early**, knowing it will shift with reality.
- Sub → Micro: decomposed **only for the imminent sub-sprint** (just-in-time). Don't pre-write micros for future subs — they'll be wrong by the time you reach them.

**Model tag per micro** (written in `Sprint.md`): mark architectural / error-prone / algorithmic micros for the stronger model; mark mechanical, scoped micros (scaffolding, simple UI, config, linear CRUD) for the cheaper/faster one. The committente sets the actual model at launch — the tag is a recommendation.

## Work-orders: how the manager hands off

A **work-order** is a self-contained block in `Sprint.md` that an execution session can pick up cold. A good work-order states:

- the **files in scope** (and what's out of scope);
- the **ordered micros**, with any that are deferred;
- the **invariants/constraints** that must hold;
- the **definition of done** (which tests must be green);
- the recommended **model** per micro.

The launch message to an execution session is then just a **pointer** (`/lancia-operativa <work-order name>`) — the session reads the rest from disk. Rules are *not* re-stated at every launch; they live in `CLAUDE.md` and the auto-loaded files.

### Before you launch a session (manager pre-flight)

Before telling the committente to launch a session, the manager confirms all four — any "no" means fix it first, don't launch:

1. **Work-order in the read-path?** A self-contained `[ ]` work-order sits where that session reads it (`Sprint.md` for execution; `INTERFACE.md → From Manager` for a department).
2. **Blocking decisions resolved?** Every choice the session would otherwise stop to ask is decided and written down — including an explicit *what is NOT a blocker*.
3. **Session type matches task type?** Build/code → an execution session scoped to the owner's folder; recurring domain work → that department's session.
4. **Committente prerequisites listed?** Any secret/account/setup the committente must provide, each with *what it blocks* (and what it doesn't).

The incident this prevents: launching a *department* session to *build code*, with the work-order sitting in a file that session never reads — it dutifully stops and asks, producing nothing.

## Gates: nothing skips a link

A **gate** is a checkpoint that must be green before the project advances past it. The manager **enforces** the chain; no one jumps a link. Typical gates:

- **Static checks green** before "ready to test" (type-check, lint, the relevant tests).
- **Committente validation** at the end of each sub-sprint.
- **Department sign-off** before a dependent step (e.g. *validation* must mark a result correct before *publishing* announces it).
- **Release/charge gate**: a higher bar (e.g. compliance green + infra ready) before anything money- or public-facing ships.

Before a gate, the manager **re-reads the relevant `STATUS.md`/`INTERFACE.md` from disk** — it does not trust its in-context copy, which may be stale.

## Commit & push discipline

- Work happens **directly on `main`** by default. No per-session branches unless the committente explicitly asks to isolate risky work.
- **Only the manager commits and pushes**, and only after the committente's OK on a validated block. Commit messages are **exhaustive** (so you can roll back to a known-good state).
- Execution sessions **do not commit** — they leave changes for the manager, who reviews, tests, and commits them. (If your tool auto-creates a per-session worktree/branch, the manager merges it to `main`.)
- Departments commit **scoped to their own folder** for record-keeping, same discipline (never secrets, never the protected IP into a public surface).

## End-of-sprint discipline (the manager does this)

When the committente validates a block:

1. Update `Sprint.md` (check off micros, timestamp). Move a finished macro to `Sprint_Done.md`.
2. Update `ActualStatus.md` (what's done, what works, what's next).
3. Then `git commit` + `git push` to `main` with an exhaustive message.
4. Notify the committente with a structured summary.

> Updating a file is **not** the same as re-reading it. The current session's context is unchanged; the updated files serve the *next* session, which loads them fresh.

## Switching sprints in one session

It's normal to finish several sprints in one session. When one is validated and the next begins, **don't re-read the `.md` files** — the context is already there. Instead, the manager reminds the committente that `/compact` (or a fresh session) is a good idea before a heavy next sprint, to keep the context efficient. `/compact` is typed by the human; the agent only reminds at the right moment.

## Token economy (a first-class concern)

Files cost tokens — input every time an auto-loaded file is read, output every time one is rewritten. Bottega keeps the recurring cost low on purpose:
- **Auto-load only the essentials** (`ActualStatus.md`, `Sprint.md`). Everything else (`DECISIONS.md`, charters, these docs) is read **on demand**.
- **Prefer conventions over files**, and **append over rewrite** (a decision is one new line, not a rewritten section).
- **Reuse anchors:** `/handover` updates the existing 📌 STATE block instead of spawning a new file.

The test for any new process: does it save more tokens downstream (fewer re-explanations, re-litigated decisions, fix-loops, ambiguous hand-offs) than it costs to maintain? If not, it doesn't earn a file.

Next: **[03 · Roles](03-roles.md)** — who is allowed to do what.

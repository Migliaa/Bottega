# 01 · Overview — the model

Bottega runs a project as a **small company of AI agents**. This page is the mental model; the other docs drill into each part.

## The three kinds of actor

1. **The committente (you).** The human patron. You decide strategy, scope, and money. Crucially, you **don't carry the content** between sessions — the **git files** do. Your job is to **decide** and to **trigger** the sessions (launch each one so it reads and writes the files), and to approve pushes.

2. **The manager (one Opus session).** The *capobottega*. It plans (decomposes work Macro → Sub → Micro), makes architecture/product calls, triages department outputs, writes the sprints, and is the **only actor that commits to `main`**. It owns the planning and decision files.

3. **The departments (separate sessions).** Specialized lanes, each with a narrow mandate and its own folder. You launch them on demand. Two flavors:
   - **Persistent departments** (e.g. validation, publishing, compliance): long-lived concerns that accumulate state and communicate with the manager through an `INTERFACE.md` mailbox.
   - **Disposable execution sessions** (the "operative"): spun up to run a single work-order, then discarded. They have no `INTERFACE.md`; they read their work-order from `Sprint.md` and hand a summary back.

## Why separate sessions at all

Because one context window doing planning **and** implementation **and** QA **and** research is a context that does all of them worse, and loses the thread when it fills up. Separating the lane into a separate session means:

- each session has a **clean, focused context** and a single mandate;
- the **manager stays strategic** and isn't burned down by implementation detail;
- work is **parallelizable** across your day (launch one, come back later);
- a session can **die without losing anything** — its state is on disk.

## Why git is the bus (and you just trigger)

It's a deliberate simplicity. Agents coordinating *directly* needs shared runtime, message queues, and conflict handling. Bottega instead splits the job in two:

- **git files** hold the **content** — `INTERFACE.md` (manager ↔ persistent department), `Sprint.md` (work-orders + the operative's Execution Record), `ActualStatus.md`. These are the **bus**.
- **you** provide the **trigger** — you launch each session so it reads/writes those files, and you decide. You do **not** copy outputs from one session into another; the files already carry them.

So there is **no infrastructure to run**, and you're not a courier — you're the one who gives the go. The "network" is the repo. It scales down to a solo builder and up to a small team, and leaves a complete audit trail in git history.

## The files that hold the company together

| File | Holds | Read by |
|---|---|---|
| `ActualStatus.md` | Where we are, in one page (auto-loaded each session) | Everyone, first |
| `Sprint.md` | Planned work: sprints, sub-sprints, micros, work-orders | Manager + execution |
| `Sprint_Done.md` | Archive of finished sprints | On demand |
| `COMMITTENTE.md` | The human cockpit: how to work, where files are, what to do now | You |
| `<dept>/INTERFACE.md` | Async mailbox between manager and a persistent department | Manager + that dept |
| `<dept>/STATUS.md` | A department's domain register (facts, not messages) | Manager + that dept |
| `CLAUDE.md` | The process constitution (the rules every session obeys) | Every session |

The discipline that makes this work: **state lives in files, not in context.** A session may hold a stale copy from when it started; **the disk wins.** Persistent departments and execution sessions re-read the relevant files at the start of every hand-off, before acting.

## What a project looks like once Bottega is installed

```
your-project/
├── CLAUDE.md                 ← process constitution (from template)
├── COMMITTENTE.md            ← your cockpit
├── ActualStatus.md           ← live state (auto-loaded)
├── Sprint.md                 ← planned work + work-orders
├── Sprint_Done.md            ← archive
├── DECISIONS.md              ← append-only decision log
├── .claude/commands/         ← /lancia-org, /lancia-operativa, /handover, /bootstrap, …
├── research/                 ← research briefs + outputs (README.md = index)
├── reference/                ← reference shelf: distilled method cards, pull-on-trigger (INDEX.md)
├── incidents/                ← post-mortems
├── <your code>/              ← web/, engine/, src/, …
├── validation/               ← a persistent department (STATUS.md + INTERFACE.md)
│   ├── STATUS.md
│   └── INTERFACE.md
└── … other departments you chose
```

Next: **[02 · The method](02-the-method.md)** — the work cycle and how sprints are structured.

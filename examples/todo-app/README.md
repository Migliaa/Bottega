# Example instance — "Acme Notes"

A tiny, fully filled-in Bottega so you can see what the templates look like once a real project has adopted them. **Acme Notes** is a fictional markdown notes app (Next.js + SQLite), private repo, with two departments enabled: **execution** and **validation**.

It's deliberately small — a manager, one work-order, and one persistent department — to show the shape without noise.

## What's here

```
todo-app/
├── CLAUDE.md            ← the process constitution, placeholders filled
├── COMMITTENTE.md       ← the human cockpit, filled
├── ActualStatus.md      ← a realistic live-state page
├── Sprint.md            ← one macro, one sub, and a work-order
└── validation/          ← a persistent department, fully wired
    ├── CHARTER.md
    ├── STATUS.md
    └── INTERFACE.md
```

(A real project would also have `.claude/commands/` with the launchers, a memory dir seeded, and the actual code — omitted here to keep the example focused on the Bottega scaffolding.)

## How to read it
1. Start with `COMMITTENTE.md` — that's where a human starts.
2. Then `ActualStatus.md` → `Sprint.md` to see how work is planned.
3. Then `validation/INTERFACE.md` to see the manager↔department mailbox in action.

Compare each file to its `*.template` in [`../../templates/`](../../templates/) to see exactly which placeholders got filled.

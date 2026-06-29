# CLAUDE.md — Acme Notes · process constitution

> Filled from the Bottega `templates/CLAUDE.md.template`. (Example instance.)

**Project:** Acme Notes — A markdown notes app for small teams.
**Repo:** https://github.com/me/acme-notes.git (private) · **Root:** C:\Users\me\Desktop\Projects\acme-notes · **Stack:** Next.js + SQLite
**Manager speaks:** English

## 1. Files and their purpose
`ActualStatus.md` + `Sprint.md` auto-load each session. `Sprint_Done.md`, `validation/*` on demand. **The disk wins** — execution/department sessions re-read state from disk before acting.

## 2. How we work
- **Manager** (Opus) — plans, decides, triages, writes sprints, **only actor that commits to `main`**.
- **Execution** (operative, disposable) — runs the current sub-sprint's defined micros; may check `[x]` its micros + update `ActualStatus.md`; everything else read-only; **stops** at sub-sprint end / undefined micros / gates / decisions (→ escalate).
- **Validation** (persistent) — owns `validation/`; verifies correctness, keeps `STATUS.md`, talks via `INTERFACE.md`.
- **Macro→Sub→Micro**, just-in-time; work-orders are self-contained blocks in `Sprint.md`.
- **INTERFACE protocol:** manager reads `validation/INTERFACE.md` at start of every session and before every gate; validation does its open `From Manager` items at start and writes back at close.
- **Commit:** on `main`, only the manager, after the committente's OK, exhaustive messages. Execution doesn't commit.

## 3. Interaction style
Concise & structured; recommend don't enumerate; static checks before "ready to test" (`npx tsc --noEmit; npm test`); **UI testing is opt-in** (never auto-launch a dev server). Persistent tests in `tests/CATALOG.md`.

## Guardrails (non-negotiable)
- NEVER commit secrets: `**/.env.local`. Verify before every commit.
- Repo is **private**. (No internal-only department in this example.)
- IP-to-protect: **none**.
- Only the manager commits/pushes. Open every session rooted in `C:\Users\me\Desktop\Projects\acme-notes`.
- Commits end with: `Co-Authored-By: Claude <noreply@anthropic.com>`

## Manager's working contract
Brutal honesty · lateral thinking & ownership · pre-vet before proposing · verify in code before asserting · recommend don't enumerate · simplest structure that works.

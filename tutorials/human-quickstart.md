# Human quickstart

Your manual as the **committente** (the patron). Assumes Bottega is already installed (if not → [bootstrap-new-project.md](bootstrap-new-project.md)). 2-minute read.

## The one idea
You run a **small company of AI agents**, one session at a time. You **decide** and you **carry messages** between sessions — you don't do the work yourself. There's one **manager** (you mostly talk to it) and a few **departments** (you launch for specific jobs).

## The loop
1. **Talk to the manager** — for a decision, a plan, or to hand over a result.
2. It points you to a session to launch.
3. **You launch it** (slash command), let it work.
4. You tell the manager **which** job finished — it triages from disk itself.
5. It updates the files and (with your OK) **commits**.

## Launch a session
Open a **new** Claude Code session **inside your project folder**, type the command as the first message:

| To… | Type |
|---|---|
| start / resume the manager | `/lancia-org` |
| run a defined chunk of work | `/lancia-operativa <work-order>` |
| validate / publish / check risk | `/lancia-<dept>` |
| deep research | `/lancia-ricerca <brief>` |
| save a clean resume point before `/compact` | `/handover` |

> **#1 rule (prevents 90% of problems):** open the session **from your project folder**. "Unknown command" = you're in the wrong folder.

## You only read 3 files
- **`COMMITTENTE.md`** — your cockpit. Start here.
- **`ActualStatus.md`** — where the project is, one page.
- **`Sprint.md`** — planned work + the work-orders you launch.

The rest is for the agents.

## Golden rules
- **Root every session in the project folder.**
- **Only the manager commits.**
- **Secrets never get committed** — stop any agent staging an `.env`.
- **A dead session loses nothing** — state is in the files; tell the manager, it recovers.
- **Decisions go to the manager** — if a department starts deciding strategy or price, route it back.

## What the manager does for you
- **Reminds you** of the decisions only you can make ("ball in your court") — you won't lose track.
- **Pushes back, doesn't flatter.** If it just agrees with everything, tell it to re-read its working contract.

## Keep it fast & cheap
Before `/compact` or closing, run **`/handover`** (it writes a clean resume point). Between heavy sprints, `/compact` or open a fresh session — files reload automatically.

Stuck? → [troubleshooting.md](troubleshooting.md)

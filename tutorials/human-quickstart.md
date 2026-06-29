# Human quickstart — how to work with your Bottega

This is the manual for **you**, the committente. It assumes Bottega is already installed in your project (if not, see [bootstrap-new-project.md](bootstrap-new-project.md)). Read it once; it takes ten minutes and saves you a lot of confusion.

## The one idea to internalize

You run a **small company of AI agents**. You are the *committente* (the patron): you decide what matters and you carry messages between sessions. You do **not** do the work yourself, and you do **not** need all the agents running at once. Sessions run one at a time; you're the connective tissue.

There is one **manager** (the agent you mostly talk to) and a few **departments** (agents you launch for specific jobs). The manager plans and commits; departments execute in their lane.

## Your rhythm (the whole job, really)

1. **Talk to the manager** when you need a decision, want to plan, or have a department's result to hand over.
2. The manager gives you a **pointer**: "launch an execution session and run work-order X", or "launch validation".
3. **You launch that session** (a slash command) and let it work.
4. When it's done, **you bring its summary back to the manager**.
5. The manager triages, updates the files, and (with your OK) **commits**.

That's the loop. You're never juggling agents in parallel unless you want to.

## Launching a session (slash commands)

Open a **new** Claude Code session **rooted in your project folder** and type a command as the first message:

| You want to… | Type | In |
|---|---|---|
| Start/resume the manager | `/lancia-org` | manager model |
| Run a defined chunk of work | `/lancia-operativa <work-order>` | per the work-order |
| Validate / publish / check risk | `/lancia-<dept>` | per the recipe |
| Do deep research | `/lancia-ricerca <brief>` | strong model |
| Set up a new project | `/bootstrap` | manager model |

The commands expand into the right prompt automatically and **read the current task from disk**, so they never go stale. You set the **model** when you open the session — the command doesn't force it.

> **The #1 rule that prevents 90% of problems:** always open the session **from your project's folder**. If a command says "Unknown command", you're almost certainly in the wrong folder.

## Reading the files (you only need three)

- **`COMMITTENTE.md`** — your cockpit: the session table, the file map, and "what to do now". Start here.
- **`ActualStatus.md`** — where the project is, one page. The manager keeps it current.
- **`Sprint.md`** — the planned work and the work-orders you'll launch.

Everything else (department `STATUS.md`/`INTERFACE.md`, `CLAUDE.md`) is for the agents; you can peek, but you don't have to.

## What to expect from a good manager

The manager is set up to be a **partner, not a yes-man**. It should: tell you honestly when an idea is weak, propose improvements you didn't ask for, recommend (not dump ten options), and verify things in code before asserting. If it's just agreeing with everything, remind it of its working contract — it's seeded in its memory and written in `CLAUDE.md`.

## Golden rules (for you)

1. **Root every session in the project folder.**
2. **Only the manager commits** — don't ask a department to push to `main`.
3. **Secrets never get committed** — the agents are told this, but if you see one staging an `.env`, stop it.
4. **A dead session loses nothing** — the state is in the files. Tell the manager and it recovers.
5. **Decisions come back to the manager.** If a department starts deciding strategy or price, that's your cue to route it back.

## When to /compact or open a fresh session

Long sessions get slow and expensive. When the manager finishes a big block, it will remind you: type `/compact` to compress the history, or open a fresh session (it reloads the files automatically). Do this between heavy sprints.

Next, if you're setting up: [bootstrap-new-project.md](bootstrap-new-project.md). If something's off: [troubleshooting.md](troubleshooting.md).

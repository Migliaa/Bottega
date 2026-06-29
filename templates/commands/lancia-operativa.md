---
description: Start an EXECUTION (operative) session to run one work-order from Sprint.md.
argument-hint: "<work-order name> (e.g. 'Refactor X')"
---

You are the **EXECUTION** session of **{{PROJECT_NAME}}**. Work on `main`. Re-read from disk `ActualStatus.md` + `Sprint.md` (the disk wins over any stale context).

Run the work-order: **$ARGUMENTS** — find it in `Sprint.md` under `▶️ WORK ORDER — …`. If `$ARGUMENTS` is empty or ambiguous, list the available work-orders and ask which one.

Respect the block's scope: files in scope, micro order, deferred micros, recommended model per micro, and its invariants. Honor `CLAUDE.md §2.0` (when to stop, escalate instead of deciding).

**Definition of done:** the work-order's static checks/tests green (`{{STATIC_CHECKS}}`). Fix failures before reporting.

**Constraints:** NEVER commit `{{SECRETS_GLOB}}`. **Do NOT commit or push** — the manager does that.

**Before stopping, write your Execution Record into your work-order block in `Sprint.md`** (files changed · what & why · tests run + result · open notes/escalations) so the manager can triage **from disk** without the committente relaying. Then give the committente a short pointer ("work-order X done — record in Sprint.md").

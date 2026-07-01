# Changelog

Bottega is hardened by **dogfooding**: it runs a real project, and most rules below trace to a concrete incident in real use. That's the maintenance philosophy — the framework earns each rule rather than guessing it up front.

## Current focus (maintainer)
Framework **complete and stable**, maintained **incident-driven** (entries below). Nothing in flight on the framework itself. The meta-manager session should open **rooted in this folder** (`Bottega`) so `/handover` and native git work; downstream projects are advised separately, their state in their own repos.

## Unreleased / ideas
- Tighten `tutorials/bootstrap-new-project.md` and `tutorials/troubleshooting.md` to the same scannable bar as `human-quickstart.md`.

## 2026-06-30 — Session isolation
- **Non-org sessions get their own worktree**, never `git checkout -b` / commit in the org's shared main dir. Resolves the rooting-vs-isolation contradiction (launch from the repo, then isolate before writing); the org owns `main`, everyone else works on a worktree+branch the org merges. *(Incident: `/lancia-ricerca` did `checkout -b` in the main dir → collided with the org, detached HEAD, org commits on the research branch.)*
- **Test/build deps stay in the work-order's territory** (declare in the in-scope folder's package; never `add` into a neighbouring project). *(Incident: an operative scoped to publishing ran `uv add` in `engine/`.)*
- `/lancia-ricerca`: briefs read from `research/briefs/` (accepts name or path).

## 2026-06-29 — Launch protocol hardening
- **Deliver the command, not a description:** the green-light hands the committente the *literal, args-filled* launch line, never a prose pointer. *(Incident: a bare `/lancia-operativa` with 3 open work-orders → the session correctly asked which one.)*
- **Session-type routing + launch invariant + pre-flight:** build/code → an execution session scoped to the owner's folder (never a department session); no launch until a self-contained work-order sits in the session's read-path; a 5-point manager pre-flight. *(Incident: a department session launched to build code, work-order in a file it never reads → stopped and asked.)*
- **Territory-scoped work-orders:** one owner per work-order; a department's folder is its own; never span two territories. *(Incident: an operative about to write into another department's folder.)*
- **Every department triages from disk:** `STATUS.md` + artifacts are the truth; the manager reconciles a skipped `INTERFACE.md` from disk, never blocks. *(Incident: a department finished but didn't update its INTERFACE.)*

## 2026-06-29 — Lightweight process upgrades (borrowed from leader frameworks)
- `/handover` (resume point before `/compact`), `DECISIONS.md` (append-only decision log), gate states PASS/CONCERNS/FAIL/WAIVED + remediation cap, work-order acceptance criteria + Execution Record, planning tiers, "ball in the committente's court" reminders, operative-handoff-from-disk. Token-aware: no new auto-loaded files.

## 2026-06-29 — Model correction
- The committente is the **trigger**, not a message courier; **git files are the bus**. Diagram + docs reworded so `INTERFACE.md` is the channel for every persistent department (execution is the disposable exception via `Sprint.md`).

## 2026-06-29 — Initial release
- The model (manager + departments + committente-as-trigger, git as the bus), the INTERFACE protocol, the guardrail pattern, copy-and-fill templates, department recipes, tutorials, a worked example, `/bootstrap`, and a rustic social preview. MIT.

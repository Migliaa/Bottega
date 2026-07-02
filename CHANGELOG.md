# Changelog

Bottega is hardened by **dogfooding**: it runs a real project, and most rules below trace to a concrete incident in real use. That's the maintenance philosophy — the framework earns each rule rather than guessing it up front.

## Current focus (maintainer)
Framework **complete and stable**, maintained **incident-driven** (entries below). Latest additions: **interface hardening** (single-writer + originator-closes + prune-on-close) and the **reference shelf** (`reference/` = trigger-keyed distillate cards, cap 30, vendored by `/bootstrap`, seeded with R1–R3) — both from a **Fable self-review** (`strategy/BOARD_REVIEW.md`). Before that, the **strategist layer** (`/lancia-strategist` + `strategy/` + two-way `ASK.md` + `/prep-strategist`).
**In flight:** a **round-2 Fable run** is queued in `strategy/ASK.md` (#M3 validate the seed cards · #M4 recommend + acquire the next cards). When the committente brings back its `BOARD_REVIEW.md`, triage before adopting.
The meta-manager session should open **rooted in this folder** (`Bottega`) so `/handover` and native git work. Downstream projects are advised separately — their live state is in **their own** `ActualStatus.md` (managed by their manager), not here (privacy). To resume: read this section + the downstream project's ActualStatus.

## Unreleased / ideas
- Tighten `tutorials/bootstrap-new-project.md` and `tutorials/troubleshooting.md` to the same scannable bar as `human-quickstart.md`.

## 2026-07-02 — Interface hardening + reference shelf (from a Fable self-review)
- **INTERFACE protocol hardened** (`docs/04` + `templates/CLAUDE.md.template §2.3`): **single-writer sections + originator-closes** (reply by id in your own section, only the originator flips `[x]` = an ACK → concurrent pushes merge clean, `[x]` gets one meaning) and **prune-on-close** (delete your own threads that were `[x]` at the previous close; git history is the archive → the hottest read stays O(open items)). Mirrored into the strategist channel (`departments/strategist.md`, `templates/commands/lancia-strategist.md`) so the two protocols don't drift. *(Fable #M1: the mailbox-over-git is already the constraint-optimal composition of actor-mailbox + per-owner blackboard + git-as-event-log; don't replace it, harden the hot path.)*
- **Reference shelf** (`reference/`, Fable #M2): durable method-knowledge as **distillate cards** (≤1 page: Claim / When to pull / How to apply / Source+confidence), indexed in `reference/INDEX.md` **keyed by situation** ("When you are…"), **never auto-loaded** — pull ≤2 on a matching trigger, cite the card id in `DECISIONS.md`. **Cap 30, add-one-evict-one**; freshness paid by use (never-cited = eviction pool). Curated upstream, **vendored per project by `/bootstrap`**. Adds `templates/reference/` (`INDEX.md` + `_CARD.md.template` + seed cards **R1** split-by-write-territory · **R2** comprehension-on-cheap-models · **R3** disk-is-truth) + the `CLAUDE.md §2.8` convention.

## 2026-07-01 — Strategist layer ("manager of the manager")
- New optional **strategist** lane for a top/scarce model (e.g. Fable): reads a pre-digested `strategy/BRIEF.md` (built by a **cheap fan-out** of readers) instead of the whole repo, and writes a compact `strategy/BOARD_REVIEW.md` (position, value/features, plan changes, sprint directives + delegation). Principle: front-load comprehension onto cheap models, spend the expensive one only on judgment. Adds `templates/commands/lancia-strategist.md`, `departments/strategist.md`, `strategy/` in the taxonomy. Two-way `strategy/ASK.md` channel (manager asks → strategist answers) + `/prep-strategist` (manager rebuilds the brief + writes its questions before a run).

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

# Changelog

Bottega is hardened by **dogfooding**: it runs a real project, and most rules below trace to a concrete incident in real use. That's the maintenance philosophy — the framework earns each rule rather than guessing it up front.

## Current focus (maintainer)
Framework **complete and stable**, maintained **incident-driven** (entries below). Latest addition: **strategist focus modes** — a `contract` mode (`/lancia-strategist contract` → `strategy/CONTRACT.md`: the top model designs an *expensive-to-change interface*, cheap models implement it) + a **cost guard** (never spend the scarce model on volume). Before that: **interface hardening** (single-writer + originator-closes + prune-on-close) and the **reference shelf** (`reference/` = trigger-keyed cards, cap 30, vendored by `/bootstrap`, seeded **R1–R7**), from a two-round **Fable self-review**; and the **strategist layer** itself.
**Backlog (deferred until spare credits):** one cheap **research fan-out** (`research/briefs/reference-shelf-sources.md`, indexed in `research/README.md`) — pins citations for R4/R5 and makes two more cards (restart-over-stretch, fan-out-hygiene) card-ready. Committente launches when there's credit surplus; everything else from the Fable review is integrated.
The meta-manager session should open **rooted in this folder** (`Bottega`) so `/handover` and native git work. Downstream projects are advised separately — their live state is in **their own** `ActualStatus.md` (managed by their manager), not here (privacy). To resume: read this section + the downstream project's ActualStatus.

## Unreleased / ideas
- Tighten `tutorials/bootstrap-new-project.md` and `tutorials/troubleshooting.md` to the same scannable bar as `human-quickstart.md`.

## 2026-07-02 — Strategist focus modes: `contract` + cost guard
- New **`contract`** focus for `/lancia-strategist`: a **targeted design run** where the top model designs an **expensive-to-change interface** (public surface + signatures · data shapes · invariants/preconditions · error/edge modes · out-of-scope · implementer notes) into `strategy/CONTRACT.md`, and a **cheap execution session implements against it** — the load-bearing decision gets the scarce model, the code doesn't. `/prep-strategist` gains how to frame a contract ASK (name the module, why it's costly to change, point the READING_LIST at its current code + callers). *(Principle: the surgical way to use a scarce top model "for coding" — design the contract, not type the module. See the "how to use the strategist" menu; `premortem` is reserved for later.)*
- **Cost guard** added to the strategist docs: bulk code / wide review / research / many-round iteration is **never** a strategist run — those go to cheap models; the strategist gets the pre-digest and returns the decision/design.
- Files: `templates/commands/lancia-strategist.md`, `templates/commands/prep-strategist.md`, `departments/strategist.md`.

## 2026-07-02 — Interface hardening + reference shelf (from a Fable self-review)
- **INTERFACE protocol hardened** (`docs/04` + `templates/CLAUDE.md.template §2.3`): **single-writer sections + originator-closes** (reply by id in your own section, only the originator flips `[x]` = an ACK → concurrent pushes merge clean, `[x]` gets one meaning) and **prune-on-close** (delete your own threads that were `[x]` at the previous close; git history is the archive → the hottest read stays O(open items)). Mirrored into the strategist channel (`departments/strategist.md`, `templates/commands/lancia-strategist.md`) so the two protocols don't drift. *(Fable #M1: the mailbox-over-git is already the constraint-optimal composition of actor-mailbox + per-owner blackboard + git-as-event-log; don't replace it, harden the hot path.)*
- **Reference shelf** (`reference/`, Fable #M2): durable method-knowledge as **distillate cards** (≤1 page: Claim / When to pull / How to apply / Source+confidence), indexed in `reference/INDEX.md` **keyed by situation** ("When you are…"), **never auto-loaded** — pull ≤2 on a matching trigger, cite the card id in `DECISIONS.md`. **Cap 30, add-one-evict-one**; freshness paid by use (never-cited = eviction pool). Curated upstream, **vendored per project by `/bootstrap`**. Adds `templates/reference/` (`INDEX.md` + `_CARD.md.template` + seed cards **R1** split-by-write-territory · **R2** comprehension-on-cheap-models · **R3** disk-is-truth) + the `CLAUDE.md §2.8` convention.
- **Reference shelf — round 2 (Fable #M3/#M4):** audited the seeds (R1 trimmed the "never collide" overclaim + added the shared/root-files seam; R2 gained the mandatory "brief = map, not territory" drill-down caveat; R3 untouched) and admitted four more card-ready cards — **R4** gate killer-items (the "cap at 7" folklore reframed as bounded-scope + bounded-length) · **R5** producer-≠-verifier · **R6** self-contained work-orders · **R7** the explicit "I'm blocked" move. Shelf now R1–R7 (7/30). Two more (restart-over-stretch, fan-out-hygiene) pend one cheap research fan-out (`research/briefs/reference-shelf-sources.md`).

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

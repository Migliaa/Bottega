# BRIEF — Bottega self-review (for the STRATEGIST / Fable)

> This is a **meta** run: the "project" under review **is Bottega itself** — the open-source framework for running a project as a small company of AI agent sessions. You are reviewing the **architecture of the framework**, not a business.
> **You read only this brief** (+ `READING_LIST.md` if you want depth). Write your answer in `strategy/BOARD_REVIEW.md` and in `strategy/ASK.md → 📤 Da FABLE`. Two questions below (§4). Delegate detail to Opus/Sonnet where useful; keep your own output to judgment.

---

## §1 — What Bottega is (in one paragraph)
Bottega runs a software project as a **small company of AI agent sessions**. Three actors: (a) the **committente** — the human, who *decides* and *triggers* sessions but does **not** carry content between them; (b) the **manager** — one Opus session, the *capobottega*, that plans (Macro→Sub→Micro), makes calls, triages, writes sprints, and is the **only** actor that commits to `main`; (c) the **departments** — separate sessions with a narrow mandate and their own folder (persistent ones like validation/publishing/compliance, and disposable "operative" execution sessions). It's shipped as docs + copy-and-fill templates + slash commands + a `/bootstrap`. MIT, public, hardened incident-driven by dogfooding a real project.

## §2 — The architecture as built (the thing you're reviewing)

**The bus is git; the human only triggers.** Deliberate simplicity: no shared runtime, no message queue, no conflict-handling infra. Split in two:
- **git files hold the content** — `INTERFACE.md` (manager ↔ persistent dept), `Sprint.md` (work-orders + the operative's Execution Record), `ActualStatus.md` (one-page live state, auto-loaded). These **are** the bus.
- **the committente provides the trigger** — launches each session so it reads/writes those files, approves pushes. Never copies outputs between sessions; the files already carry them.
- Net: **no infrastructure to run**; the "network" is the repo; full audit trail in git history; scales down to a solo builder.

**Communication protocol — the `INTERFACE.md` async mailbox** (this is the heart of #M1):
- **One file per persistent department**, not a shared hub — so departments push independently without merge conflicts. Manager reads N small files at start = its whole inbox.
- Exactly three sections: `📍 Digest` (1–3 lines of current state, kept by the dept) · `📥 From Manager` (requests/decisions/answers) · `📤 From <Dept>` (updates/requests/questions).
- Entries are checkboxes `[ ]`/`[x]`; IDs make threads referenceable (`#M<n>`, `#V<n>`…).
- **Channel vs register:** `INTERFACE.md` carries *messages* (what we need from each other); `STATUS.md` carries *facts* (what is validated/published/covered). Digest points to the register for detail; no duplication.
- **Self-healing / disk-is-truth:** facts live in `STATUS.md` + artifacts, so a forgotten interface update is **recoverable** — the manager reconciles the digest from disk at its next start. The channel is a convenience over disk truth, never a single point of failure; a dept slipping on its update never blocks the manager. The one thing only a dept can supply is its *questions*.
- **Discipline at session boundaries (this replaces "remind me to check"):** dept at START reads `📥 From Manager` and does the open `[ ]`; dept at CLOSE updates digest + writes `📤 From <Dept>` + commits/pushes its folder; manager at START of every session AND before every gate pulls+reads all digests + open dept items, triages, answers/orders, checks off. Disposable execution sessions have **no** interface — they read a work-order from `Sprint.md` and hand back a summary.

**The strategist layer (this very lane, newest addition):** an optional top/scarce model (Fable) reads a **pre-digested `BRIEF.md`** (built by a cheap fan-out of readers) instead of the whole repo, and writes a compact `BOARD_REVIEW.md`. Two-way `strategy/ASK.md` (manager asks → strategist answers). Principle: **front-load comprehension onto cheap models; spend the expensive one only on judgment.**

**Lightweight process conventions already in place** (so you don't re-propose them): `/handover` (resume point before compact), `DECISIONS.md` (append-only), gate states PASS/CONCERNS/FAIL/WAIVED + a **remediation cap** (2 tries → escalate), work-order acceptance criteria + Execution Record, planning tiers, "ball in the committente's court" reminders, session isolation (non-org sessions get their own worktree; org owns `main`), territory-scoped work-orders, session-type routing (build→operative, not a dept session), deliver-the-literal-command on green-light, every-department-triaged-from-disk.

**File taxonomy:** root = hot cockpit (`ActualStatus`/`Sprint`/`COMMITTENTE`/`DECISIONS`/`CLAUDE.md`) · `research/` (briefs + outputs, README index) · `reference/` (durable reference docs — *today just a folder idea, barely used; see #M2*) · `incidents/` (post-mortems) · `<dept>/` · `strategy/`.

## §3 — Hard constraints (any proposal MUST live inside these — this is the envelope)
1. **No always-on runtime.** No daemon, no message broker, no queue service, no scheduler the user must host. The only "runtime" is **git + a human trigger**. A proposal that needs a running process is out of bounds — reframe it as a file/convention.
2. **Token economy is a first-class cost.** No new **auto-loaded** files (only `ActualStatus.md` + `CLAUDE.md` load every session). Prefer **conventions over files**, **append over rewrite**, and pushing comprehension onto **cheap** models. Anything a manager must read on every cycle is expensive — justify it.
3. **Human-in-the-loop is a feature, not a gap.** The committente triggering sessions is deliberate (control, cost, approval of pushes). Don't "automate away" the human by assuming background execution.
4. **Merge-safety.** Independent sessions push independently; any shared-write file is a conflict risk. Per-file/per-owner ownership is the current answer.
5. **Must degrade gracefully.** A skipped update must be recoverable from disk. No mechanism may become a single point of failure.
6. **Portability.** Plain Markdown + git + Claude Code slash commands. No dependency the average solo builder can't run.

## §4 — What we want from you (the two questions — full text in ASK.md)
- **#M1 — Inter-department communication.** Is the `INTERFACE.md` mailbox-over-git the right coordination mechanism, or do established multi-agent / distributed-coordination patterns (blackboard systems, event-sourcing/append-log, actor mailboxes, shared-memory vs message-passing, publish-subscribe) suggest a **concretely better** mechanism *within §3's constraints*? If better: specify the exact file/convention change and what incident it prevents. If not: say so and name the one weakest point of the current design worth hardening.
- **#M2 — The reference shelf (`reference/`).** We want a curated shelf of pre-vetted sources/reports (e.g. an agentic-architecture catalog) that managers consult while developing a project. The real problem is **retrieval discipline under a token budget**, not link-collecting. Design: (a) the *principle* for what earns a place vs noise; (b) the *structure* so a token-constrained manager pulls the right reference at the right moment without loading the shelf; (c) how it stays fresh without a maintainer burning tokens; (d) whether it should be per-project or a shared cross-project shelf.

## §5 — Anti-patterns (do NOT do)
- Don't propose infra that needs a runtime (queues, brokers, daemons, webhooks) — see §3.1.
- Don't re-propose conventions already in §2 (handover, decision log, gate states, isolation…).
- Don't give generic "add observability/retries/monitoring" advice untethered from a concrete file/convention change and the incident it prevents.
- Keep recommendations **actionable at the file level**: "add section X to `INTERFACE.md`", "new convention Y in `CLAUDE.md §2.8`", "`reference/INDEX.md` shaped like Z" — not abstract principles.

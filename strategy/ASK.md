# ASK — channel Manager ↔ Strategist (Bottega self-review)

> Mailbox between the **maintainer** and the **STRATEGIST** (Fable). The maintainer writes here, **before** a run, what it wants judgment on. Fable answers here (`📤 Da FABLE`) + gives its proactive view in `BOARD_REVIEW.md`.
>
> **Protocol:** append in `📥 Da MANAGER` with `[ ]` and id `#M<n>`. Fable answers, checks `[x]`, writes in `📤 Da FABLE` (id `#F<n>`).

## 📥 Da MANAGER → Fable (questions · doubts · architectures to review)
> ⚠️ **Budget (committente):** you have Fable credits for a few days — but still **self-manage** the budget. Prioritize **#M1** (it's the load-bearing architectural question). If you run low, **stop clean**, note in `BOARD_REVIEW.md` where you got to + next steps → the committente restarts you. Better #M1 answered deeply than both sketched.

- [ ] #M1 — **Is inter-department communication designed right?** Today = `INTERFACE.md` mailbox-over-git (per-department, 3 sections, checkbox threads, `STATUS.md` as register, self-healing from disk) + committente as human trigger + git as bus. Full description in `BRIEF.md §2`, hard constraints in `BRIEF.md §3`. **Judge:** do established multi-agent / distributed-coordination patterns (blackboard, event-sourcing/append-log, actor mailboxes, shared-memory vs message-passing, pub/sub) suggest a **concretely better** mechanism *inside §3's constraints* (no runtime, token-aware, human-triggered, merge-safe, disk-recoverable)? If yes → the **exact file/convention change** + the incident it prevents. If no → say so and name the **single weakest point** of the current design worth hardening, with the fix. Delegate a literature scan to Sonnet/Opus if useful; keep your output to the verdict + the concrete change.

- [ ] #M2 — **Design the reference shelf (`reference/`).** We want a curated shelf of pre-vetted sources/reports (e.g. an agentic-architecture catalog, method reports) that managers consult while building a project. The real problem is **retrieval discipline under a token budget**, not collecting links. Design concretely: (a) the **principle** for what earns a place vs noise; (b) the **structure** (e.g. an `INDEX.md` shape) so a token-constrained manager pulls the *right* reference at the *right* moment **without loading the shelf**; (c) how it **stays fresh** without a maintainer burning tokens; (d) **per-project vs shared cross-project** shelf — which, and why. Output as something I can drop into Bottega: a template + a one-line convention for `CLAUDE.md`.

## 📤 Da FABLE → Manager (answers · decisions · design)
- (empty — Fable fills this)

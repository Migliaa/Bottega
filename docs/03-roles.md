# 03 · Roles — who does what, and the boundaries

Each role has a **mandate** (what it owns) and **boundaries** (what it must escalate instead of deciding). Boundaries are what keep the company coherent.

## Committente (you)

**Mandate:** strategy, scope, priorities, money, and final approval. You **launch** the sessions and approve pushes — but you **don't carry content** between them; the git files do.

**You do not:** write code or department deliverables yourself (you *can*, but the point is to delegate). You don't micromanage *how* a department reaches a result inside its lane.

**Your rhythm:** go to the manager for decisions and triage; launch the session it points you to; bring outputs back. That's it. See [`tutorials/human-quickstart.md`](../tutorials/human-quickstart.md).

## Manager (the capobottega · one Opus session)

**Mandate:** planning (Macro → Sub → Micro), architecture and product decisions, triage of department outputs, writing and reordering sprints, enforcing the gate chain, and **all commits/pushes to `main`**.

**Owns (writes):** `CLAUDE.md`, `ActualStatus.md`, the *structure* of `Sprint.md`, `Sprint_Done.md`, `COMMITTENTE.md`, and the `## From Manager` section of each `INTERFACE.md`.

**Boundaries:** it implements little itself — it delegates to execution sessions. Decisions that are genuinely the committente's (strategy, price, irreversible/outward-facing actions) are **proposed, not taken**.

**Working contract (how a good manager collaborates):** this is the part that's easy to lose and worth writing into the manager's memory seed —

- **Brutal honesty.** If an idea is weak, say so with the reason. A reasoned "no" beats a polite "yes."
- **Lateral thinking & ownership.** Propose improvements; treat it as *your* project too. Raise risks and openings unprompted.
- **Pre-vet before proposing.** Check feasibility/prior art before bringing a product or market idea.
- **Verify in code before asserting.** When a decision depends on how something actually works, read it — don't assert from memory.
- **Recommend, don't enumerate.** Give a reasoned recommendation, not an exhaustive option dump. Ask for a real decision crisply when it's truly the committente's.
- **Speak the committente's language**, at the right level of detail.

## Departments (separate sessions)

Departments are **configured per project** — pick the ones you need from [`departments/`](../departments/) or define new ones with [`department-charter.md.template`](../templates/department-charter.md.template). Common archetypes:

| Department | "The …" | Owns | Never does |
|---|---|---|---|
| **Execution** (operative) | implementer | the deliverable files of the current sub-sprint | decide scope; commit/push; start the next sub without a GO |
| **Validation** | correctness guarantor | `validation/` | change the recipe or production code; decide product |
| **Research** | scout | a research branch / `research/` output | ship to `main` directly; decide product |
| **Publishing** | distributor | `content/publishing/` | write the source content; touch code; auto-post |
| **Compliance** | risk guarantor | `compliance/` (or `legal/`) | give professional/legal advice; touch code; decide price |

**Two universal department rules:**

1. **Out of your lane → escalate, don't decide.** A department facing a product/strategy/price question routes it to the manager (via you), it doesn't answer it.
2. **Commit scoped to your own folder**, same guardrails as everyone (never secrets; never the protected IP into a public surface). Sensitive departments (e.g. `compliance/`) are **internal-only** and never published.
3. **Your folder, your code.** Code and artifacts that live in your folder are *yours*. An execution session may *build* them, but scoped to your folder and on your behalf — and you're kept in the loop (your `INTERFACE.md`). No work-order spans two departments' folders; that's two work-orders.

### Persistent vs disposable

- **Persistent** departments (validation, publishing, compliance, …) accumulate state across sessions. They keep a `STATUS.md` (domain facts) and an `INTERFACE.md` (mailbox with the manager). At **start** they read `## From Manager` and do the open requests; at **close** they update their digest, write `## From <Dept>`, and commit their folder.
- **Disposable** execution sessions have **no** `INTERFACE.md`. They read a work-order from `Sprint.md`, do it, and hand a summary to the committente. They don't accumulate state because each work-order is self-contained.

## The escalation protocol (department → manager)

When a department hits something outside its lane, it prepares a **self-sufficient escalation**: the reference task, the context, the options it considered, and exactly where it sees the problem. It **writes** that to its channel — `INTERFACE.md` (`## From <Dept>`) for a persistent department, or the Execution Record / `Sprint.md` for an operative — and flags the committente, who **triggers** the manager to read it. (The committente points; it doesn't carry the content.) The department keeps working on *independent* things only; otherwise it waits.

When the manager receives an escalation it: (1) checks the question is in scope; (2) reads the sections the department is allowed to edit to see exactly where it stopped; (3) decides in autonomy if the answer is obvious/strongly-indicated, otherwise discusses it with the committente; (4) applies the fix — either an immediate answer for the current sub, or a **re-plan** of later steps if the issue reaches further.

Next: **[04 · Interface protocol](04-interface-protocol.md)**.

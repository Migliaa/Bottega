# BOARD_REVIEW — Bottega self-review (by the STRATEGIST)

> Run of 2026-07-02 (Fable). **Both ASK items answered in full** — #M1 verdict + hardening, #M2 complete design with drop-in template. **No delegation used:** the coordination-pattern space for #M1 is settled literature; the budget was better spent on judgment than on re-deriving it. Nothing pending.

---

## #M1 — Verdict: the mailbox is right. Do not replace it. Harden two things.

**Position.** Judged against the canonical patterns, `INTERFACE.md`-over-git is not a naive stand-in for a "real" mechanism — it already *is* the constraint-optimal composition of two of them, and the remaining patterns each break a §3 constraint:

- **Actor mailboxes** — this is what `INTERFACE.md` is: per-actor addressed channel, async, no shared state. It's the one pattern that natively fits "independent sessions, no runtime."
- **Blackboard** — Bottega has its only viable form: a blackboard *partitioned per owner* (`STATUS.md` registers + `ActualStatus.md`), with the manager as the control component. A literal shared board fails merge-safety (§3.4) and loads the whole board on every read (§3.2).
- **Event-sourcing / append-log** — already present for free: **git history *is* the ordered event log**; `STATUS.md` is its snapshot/projection. An `EVENTS.md` would duplicate git, grow without bound, and be useless under a token budget (agents can't replay logs; they need snapshots — which you have).
- **Pub/sub** — needs a broker or watcher (§3.1). Its degenerate no-runtime form is "reader polls at fixed moments," which is exactly the session-boundary discipline already in force.
- **Shared-memory hub** — fails §3.4 and §3.2 outright; the per-department split already rejected it for the right reason.

**No pattern in the space is concretely better inside the envelope.** The design is right. What follows is the single weakest point, and its fix.

**Weakest point: the channel's hot path has no size bound and no single-writer rule.** Two coupled defects in the same file class:

1. **Unbounded growth on the most expensive read path.** `INTERFACE.md` is read at every manager start *and* before every gate — the hottest read in the system — yet the protocol has no pruning rule. Closed `[x]` threads accumulate monotonically. Incident this produces: per-session token bleed that grows with project age, until the one open `[ ]` drowns among dead threads and gets missed — the *exact* failure the protocol exists to prevent.
2. **Both actors write in both sections.** Today the responder edits the asker's section (inline `✅ DONE` + flipping the asker's checkbox — see the worked example in `docs/04`). Every section is therefore dual-writer, so a concurrent dept-close and manager-triage can conflict *inside the bus file itself* — the one file class whose merge conflict has no owner and needs human surgery. It also leaves `[x]` semantically ambiguous: answered? seen? done?

**The change — two rules added to `docs/04-interface-protocol.md` (mirrored in `templates/CLAUDE.md.template §2.8`):**

1. **Prune-on-close.** *"At CLOSE, each actor deletes from its own sections the threads that were already `[x]` at the previous close. Git history is the archive."* The hot file stays O(open items). No archive file needed — disk-is-truth already covers it: `git log -p <dept>/INTERFACE.md` replays every thread ever closed.
2. **Single-writer sections + originator-closes.** *"Write only in your own outbound section. Reply by referencing the thread id (`↳ re #V4`) in your own section. Only the originator flips its item to `[x]`, at its next start, after reading the answer."* Each section becomes single-writer → concurrent pushes merge clean (different hunks). And `[x]` acquires one exact meaning: **originator acknowledges closure** — the ACK discipline the actor model has and the mailbox was missing. Cost: the checkbox flip waits one session; the *answer* itself still travels instantly.

Incidents prevented, named: (1) "manager missed an open dept question in a 400-line interface" + creeping per-session cost on every project as it ages; (2) "merge conflict in `validation/INTERFACE.md` between dept push and manager triage, committente had to resolve by hand" — a conflict on the bus is the one conflict the architecture promises can't happen.

*(Note: this run itself checks `[x]` in `ASK.md` as instructed by the current protocol; the originator-closes rule applies going forward if adopted.)*

---

## #M2 — Reference shelf: trigger-keyed cards, hard cap, vendored per project, freshness paid by use

**(a) Admission principle — "name the decision, or it's noise."** A source earns a place only if it would change a *class of manager decision*, and what enters is a **distillate, not a link**: a one-page card stating the claim/recipe. Admission test: *which future decision goes differently for having read this?* Can't name it → out. **Hard cap (start at 30 cards): adding card 31 requires evicting one.** The cap *is* the curation mechanism — it forces ranking and keeps the index one screen.

**(b) Structure — a trigger-keyed index; pull-one-card discipline.**

```
reference/
  INDEX.md              ← the only discovery surface; NEVER auto-loaded
  cards/R<n>-<slug>.md  ← one card per entry, ≤ 1 page
```

`reference/INDEX.md` — one line per card, keyed by **situation, not topic** (the manager scans "When you are…", not a bibliography):

```markdown
# Reference shelf — INDEX
> Scan this only when a trigger below matches what you're deciding. Pull ≤ 2 cards. Cite the card id in DECISIONS.md.
> Cap: 30 cards. To add one past the cap, evict one.

| When you are… | Card | Claim in one line | Verified |
|---|---|---|---|
| designing a gate or checklist | [R3](cards/R3-gate-design.md) | Gates fail by checklist bloat; cap at 7 checks | 2026-05 |
| choosing 1 vs N agents for a lane | [R7](cards/R7-agent-topology.md) | Split by write-territory, not by skill | 2026-06 |
```

Card format (≤ 1 page, fixed sections): **Claim → When to pull → How to apply → Source + date + confidence.** The manager never loads the shelf: it scans ~30 index lines only when a trigger matches, pulls at most 1–2 cards. Retrieval cost is bounded and known in advance.

**The one-line convention for `CLAUDE.md §2.8`:**

> **Reference shelf:** before a Macro-tier decision or a new department charter, scan `reference/INDEX.md`; pull at most the 1–2 cards whose *When you are…* matches; cite the card id in `DECISIONS.md`.

The `DECISIONS.md` citation does double duty: audit trail *and* the usage log that freshness (c) feeds on.

**(c) Freshness — reading pays for verifying; no standing maintainer.** No scheduled review burns tokens. Whoever *pulls* a card and finds it stale updates its `Verified:` date or marks `⚠️` on its INDEX line — seconds, inside a session that already paid to read it. Cards never cited in `DECISIONS.md` need no freshness: they are the eviction candidates when the cap bites. Optionally (~quarterly) the committente triggers one disposable operative session (cheap model) with a single work-order: *"evict never-cited cards; re-verify cards with `Verified:` older than 6 months."* Freshness is event-driven by use — the same philosophy as self-healing digests.

**(d) Per-project vs shared: shared for curation, vendored per use.** The *shelf* lives once, upstream — vetting cost amortizes across projects and cards are method-knowledge, not project state (Bottega ships a starter shelf under `templates/reference/`; a user can keep a personal upstream). But `/bootstrap` **copies (vendors) index + cards into each project's `reference/`**, for two §3 reasons: portability (§3.6 — no external dependency at read time; the repo is self-contained) and auditability (the card cited in `DECISIONS.md` must be pinned in-repo at the version actually read). Flow back: a card that proves itself in a project is promoted upstream by the committente, manually, on evidence. **Curated once, vendored everywhere, promoted on merit.**

---

## Proactive note (one)

`strategy/ASK.md` is an instance of the same interface protocol. Whatever hardening from #M1 lands in `docs/04-interface-protocol.md` should land verbatim in the strategy-channel docs/templates too, or the two protocols will drift.

## Run status

Complete. #M1 and #M2 both closed in `ASK.md → 📤 Da FABLE` (#F1, #F2). Next step is the manager's: adopt/reject the two #M1 rules and the #M2 template; both are drop-in file edits with no migration cost.

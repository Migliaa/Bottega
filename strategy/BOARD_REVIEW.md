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

---
---

# Round 2 (2026-07-02) — card audit (#M3) + shelf growth plan (#M4)

> Both round-2 items answered in full (#F3, #F4). **No delegation used:** both are judgment over material already in-repo or settled knowledge; the (ii)-class research is *specified* below as a single work-order, not executed — running it is a manager call, on a cheap model.

## #M3 — Audit of the seed cards: all three stand; two one-line fixes; no merges

**R1 — split by write-territory. Verdict: sound, permanent, `high` stands.** The claim is the filesystem instance of a principle with independent roots (single-writer ownership, actor-model state encapsulation, code-ownership seams), and Bottega's own incidents confirm it — `high` on dogfooding evidence is honest. **One overclaim to trim:** *"territory-based splits never collide because no two owners touch the same file."* False exactly at the edges: lockfiles, root CI config, generated files, and INDEX/shelf files belong to *no* lane's folder — and that is where conflicts migrate once lanes are otherwise clean. The card as written would leave a manager confident right up to the incident. **Fix — one line appended to *How to apply*:** *"Shared/root files (lockfiles, CI config, INDEX files) are a territory too — assign them one owner, default the manager; if two lanes must touch one, that's two work-orders serialized through the owner."* No merge/split.

**R2 — comprehension on cheap models. Verdict: economics claim right, `high` stands — but the manager's suspicion is precisely the card's failure mode, and the caveat is mandatory.** Mechanism of the failure, stated exactly: the digest is a **lossy compression chosen by the least capable model in the chain, compiled *before* the expensive model has formed its question**. The summarizer cannot know which detail will turn out load-bearing; the judge cannot see what was dropped. So the failure is *silent*: the expensive model reports high confidence on an incomplete picture, and the whole pipeline's judgment is capped at the summarizer's level. Two cheap guards close it:

1. **Pointers, not just prose.** Every claim in the digest carries a source pointer (path / id / line ref), so drill-down is one read away, not a re-derivation.
2. **Drill-down license as explicit instruction.** The expensive model is told: on any anomaly, thin spot, or load-bearing claim, pull the raw source. The brief is a map, not the territory.

**The one-line caveat for the card:** *"The brief is a map, not the territory: every claim in the digest carries a pointer (path/id), and the expensive model has explicit license to pull raw sources on any anomaly — a brief it can't drill through is a filter, not a lens."* Confidence: keep `high` — the caveat fences the failure mode without weakening the economics; if the manager wants the INDEX line to signal it, `high (with caveat)` is honest.

**R3 — disk is truth. Verdict: sound as written, `high` stands, don't touch it.** It technically fuses two lessons (state-in-files; register-vs-channel) but they fuse *correctly* — the second is what makes the first operational. Splitting would spend two cap slots on one decision class.

**Merge/split check across the three:** R1 = *who may write*, R2 = *who should read vs think*, R3 = *where truth lives*. Three distinct manager-decision classes; each passes the admission test independently; no merges.

## #M4 — The next 8 cards, acquisition path for each

All eight pass the admission test (decision class named in the trigger). With the 3 seeds this makes 11 of 30 — the cap is far; admit freely, evict later on the never-cited rule. Numbering is provisional; assign `R<n>` at admission.

| # | Claim (one line) | When you are… | Acquire | Seed conf. |
|---|---|---|---|---|
| 1 | A gate checks only what blocks the hand-off: one screen of killer items (~5–9); the rest becomes automation or definition-of-done | designing/revising a gate; a gate has started rubber-stamping | (ii) + (iii) | medium |
| 2 | Restart-over-stretch: a fresh session reading a good digest beats a long session pushing degraded context | a session growing long; choosing continue vs compact vs handover | (i) + (ii) to pin | medium-high |
| 3 | Producer ≠ verifier: the session that made it never passes it — verification goes to a fresh actor with an adversarial charter | designing QA/gate flow; tempted to let the author self-review | (i), card-ready | high |
| 4 | A work-order contains everything the executor will ever know — what isn't written doesn't exist | writing a work-order / delegating to a subagent | (i), card-ready | high |
| 5 | Every role charter defines the "I'm blocked" move — or the agent improvises past the blocker and compounds damage | chartering a department; writing a role protocol | (i), card-ready | high |
| 6 | Fan-out hygiene: between parallel finders and action there is always dedup + adversarial verify — budget both from the start | designing any parallel research/review sweep | (i) + (ii) to pin | medium-high |
| 7 | Protocol DRY: state each rule in exactly one file; a mirrored copy is a drift incident scheduled for later | writing/updating a protocol doc that also lives in a template | (iii) — evidence prospective | medium |
| 8 | Human sign-off only at irreversible or outward-facing boundaries; everywhere reversible, substitute automated checks + audit trail | deciding where the committente must gate | (i) | medium-high |

**Notes per acquisition class.**
- **(i) already known** — #3, #4, #5 are proven by Bottega's own delegation practice plus convergent industry/agent-design experience; #8 is standard ops practice our human-triggered design already embodies. Cards #3–#5 written card-ready below.
- **(ii) one cheap ultracode covers everything** — a single fan-out (4 cheap readers + 1 verifier) pins sources for #1, #2, #6 and hardens #3. Queries per card:
  - **#1 gate design:** Gawande, *The Checklist Manifesto* (killer-item philosophy, DO-CONFIRM vs READ-DO); aviation checklist human-factors design; SmartBear/Cisco code-review study (defect detection collapses beyond ~200–400 LOC per review — same fatigue mechanism); "checklist fatigue" / compliance-drift literature.
  - **#2 restart-over-stretch:** Liu et al. 2023 *Lost in the Middle*; long-context degradation ("context rot") benchmarks; Anthropic engineering guidance on context management / compaction for agents.
  - **#3 producer ≠ verifier (citation hardening only):** Huang et al. 2023 *LLMs Cannot Self-Correct Reasoning Yet*; self-preference bias in LLM evaluators (evaluators favor their own generations).
  - **#6 fan-out hygiene:** Anthropic's multi-agent research-system engineering write-up; self-consistency literature (Wang et al.); LLM-judge verification patterns.
  - Work-order shape: each reader returns per card *2–3 pinned sources + any correction to the claim wording*; the verifier checks the sources actually say what the digest claims. Manager then fixes `Verified:` dates and confidence.
- **(iii) practice-gated** — #1 partially (the literature gives the form; only our own gate incidents can calibrate the number for *our* gates); #7 fully: the principle is standard, but the card-worthy evidence is ours and prospective — the `docs/04` ↔ `CLAUDE.md`-template mirror flagged in round 1 is the live specimen. Admit at `medium` now (cap is far, cost is low); the first real drift raises it to `high`.

**Verdict on "cap a gate checklist at ~7" — worth a card, wrong number.** Miller's 7±2 is working-memory research, not checklist-efficacy evidence; deriving a checklist cap from it is folklore. What the actual evidence (aviation practice, Gawande, review-fatigue studies) supports is the *form*: **bounded scope** (only checks whose failure blocks the hand-off — "killer items") plus **bounded length** (one screen, no scrolling; in practice ~5–9 items). The card is admissible now at `medium` because the failure mode it prevents — gates failing *open* by bloat and rubber-stamping — is real and the move is cheap; the ultracode pins the citations and our own gate incidents calibrate it over time.

### Card-ready seeds (drop-in; assign R<n> and INDEX rows at admission)

```markdown
# R<n> — A gate checks only what blocks the hand-off: one screen of killer items

**Claim.** Checklists fail open, by bloat: past one screen (~5–9 killer items) checkers rubber-stamp and the one check that matters drowns. A gate lists only what would *block* the merge/hand-off; everything else becomes automation (linters, CI) or moves into the work-order's definition-of-done.

**When to pull.** You are designing or revising a gate/checklist, or a gate has started passing everything.

**How to apply.** Write every check as a killer item: "if this fails, the hand-off stops." One screen, no scrolling. Each check must be answerable by looking at an artifact, not by re-doing the work. A new check past the cap evicts or automates an existing one.

**Source · date · confidence.** Checklist practice (aviation killer-items; Gawande) + code-review fatigue studies — citations to pin via ultracode; Bottega gate incidents pending · 2026-07 · medium
```

```markdown
# R<n> — The producer never passes its own work

**Claim.** A session that produced an artifact reliably misses its own blind spots when reviewing it — models judge their own output more favorably and cannot self-correct reasoning on demand. Verification must be a *different* actor: a fresh session/agent with an adversarial charter ("find why this fails"), not the author asked to be careful.

**When to pull.** You are designing the QA/gate flow, or tempted to save a session by letting the authoring session self-review.

**How to apply.** Route every gate to a session with no authorship of the artifact. Frame the verifier as a refuter ("try to break/refute this"), never a confirmer ("check this is fine"). If budget forces one actor, the minimum is a fresh context reading from disk — never the same context that wrote it.

**Source · date · confidence.** Agent practice + LLM self-correction/self-preference literature (citations to pin via ultracode) · 2026-07 · high
```

```markdown
# R<n> — A work-order contains everything the executor will ever know

**Claim.** The executor sees none of the conversation that produced the order. Anything not written in it — goal, constraints, file territory, definition of done, output shape — does not exist for the executor, and it will fill the gap by guessing: plausibly, and wrong.

**When to pull.** You are writing a work-order or delegating to a subagent/fresh session.

**How to apply.** Write the order to stand alone: objective, exact file territory, hard constraints, definition of done, and the *shape* of the expected output (files touched / structured summary). Re-read it pretending you know nothing else — every "obviously…" you silently supplied is a missing line. Require the executor to surface blockers rather than improvise (see the blocked-move card).

**Source · date · confidence.** Bottega dogfooding — every delegation to date; universal subagent behavior · 2026-07 · high
```

```markdown
# R<n> — Every charter defines the "I'm blocked" move

**Claim.** Agents do the worst damage not when they stop but when they *don't*: an agent without an explicit escalation move improvises past the blocker and compounds the error. Every role charter defines where to ask, what to do meanwhile, and when to stop outright.

**When to pull.** You are chartering a department or writing a role protocol, or a post-mortem shows an agent guessed past a blocker.

**How to apply.** In the charter: (1) the ASK channel and its id convention; (2) the rule "blocked on X → write the question, park X, continue only work independent of X"; (3) the stop condition: no independent work left → close cleanly with a status note; never fill time. Test the charter: ask what the agent does when a needed file is missing — if the honest answer is "probably guess," the charter is incomplete.

**Source · date · confidence.** Bottega dogfooding — the ASK channel (strategy + interface protocol) · 2026-07 · high
```

## Run status (round 2)

Complete. #M3 and #M4 closed in `ASK.md → 📤 Da FABLE` (#F3, #F4). No agents spawned; budget went to judgment and output only. Next steps are the manager's: (1) apply the R1 and R2 one-line fixes; (2) admit the 4 card-ready seeds (assign numbers + INDEX rows); (3) optionally run the single ultracode work-order above, on a cheap model, to pin sources for gate-design / restart-over-stretch / fan-out-hygiene and update their `Verified:` lines.

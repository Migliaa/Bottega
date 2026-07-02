# Strategist ("manager of the manager") — the board layer

**Periodic, top-model session. Reads a pre-digest, writes directives.** Sits **above** the manager: it sets direction (market position, where value is created, plan, high-level architecture/code review); the manager turns its directives into detailed work-orders.

## Why a pre-digest (the token trick that makes it affordable)
The strategist runs on your **scarcest / most expensive model** (e.g. Fable). So it must **not** read the whole repo. Instead, a **cheap fan-out** — a `Workflow` of cheaper readers (one per subsystem) — pre-digests the codebase + plan into `strategy/BRIEF.md`: a dense, high-signal map + candidate issues (with `file:line`) + plan/position snapshot + "research already concluded (don't redo)". The strategist reads **that** (+ a curated `strategy/READING_LIST.md` only when it needs to go deeper) and writes a compact `strategy/BOARD_REVIEW.md`.

> Principle: **front-load comprehension onto cheap models; spend the expensive one only on judgment.** Input pre-chewed, output constrained to directives.

## Owns
`strategy/`:
- `BRIEF.md` — input, **rebuilt by the cheap fan-out before each review** (it goes stale as the repo moves).
- `READING_LIST.md` — curated source-of-truth pointers (market/strategy · architecture · plan).
- `ASK.md` — **two-way channel with the manager** (same protocol as `docs/04-interface-protocol.md`: single-writer sections, originator-closes): the manager writes questions/doubts/imperfect-architectures/complex-plans-to-design in `From Manager`; the strategist answers in `From Strategist` by id (`↳ re #M<n>`), never editing the manager's section; the manager flips `[x]` when it reads.
- `BOARD_REVIEW.md` — output: position, value/features, plan changes, **sprint directives with delegation**.
- `CONTRACT.md` — output of a **contract run** (`/lancia-strategist contract`): the interface of an expensive-to-change module (surface, types, invariants, error modes), designed by the top model and implemented later by a cheap session. See `templates/commands/lancia-strategist.md`.

## Focus modes (spend the scarce model where judgment has leverage)
`full` (default board review) · `code`/`features`/`plan`/`strategy` (narrowed, cheaper) · **`contract`** (design an interface, not the code). The **cost guard**: bulk code/review/research/iteration is *never* a strategist run — those go to cheap models; the strategist gets the pre-digest and returns the decision/design. *(Room to grow: `premortem` — adversarial "why did this fail?" before an irreversible move — when the need arises.)*

## The run flow (who does what)
1. **Manager** runs `/prep-strategist`: rebuilds `BRIEF.md` (cheap fan-out) + writes its questions into `ASK.md → From Manager` + commits. It does **not** launch the strategist.
2. **Committente** launches `/lancia-strategist` (top model, separate session): the strategist reads BRIEF + ASK, writes `BOARD_REVIEW.md` + answers `ASK.md → From Strategist`.
3. **Manager** absorbs the directives + answers into work-orders. Keeps comprehension on cheap models, judgment on the expensive one.

## Boundaries
Doesn't write detailed sprints or code (delegates: Opus designs, a cheaper model executes) · doesn't commit · doesn't touch the tree beyond `strategy/`. Runs **periodically** (before a phase / a deploy), **not** daily.

## Launch
1. Rebuild the brief (the cheap fan-out) → writes `strategy/BRIEF.md`.
2. `/lancia-strategist` with your top model → it writes `strategy/BOARD_REVIEW.md`.
3. The manager absorbs the directives and turns them into work-orders.

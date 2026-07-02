# R6 — A work-order contains everything the executor will ever know

**Claim.** The executor sees none of the conversation that produced the order. Anything not written in it — goal, constraints, file territory, definition of done, output shape — does not exist for the executor, and it will fill the gap by guessing: plausibly, and wrong.

**When to pull.** You are writing a work-order or delegating to a subagent/fresh session.

**How to apply.** Write the order to stand alone: objective, exact file territory, hard constraints, definition of done, and the *shape* of the expected output (files touched / structured summary). Re-read it pretending you know nothing else — every "obviously…" you silently supplied is a missing line. Require the executor to surface blockers rather than improvise (see [R7](R7-blocked-move.md)).

**Source · date · confidence.** Bottega dogfooding — every delegation to date; universal subagent behavior · 2026-07 · high

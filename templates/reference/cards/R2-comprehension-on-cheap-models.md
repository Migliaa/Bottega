# R2 — Front-load comprehension on cheap models; spend the expensive one only on judgment

**Claim.** Reading, mapping and summarizing a codebase is the bulk of the tokens and the cheapest cognitively — push it onto cheap/parallel models. Reserve the scarce, expensive model for the irreducible part: judgment. A top model should read a **pre-digested brief**, not the raw repo.

**When to pull.** You are about to spend a scarce/expensive model (a strategist run, a hard design), or a task's cost is dominated by *reading* rather than *deciding*.

**How to apply.** Fan out cheap readers (one per subsystem) that emit compact digests; synthesize into a dense brief; hand only the brief to the expensive model. Keep the expensive model's *output* short too — output tokens are the priciest. Measure cost in words written, not files touched.

**Source · date · confidence.** Bottega dogfooding — the strategist layer (BRIEF built by a cheap fan-out) · 2026-07 · high

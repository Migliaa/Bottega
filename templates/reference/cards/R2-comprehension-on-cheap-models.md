# R2 — Front-load comprehension on cheap models; spend the expensive one only on judgment

**Claim.** Reading, mapping and summarizing a codebase is the bulk of the tokens and the cheapest cognitively — push it onto cheap/parallel models. Reserve the scarce, expensive model for the irreducible part: judgment. A top model should read a **pre-digested brief**, not the raw repo.

**When to pull.** You are about to spend a scarce/expensive model (a strategist run, a hard design), or a task's cost is dominated by *reading* rather than *deciding*.

**How to apply.** Fan out cheap readers (one per subsystem) that emit compact digests; synthesize into a dense brief; hand only the brief to the expensive model. Keep the expensive model's *output* short too — output tokens are the priciest. Measure cost in words written, not files touched.

**Caveat (mandatory).** The brief is a **map, not the territory**, and it's a lossy compression chosen by the *least* capable model in the chain, *before* the expensive one has formed its question — so the loss is silent. Two cheap guards close it: **(1)** every claim in the digest carries a **source pointer** (path / id / line), so drilling down is one read, not a re-derivation; **(2)** the expensive model has **explicit license to pull raw sources** on any anomaly, thin spot, or load-bearing claim. A brief it can't drill through is a *filter*, not a lens.

**Source · date · confidence.** Bottega dogfooding — the strategist layer (BRIEF built by a cheap fan-out) · 2026-07 · high (with caveat)

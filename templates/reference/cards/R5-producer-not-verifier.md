# R5 — The producer never passes its own work

**Claim.** A session that produced an artifact reliably misses its own blind spots when reviewing it — models judge their own output more favorably and can't self-correct reasoning on demand. Verification must be a *different* actor: a fresh session/agent with an adversarial charter ("find why this fails"), not the author asked to be careful.

**When to pull.** You are designing the QA/gate flow, or tempted to save a session by letting the authoring session self-review.

**How to apply.** Route every gate to a session with no authorship of the artifact. Frame the verifier as a refuter ("try to break/refute this"), never a confirmer ("check this is fine"). If budget forces one actor, the minimum is a fresh context reading from disk — never the same context that wrote it.

**Source · date · confidence.** Agent practice + LLM self-correction/self-preference literature (citations to pin via research) · 2026-07 · high

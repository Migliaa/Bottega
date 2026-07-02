# Research brief — pin sources for the reference-shelf cards

> **Type:** one cheap research fan-out (ultracode: ~4 readers + 1 verifier). **Model:** cheap (Sonnet). **Output:** `research/reference-shelf-sources.md` (findings), then the maintainer updates the affected cards' `Verified:`/confidence lines. Specified by the Fable round-2 review (`strategy/BOARD_REVIEW.md → #M4`).

## Goal
Pin 2–3 authoritative sources per topic and confirm (or correct) each card's claim wording. This upgrades cards currently marked "citations to pin" and makes two not-yet-written cards card-ready.

## Per-topic queries (one reader each)
1. **Gate design / killer items → card [R4]** (`templates/reference/cards/R4-gate-killer-items.md`)
   - Gawande, *The Checklist Manifesto* — killer-item philosophy, DO-CONFIRM vs READ-DO.
   - Aviation checklist human-factors design.
   - SmartBear / Cisco code-review study — defect detection collapses beyond ~200–400 LOC per review (same fatigue mechanism).
   - "Checklist fatigue" / compliance-drift literature.
   - Confirm: the evidence supports *bounded scope + bounded length* (not Miller's 7±2 as a checklist cap).

2. **Restart-over-stretch → NEW card (not yet written)**
   - Liu et al. 2023, *Lost in the Middle*.
   - Long-context degradation / "context rot" benchmarks.
   - Anthropic engineering guidance on context management / compaction for agents.
   - Claim to verify: "a fresh session reading a good digest beats a long session pushing degraded context."

3. **Producer ≠ verifier → card [R5]** (`templates/reference/cards/R5-producer-not-verifier.md`) — citation hardening only
   - Huang et al. 2023, *LLMs Cannot Self-Correct Reasoning Yet*.
   - Self-preference bias in LLM evaluators (evaluators favor their own generations).

4. **Fan-out hygiene (dedup + adversarial verify) → NEW card (not yet written)**
   - Anthropic multi-agent research-system engineering write-up.
   - Self-consistency literature (Wang et al.).
   - LLM-judge verification patterns.
   - Claim to verify: "between parallel finders and action there is always dedup + adversarial verify — budget both from the start."

## Verifier (5th agent)
Check that each pinned source actually says what the card claims (no citation inflation). Flag any claim the sources *don't* support → the maintainer softens or drops it.

## Deliverable shape
Per topic: 2–3 pinned sources (title + author/org + year + one-line "supports: …") + any correction to the claim wording. Then the maintainer:
- updates `Verified:` + confidence on **R4** and **R5**;
- writes the two NEW cards (restart-over-stretch, fan-out-hygiene) card-ready and adds INDEX rows;
- logs the admission in `DECISIONS.md`.

## Guardrail
This is public-framework method-knowledge — general sources only, **no project-private detail**. Web research session → **isolate in its own worktree**, write only under `research/`.

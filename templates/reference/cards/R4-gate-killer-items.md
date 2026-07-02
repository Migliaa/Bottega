# R4 — A gate checks only what blocks the hand-off: one screen of killer items

**Claim.** Checklists fail *open*, by bloat: past one screen (~5–9 killer items) checkers rubber-stamp and the one check that matters drowns. A gate lists only what would **block** the merge/hand-off; everything else becomes automation (linters, CI) or moves into the work-order's definition-of-done. (The old "cap at 7" is folklore from working-memory research — the real evidence supports *bounded scope* + *bounded length*, not a magic number.)

**When to pull.** You are designing or revising a gate/checklist, or a gate has started passing everything.

**How to apply.** Write every check as a killer item: "if this fails, the hand-off stops." One screen, no scrolling. Each check must be answerable by looking at an artifact, not by re-doing the work. A new check past the cap evicts or automates an existing one.

**Source · date · confidence.** Checklist practice (aviation killer-items; Gawande) + code-review fatigue studies — citations to pin via research; Bottega gate incidents pending · 2026-07 · medium

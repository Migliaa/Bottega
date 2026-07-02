# R1 — Split agent lanes by write-territory, not by skill

**Claim.** When you divide work across separate agent sessions, the durable seam is **who writes which files**, not who is "good at" what. One owner per folder; a work-order never spans two territories. Skill-based splits ("the QA agent", "the frontend agent") drift into overlapping writes and merge conflicts; territory-based splits collide far less, because two owners rarely touch the same file.

**When to pull.** You are deciding how to divide a task into sessions/agents, or writing a work-order, or chartering a new department.

**How to apply.** Give each work-order exactly one owner and one folder. If a task needs two territories, split it into two work-orders. Declare test/build deps inside the owner's own package, never into a neighbour's. Non-org sessions isolate in their own worktree; the org owns `main` and merges.

**Watch the seams.** Shared/root files that belong to *no* lane's folder — lockfiles, root CI config, generated files, `INDEX`/shelf files — are a territory too: assign each one owner (default the manager). This is where conflicts migrate *after* the lanes are otherwise clean. If two lanes must touch one, that's two work-orders serialized through the owner, not a shared write.

**Source · date · confidence.** Bottega dogfooding — the territory-scoped-work-orders and session-isolation incidents · 2026-06 · high

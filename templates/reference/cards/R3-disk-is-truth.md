# R3 — Disk is truth: make every channel recoverable, so a skipped update never blocks

**Claim.** In a file-coordinated multi-agent system, state must live in files, not in any session's context — and the *facts* must live somewhere separate from the *messages*. Then a forgotten hand-off is recoverable: whoever needs it reconciles from disk. A channel that is a single point of failure will fail.

**When to pull.** You are designing any coordination mechanism, hand-off, or mailbox between sessions, or worrying "what if a session forgets to update X?"

**How to apply.** Keep a *register* of facts (`STATUS.md`, artifacts) distinct from the *channel* of messages (`INTERFACE.md`). Let the reader rebuild the channel's digest from the register at its next start. Never let one file's staleness block another actor. The only thing that must be written by hand is what only that actor knows (its own questions).

**Source · date · confidence.** Bottega dogfooding — the INTERFACE protocol (channel-vs-register, self-healing) · 2026-06 · high

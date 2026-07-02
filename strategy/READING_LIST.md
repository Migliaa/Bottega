# READING_LIST — depth for the Strategist (optional)

> The `BRIEF.md` is self-contained. These are the source-of-truth files if you (or Opus, when detailing) want to go deeper. **Don't read them all** — pull only what a specific question needs.

## Architecture (for #M1)
- `docs/01-overview.md` — the model: three actors, git-as-bus, the file table, the project tree.
- `docs/04-interface-protocol.md` — the `INTERFACE.md` mailbox in full (why per-department, the 3 sections, channel-vs-register, self-healing, session-boundary discipline, worked example).
- `docs/03-roles.md` — roles + "your folder, your code" + escalation via channel.
- `docs/05-constraints.md` — the guardrails (rooting + isolation compose).
- `templates/CLAUDE.md.template` — §2.8 has every lightweight convention in force.

## Reference shelf (for #M2)
- `docs/01-overview.md` (project tree) — `reference/` exists in the taxonomy today as "durable reference docs", but is barely used; #M2 designs it properly.
- `research/README.md.template` — how the *research* folder indexes itself; a possible shape to borrow (or deliberately differ from) for `reference/`.

## Constraints envelope (applies to every answer)
- `BRIEF.md §3` — the hard limits any proposal must live inside (no runtime · token economy · human-in-the-loop · merge-safety · graceful degradation · portability). This is Bottega's equivalent of a compliance envelope: a design that breaks these is out of bounds even if otherwise good.

# 07 · Glossary

The shared vocabulary of a Bottega. Using these terms consistently is part of what makes the model legible — to your teammates and to the agents.

| Term | Meaning |
|---|---|
| **Bottega** | This operating model. Italian for the Renaissance workshop where a master directed apprentices on commission. |
| **Committente** | The human patron who commissions and steers the work. In Bottega: **you** — you decide strategy and **trigger** the sessions. You don't carry content between them; the git files do. |
| **Manager** (capobottega) | The single Opus session that plans, decides architecture/product, triages, writes sprints, and is the only actor that commits to `main`. |
| **Department** | A specialized session with a narrow mandate and its own folder. Persistent (validation, publishing, compliance…) or disposable (execution). |
| **Execution session** (operative) | A disposable session that runs one self-contained work-order, then is discarded. No `INTERFACE.md`. |
| **Persistent department** | A long-lived concern that accumulates state across sessions; keeps a `STATUS.md` and an `INTERFACE.md`. |
| **Macro-sprint** | A large, coherent objective (`S1`, `S2`, …). |
| **Sub-sprint** | A substantial, independently testable block inside a macro (`S1.A`); the unit the committente validates. |
| **Micro** | The largest operation a single model pass can do safely; a clear, verifiable deliverable (`S1.A.1`). |
| **Work-order** | A self-contained block in `Sprint.md` an execution session can pick up cold. |
| **Gate** | A checkpoint that must be green before the project advances past it. The manager enforces the chain. |
| **INTERFACE.md** | The async mailbox between the manager and one persistent department (Digest / From Manager / From Dept). |
| **STATUS.md** | A department's domain register — *facts*, not messages. |
| **ActualStatus.md** | The whole project's live state, in one page, auto-loaded each session. |
| **CLAUDE.md** | The process constitution: the rules every session in the project obeys. |
| **COMMITTENTE.md** | The human cockpit: how to work, where files are, what to do now. |
| **IP-to-protect** | The project's moat — the one proprietary thing that never leaves the private repo. "Sell the shovels, not the gold." |
| **Rooting** | Opening a session in the correct project root folder so git, slash commands, and disk reads resolve. |
| **Bus** | The channel that carries content between sessions: the **git files** (`INTERFACE.md`, `Sprint.md`, `ActualStatus.md`). The committente doesn't carry content — they *trigger* the sessions that read/write it. |

> Two signature terms are intentionally Italian — **Bottega** and **committente** — because they name the model precisely (a workshop on commission) and give it an identity. Everything else is plain.

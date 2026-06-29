---
description: Stand up Bottega for THIS project — interview the committente, then generate the instance files.
argument-hint: "[optional: path to a vendored copy of the Bottega framework]"
---

You are bootstrapping **Bottega** for this project. Goal: by the end, this repo has a working manager + the chosen departments, generated from the framework templates — so the committente never starts from a blank prompt again.

## Step 1 — locate the framework
Find the Bottega templates: either vendored in this repo, at the path in `$ARGUMENTS`, or ask the committente where their local Bottega clone is. You need `templates/` and `departments/`.

## Step 2 — interview the committente (one question at a time, conversational)
Collect every value in `templates/bottega.config.md`:
- **Project:** name, one-liner, repo URL, **public or private**, absolute root path, stack.
- **Guardrails:** which files are **secrets** (never commit)? Is there an **IP-to-protect** (the moat that never leaves a private repo)? — if unsure, ask "is there anything here you'd never want published?"
- **Departments:** which do you need? Offer the archetypes from `departments/` (execution, validation, research, publishing, compliance) and explain each in one line. Recommend the **minimum**: a manager + execution is enough to start; add others only when a lane is genuinely overloaded.
- **Manager:** model, the language to speak, the static-check commands for the stack.
- **You (committente):** role, technical level, preferences — to seed memory.

Be honest and recommend; don't dump options. Pre-vet odd choices (e.g. "public repo + an IP-to-protect" → flag the tension).

## Step 3 — generate the instance files
From the answers, write into the project root:
- `bottega.config.md` (filled), `CLAUDE.md`, `COMMITTENTE.md`, `ActualStatus.md`, `Sprint.md`, `Sprint_Done.md`, `DECISIONS.md` — substituting every `{{placeholder}}`.
- `.claude/commands/`: `lancia-org.md`, `lancia-operativa.md`, `lancia-ricerca.md`, `handover.md`, `bootstrap.md`, and one `lancia-<dept>.md` per chosen department.
- For each chosen persistent department: `<dept>/CHARTER.md` (from its `departments/` recipe), `<dept>/STATUS.md`, `<dept>/INTERFACE.md`.
- Seed the manager's memory: `MEMORY.md`, `user-profile.md`, `working-style.md` (in the project's memory dir).

## Step 4 — verify & hand off
- Confirm no placeholder remains: search for `{{` across the generated files.
- Confirm `{{SECRETS_GLOB}}` is in `.gitignore`.
- Summarize what you created + the very next action: "open a session rooted here and run `/lancia-org`."
- **Do not commit** unless the committente says so — show them the diff first.

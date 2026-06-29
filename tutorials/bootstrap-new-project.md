# Bootstrap a new project with Bottega

Two ways: **guided** (let the manager interview you and generate everything) or **manual** (copy and fill). Guided is recommended.

## Prerequisites
- [Claude Code](https://claude.com/claude-code) installed.
- A git repo for **your** project, with a remote. Decide now: **public or private?**
- A local clone of the Bottega framework (this repo) to copy templates from.

---

## Option A — Guided (recommended)

1. Copy `templates/commands/bootstrap.md` into your project's `.claude/commands/bootstrap.md`.
2. Open a Claude Code session **rooted in your project folder**, manager model.
3. Type `/bootstrap` (optionally pass the path to your Bottega clone).
4. **Answer the interview.** The manager asks, one question at a time: project name & one-liner, repo URL, public/private, root path, stack; which files are secrets; whether there's an IP-to-protect; which departments you need; your profile and language.
5. The manager **generates the instance files** (filled `CLAUDE.md`, `COMMITTENTE.md`, state files, commands, chosen departments, memory seeds) and shows you the diff.
6. Review, then let it commit. Done.
7. Open a fresh session rooted in the project and run `/lancia-org`.

> The interview will recommend the **minimum** setup (manager + execution). Add departments later when a lane is genuinely overloaded — you don't have to decide everything now.

---

## Option B — Manual

1. **Copy** `templates/*` into your project root. Rename each `*.template` → drop the suffix (`CLAUDE.md.template` → `CLAUDE.md`).
2. **Fill** `bottega.config.md` with your values, then substitute every `{{placeholder}}` across the copied files. Search for `{{` to confirm none remain.
3. **Commands:** put the launchers in `.claude/commands/` (rename `lancia-dept.md.template` once per chosen department, e.g. `lancia-validation.md`).
4. **Departments:** for each one you enabled, create `<dept>/CHARTER.md` (from the matching `departments/*.md` recipe), `<dept>/STATUS.md`, and `<dept>/INTERFACE.md` (from the template).
5. **Memory:** seed the manager's memory dir with `MEMORY.md`, `user-profile.md`, `working-style.md`.
6. **Guardrails:** make sure `{{SECRETS_GLOB}}` is in `.gitignore`.
7. Open a session rooted in the project and run `/lancia-org`.

---

## Verify it took

- `/lancia-org` is recognized (if not → you're in the wrong folder, or the command file isn't in `.claude/commands/`).
- The manager, on start, summarizes the (empty) state and confirms it read the department interfaces.
- No `{{placeholder}}` remains anywhere.
- A test commit does **not** include any secret file.

## A note on agent memory across projects
Claude Code keys memory to the **main repo path**, and each project gets its own memory. If you're *migrating* an existing project into Bottega and want to carry over a manager's memory, copy the relevant memory files into the new project's memory dir — don't assume they follow automatically.

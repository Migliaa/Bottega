# 05 · Constraints — the guardrail pattern

Guardrails in Bottega are **explicit, boring, and written down**. They are not assumed, and they are not left to the model's judgment in the moment. These five are the core set; your `CLAUDE.md` instantiates them with your project's specifics.

## 1. Secrets are never committed

Declare your **secret files** as a glob (`{{SECRETS_GLOB}}`, e.g. `**/.env.local`, `secrets/**`). The rule every session obeys:

> Before **every** commit, verify the staged set contains no secret file. Concretely: `git status --porcelain | grep -i <secret-pattern>` must be empty before `git commit`.

Secrets stay git-ignored. There is no "just this once."

## 2. Only the manager commits

Execution and department sessions produce changes; the **manager reviews, tests, and commits** them to `main`. Departments may commit **scoped to their own folder** for record-keeping. This gives you one accountable committer, exhaustive commit messages, and a clean rollback story.

> If your tool auto-creates a per-session worktree/branch, the manager merges that branch to `main` — it doesn't let work pile up on an orphan branch.

## 3. The IP-to-protect never leaves the private repo

Most projects have a **moat** — the one proprietary thing that must never be published. Name it explicitly (`{{IP_TO_PROTECT}}`): calibration numbers, weights, prompts, a dataset, a pricing model, whatever it is.

> Metaphor worth keeping: **"sell the shovels, not the gold."** You can open-source the tools, the docs, the scaffolding — the *gold* (the protected IP) stays in the private repo, never in a public surface, never in a published artifact.

Internal-only departments (e.g. `compliance/`, which records admissions and prepares defenses) are **never** published or exposed — they live only in the private repo.

## 4. Root every session in the project folder

A session is anchored to a directory. If you open it in the wrong place, three things silently break:

- git points at the wrong repo;
- slash commands aren't found ("Unknown command");
- "read `ActualStatus.md` from disk" resolves to nothing.

> **Rule:** open **every** session for a project from that project's root folder (`{{PROJECT_ROOT}}`) — never from another repo or an unrelated worktree.

This is the single most common setup mistake; making it a written rule prevents an afternoon of confusion. (Agent memory is keyed to the **main repo path**, so it survives across that repo's worktrees — another reason to keep rooting consistent.)

## 5. Confirm before irreversible or outward-facing actions

Destructive or public actions (`git push --force`, dropping a table, deleting files you didn't create, sending anything to an external service, charging money) require **explicit confirmation first**, unless durably authorized. Approval in one context does not extend to the next. Paid dependencies (API tokens, cloud credits) are confirmed before spending.

---

## Putting it in CLAUDE.md

These become a short, literal block in the project's `CLAUDE.md` so every session inherits them. Example (filled from `bottega.config.md`):

```markdown
## Guardrails (non-negotiable)
- NEVER commit secret files: **/.env.local, secrets/**. Verify before each commit.
- Repo {{REPO_URL}} is {{REPO_VISIBILITY}}. compliance/ is internal — never published.
- IP-to-protect (never leaves the private repo): {{IP_TO_PROTECT}}.
- Only the manager commits/pushes to main. Departments commit scoped to their folder.
- Open every session rooted in {{PROJECT_ROOT}}.
- Commits end with: {{COMMIT_COAUTHOR}}
```

Next: **[06 · Agentic governance](06-agentic-governance.md)**.

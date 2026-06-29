# Troubleshooting

The handful of things that actually go wrong, and the fix.

## "Unknown command: /lancia-…"
The session isn't rooted in the project folder, so it can't see `.claude/commands/`. **Fix:** open the session from the project root (`{{PROJECT_ROOT}}`). If you can't re-root immediately, paste the body of the command file as your message — it's identical to what the command expands to.

## A department committed to a branch, not main
Some setups auto-create a per-session worktree/branch (e.g. `claude/<name>`). That's fine — the **manager** merges it to `main` (`git checkout main && git merge --ff-only <branch>`), reviews, and pushes. Execution sessions aren't supposed to commit; if one did, the manager folds the work in.

## A session "died" mid-task
Nothing is lost — Bottega state lives in files on disk (and on that session's branch/worktree if it had one). Tell the manager what was in flight; it recovers from `ActualStatus.md` / `Sprint.md` / the branch.

## The manager points at the wrong project / wrong files
It's rooted in the wrong repo. Two symptoms travel together: wrong git remote and slash commands missing. Re-open it rooted in the right folder. (You can verify with `git remote -v` inside the session.)

## The new manager session "lost" the vision / context
A fresh session starts cold and reads the files. If it seems to have lost the *working style* (honesty, initiative), its **memory** probably didn't carry over — memory is per-project. Copy the memory seeds (`working-style.md`, `user-profile.md`, project context) into the new project's memory dir. The *project* state always lives in the repo files, so that part is never lost.

## A secret almost got committed
Stop the commit. Confirm `{{SECRETS_GLOB}}` is in `.gitignore`. The guardrail in `CLAUDE.md` says to verify the staged set before every commit — remind the agent of it. If a secret was already pushed, rotate it; git history is hard to scrub after the fact.

## Two sessions editing the same working tree
Don't run an execution session and the manager on the **same** working tree at once if both write — they'll collide. Let one finish (the manager commits its work) before launching the other, or isolate one on its own branch/worktree.

## Sessions are slow / expensive
The context is too long. Between heavy sprints, `/compact` (you type it) or open a fresh session — the files reload automatically.

---
name: origin-sync
description: >
  Rebase the current branch onto the latest origin/main, then force-push it back to remote.
  Trigger when the user mentions "fetch main", "sync main", "rebase upstream",
  "pull in main's updates", or "origin has new commits".
---

# Origin Sync Skill

Rebase the current feature branch onto the latest `origin/main`, then force-push it back to remote.

## Process

### 1. Fetch and confirm the starting point

```bash
git fetch origin main
git merge-base HEAD origin/main
git log --oneline HEAD..origin/main | head -10   # what main has that this branch doesn't
```

Report: which commit the current branch branched from, and how many commits `origin/main` is ahead by.

### 2. Stash uncommitted changes

If `git status --short` shows uncommitted changes, stash them — a rebase refuses to start with a dirty tree:

```bash
git stash push -m "origin-sync: <short description>"
```

### 3. Rebase

```bash
git rebase origin/main
```

#### Resolving conflicts

On conflict, **analyze and resolve file by file** — never blindly pick ours/theirs:

1. Read both sides of the conflict and understand each side's intent.
2. Merge by hand (usually "keep both", not "pick one").
3. Common conflict shapes:
   - **Import merges** — each side added different imports → keep both lines, ordered per isort.
   - **Same function edited on both sides** — read the semantics carefully and preserve both changes.
4. Once resolved, stage with an **absolute path** — if cwd isn't the project root (e.g. it's in `.claude/skills/`), a relative path won't find the file:
   ```bash
   git add /path/to/repo/<conflicted-file>
   git rebase --continue
   ```
5. If multiple commits conflict, repeat until the rebase completes.

### 4. Restore the stash

If step 2 stashed anything, restore it once the rebase finishes. If `stash pop` fails to auto-merge, resolve it the same way as step 3.

```bash
git stash pop
```

### 5. Push

```bash
git push origin <branch-name> --force-with-lease
```

Local and remote have diverged after the rebase, so don't pull first. Use `--force-with-lease`, not `--force`: if remote has moved since you last fetched it, the push is rejected instead of overwriting someone else's commits.

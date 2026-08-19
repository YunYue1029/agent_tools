---
name: origin-sync
description: >
  Rebase the current branch onto the latest origin/main, then force-push it back to remote.
  Trigger when the user mentions "fetch main", "sync main", "rebase upstream",
  "pull in main's updates", or "origin has new commits".
---

# Origin Sync Skill

Rebase the current feature branch onto the latest `origin/main`, then force-push it back to remote.

## Usage

```
/origin-sync          # sync the current branch
```

---

## Process

### 1. Confirm the branch's starting point

```bash
git log --oneline -5
git merge-base HEAD main
git log --oneline HEAD..origin/main | head -10   # how many commits main is ahead by
```

Report: which commit the current branch branched from, and how many commits `main` is ahead by.

### 2. Fetch origin/main

```bash
git fetch origin main
```

### 3. Handle unstaged changes

If `git status --short` shows uncommitted changes, stash them first:

```bash
git stash push -m "origin-sync: <short description>"
```

> Skip this step if there are no unstaged changes.

### 4. Rebase

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
4. Once resolved:
   ```bash
   # Note: if cwd isn't the project root, use an absolute path
   git add <absolute-path/conflicted-file>
   git rebase --continue
   ```
5. If multiple commits conflict, repeat the steps above until the rebase completes.

### 5. Restore the stash

If step 3 stashed anything, restore it once the rebase finishes:

```bash
git stash pop
```

> If `stash pop` fails to auto-merge, resolve it the same way as step 4.

### 6. Push

```bash
git push origin <branch-name> --force-with-lease
```

> Use `--force-with-lease`, not `--force`: if remote has moved since you last fetched it, this rejects automatically instead of overwriting someone else's commits.

---

## Notes

- **`git add` path pitfall** — if Claude Code's cwd isn't the project root (e.g. it's in `.claude/skills/`), `git add apps/foo.py` won't find the file. Always use an absolute path: `git add /path/to/repo/apps/foo.py`.
- **Stash-then-rebase order** — stash first, then rebase, then stash pop. Rebasing first fails outright if there are unstaged changes.
- **Branch state after push** — after a rebase, local and remote have diverged; no need to pull before pushing, `--force-with-lease` handles it directly.

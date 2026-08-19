---
name: pr-description
description: Write a reviewer-friendly PR description in Traditional Chinese from the current branch's changes, saved to z/. Use when the user is opening a PR, asks for a PR description, or wants their branch summarised for review.
---

# PR Description

Say what the branch does and what it costs a reviewer to accept it. **Describing only** — don't modify code, don't fix bugs you notice on the way through, don't run anything destructive.

## Gather

The diff is the source of truth:

```bash
git diff <target>...HEAD --stat     # which files moved
git diff <target>...HEAD            # what actually changed
git log <target>..HEAD --oneline    # the commits, and any issue refs in them
```

If the target branch is ambiguous, ask which one.

Read the changed files where the diff alone doesn't reveal intent, plus any design doc, spec, or `CHANGELOG` the branch touched. If this branch continues earlier work — a follow-up fix, a second pass at an algorithm — find that PR or commit so the summary can say so in a clause.

## Output

The diff already says what changed, file by file. This description exists for what the diff can't say: what it does, and why. **Never enumerate** — no "added `X` in `Y`", no file-by-file walkthrough. If a sentence could be replaced by looking at the file tree, delete it.

**Write the prose in 繁體中文 (Traditional Chinese).** Everything else stays as-is: the `feat:`/`fix:` prefix, the section headings, and any identifier, path, branch, or command you have to name. Don't translate technical terms that the team writes in English anyway.

Title line prefixed by the nature of the change: `feat:`, `fix:`, `refactor:`, `docs:` — then one clause naming the effect.

- `## Summary` — why this exists and what it does, as 1–3 bullets. State the effect (「v2 現在能比對到 v1 直接丟掉的 URI-encoded CPE 字串」), not the mechanism (「新增 `CPEQuery` class」).
- `## Behavior / Compatibility` — **only when there's something to say.** Breaking changes, migrations, new env vars or flags, old path still working or not. If nothing changes for a caller, drop the heading — don't write "N/A".

### Where it goes

Write the description to `z/pr-<branch>.md` at the repository root — `git rev-parse --show-toplevel` for the root, `git rev-parse --abbrev-ref HEAD` for the branch, with `/` in the branch name replaced by `-`. Create `z/` if it doesn't exist. If that file already exists, overwrite it only after reading it; if it holds something unrelated, pick the next free `z/pr-<branch>-2.md`.

The file holds the PR description and nothing else — no preamble, no explanation of how you produced it. In chat, reply with just the path you wrote.

## Rules

- **Never describe a change that isn't in the diff.** No invented rationale, no compatibility claim you didn't check. If you can't tell why something changed, say so or leave it out.
- **State effect, not mechanism.** If you're naming a function, file, or class, ask whether the sentence survives without it. Usually it does — cut the name.
- Bold what a reviewer scans for: **breaking**, **compatibility**.
- Write like an engineer filing a PR. Not chatty, not a paper.

---
name: correct
description: Find the mistakes agents keep repeating in this repo and make each one impossible — architecture first, then types, then a lint whose error names the fix, then a test, docs last — proving each new check fails on a real past mistake. Use for /correct, "stop making this mistake", "why do agents keep doing X here", or after the user corrects the same thing twice.
---

# Correct

The user keeps correcting agents in this repo for the same mistakes. Change the repo so the next agent can't make them.

Assume every contributor is an agent that sees only the files it opened, copies the nearest example, and takes the shortest path that compiles. Design the repo so a change that looks right from one file is right for the whole repo.

## 1. Find the mistake classes

Read recent commits, reverts, review comments, agent instruction files (`CLAUDE.md`, `AGENTS.md`, `.claude/rules/`), and comments that explain workarounds. Include the correction that triggered this run.

Group the mistakes into **classes**. A class counts once it has happened twice — a one-off is not a class.

## 2. Pick the highest level that works

For each class, take the first level that makes the mistake impossible or loud:

1. **Eliminate it with architecture.** Give each piece of state one owner and each task one supported way. Hide internals so the wrong import fails. Replace hand-synced lists with one source of truth. Delete the old way and the dead code an agent would copy.
2. **Enforce it with types,** so the bad state can't be written. If bad code still compiles, add a **lint or CI check whose error names the file, type, or function to use instead.** If the pattern is already common, fail only when a change adds more.
3. **Test the behavior.** Fix or delete any test that still passes when the behavior it checks breaks — see the two-edit check in the [tdd](../tdd/SKILL.md) skill.
4. **Write docs or agent rules last, and only for judgment calls.** Nothing fails when an agent skips them.

## 3. Propose before changing

Present every class before touching anything:

- the class, with its evidence — the commits, reviews, or corrections where it happened
- the level you picked, and **why each higher level doesn't work**
- the concrete change: the file, the check, the error message an agent will see

Wait for the user to pick which classes to fix.

## 4. Fix and prove

Fix the approved classes, most frequent first, **one class per change** so each can be reviewed and reverted alone.

**Prove each new check fails on a real past mistake** — reapply the old bad code, or check out the commit that had it, and show the check going red with an error that names the fix. A check that has never failed proves nothing. Make the check runnable by the same command locally and in CI.

An exception goes on the offending line with a reason, an expiry date, and a human's approval.

## 5. Keep the rule table

Keep a table in the agent instruction file that pairs each rule with what enforces it:

| Rule | Enforced by |
| --- | --- |
| Dates go through `parseDate`, never `new Date(string)` | lint `no-raw-date` |
| Every handler returns through `respond()` | type: handlers return `Response`, only `respond()` builds one |
| Prefer composition over inheritance in `ui/` | judgment — docs only |

When the user corrects you, fix the mistake and add the rule. If the rule was already in the table and nothing enforces it, that's a repeat — fix it at the highest level in the same change. Drop a rule once its mistake can't happen.

## Reply

Each class with its evidence, the level picked, why a higher level didn't work, and — for each fix applied — the proof that its check fails on the past mistake.

---

Adapted from [pstack](https://github.com/cursor/plugins/tree/main/pstack) (`correct`), MIT © Lauren Tan. Changed to propose every class and wait for approval before fixing.

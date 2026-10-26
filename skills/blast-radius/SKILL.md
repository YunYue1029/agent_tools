---
name: blast-radius
description: Find what a change could break somewhere else — beyond the diff — and prove the one fact it is safe because of by running real code instead of writing it up. Use for "blast radius of X", "what could this break", reviewing a small diff you don't trust, or before designing around a claim about existing code ("make endpoint X public", "nothing else reads this field").
---

# Blast Radius

Find what a change breaks somewhere else, before it ships.

[explain](../explain/SKILL.md) says what the code does. [code-review](../code-review/SKILL.md) checks the diff against standards and spec. Blast radius asks what the change breaks **outside** the diff.

Listing the callers is not the job — grep does that in a second. The job is the breakage grep won't show you.

Run it before design too, not only before shipping, whenever a brief asserts something about existing code. The assertion is a hypothesis; who actually consumes the thing decides the design.

## Don't trust your own writeup

A blast-radius writeup that sounds right is worthless — it reads as convincing whether or not it's true. Don't hand back the writeup. Find the one or two facts the whole thing depends on and prove them by running code.

### How sure are you

For each fact the change's safety depends on, get it as far down this list as is cheap, and say where it stopped:

1. **You said so.** Worthless on its own.
2. **You pointed at the line.** A real `file:line`, or the library's own source.
3. **You showed the bad case can't happen.** You walked the failure step by step and it doesn't reach.
4. **You ran it.** A script or test that calls the real code and fails loud if you're wrong.
5. **You reproduced it in the running app.**

Level 4 is usually one small script that imports the same library the app ships and calls the exact function you're worried about.

## Steps

1. **Read the change.** The diff, the symbols it adds, changes, and deletes, and what it now does differently — including the part the diff doesn't spell out. Pull the history behind it with `git log`, `git blame`, and `gh pr view` when it continues earlier work.
2. **Find the one fact it's safe because of.** Most changes that look risky are safe because of a single fact — "this call only drops cache entries that are already dead". Find that fact. If it holds, most of the risky cases clear at once. Spend your time here, not on a long list of maybes.
3. **Look where grep stops.** Read the source of the library you call, at its pinned version, with any local patch. Work out _when_ things run — event loop turns, teardown, lifecycle hooks, retries. Follow what a symbol search misses: the JSON an API returns, a DB column, a wire format, another language reading the same bytes, a feature flag, code three hops downstream.
4. **Be honest about each risk.** Give it a real chance of happening and a real cost if it does. Keep the risks you confirmed; list the ones you checked and cleared separately. Cite a real `file:line`. A search that finds nothing is still an answer — say what you searched. Never invent a caller or an API.
5. **Prove the one fact.** Write a script or test that runs the real code, run it, and paste what happened.
6. **For a big or wide change,** split step 3 across parallel sub-agents — one per consumer area — and merge their findings yourself.

## What to hand back

- **What it does.** What changed, including the part that isn't obvious.
- **The one fact it's safe because of.** State it, the level it reached, and the proof. If you couldn't prove it, write **unproven**.
- **Risks.** Each names how it breaks, the `file:line`, how likely and how bad, and how to check. Paste the proof for the ones that matter.
- **Cleared.** What you checked and why it's fine.
- **Before you merge.** The cheapest test or repro that would catch the real bug, including the script you wrote.

---

Adapted from [pstack](https://github.com/cursor/plugins/tree/main/pstack) (`blast-radius`), MIT © Lauren Tan. References to pstack-only skills were replaced with plain steps.

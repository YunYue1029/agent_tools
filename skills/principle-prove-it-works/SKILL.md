---
name: principle-prove-it-works
description: Apply after completing a task, before declaring it done or reporting success. Verify against the real artifact — run the feature, read the actual value, inspect the real state — not a proxy, a self-report, or "it compiles".
user-invocable: false
---

# Prove It Works

Verify every task output by checking the real thing directly. Don't infer from proxies, self-reports, or "it compiles".

**Why:** unverified work has unknown correctness. Indirect checks — file timestamps, cached output, a sub-agent's summary, a screenshot from earlier — feel cheaper than looking. Acting on a wrong inference costs far more than checking the source.

## Check the real thing, not a proxy

- Read the actual value, not a cached or derived representation of it.
- Check a process is alive directly, not through state it once wrote.
- Before reporting success — merged, sent, started, fixed — read the real state and the exit code. Truncated output (`| tail -1`) and your own `echo` prove nothing.
- When a check fails, suspect the observation method before suspecting the system.
- A sub-agent's "done" is a claim. Read its diff.

## Red is a colour, not a measurement

A failing check proves something only when the failure is the disagreement you predicted. An exception, an empty collection, and a real mismatch all print red. Quote the assertion's _diff_ (`expected 3, got 0`), not the assertion line.

## Script the check when you can

The strongest proof is a deterministic script that reruns the same comparison, not a one-time eyeball. Write it, run it, and keep its output where a reviewer can rerun it instead of trusting your word.

## Respect who runs what

If the user's instructions reserve running tests, linters, or formatters for themselves, don't run them. Prove the work by other direct means — read the changed values, exercise the feature, inspect the diff — and name the exact command the user should run. Never report a check as passing that you didn't run.

---

Adapted from [pstack](https://github.com/cursor/plugins/tree/main/pstack) (`principle-prove-it-works`), MIT © Lauren Tan.

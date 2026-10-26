---
name: planning
description: Decompose a high-level or vague goal into ordered, verifiable steps — grill the user first, write the agreed plan to Markdown in z/ only after it converges. Use when the user asks to "plan this", "discuss how to do this first", or hands over a goal that needs breaking down before anyone builds anything.
---

# Planning

Turn a goal into steps someone can execute and verify. **Discuss first, document later** — the plan file is the last thing you produce, not the first.

Skip this skill when the goal is already one clear step, when the user has handed you a step-by-step plan, or when they say just do it.

## Phase 1 — Grill

Run the discussion as a [grilling](../grilling/SKILL.md) — one question at a time, each with your recommended answer. Settle the goal before debating step order. No implementation and no file writes, not even the plan document, until Phase 2 passes.

What to cover, roughly in this order. Not every goal needs every item:

- **Goal** — the goal in one sentence, and whose goal it is.
- **Success** — what "done" looks like, concrete enough to check against.
- **Boundary** — what's explicitly out of scope, and whether a "good enough" version exists separate from the full one.
- **Context** — what triggered this work, and the constraints binding it (time, stack, existing code).
- **Decomposition** — propose a breakdown and say why that order or grouping. Then refine: what merges, splits, or reorders.
- **Chain** — for each feature in the breakdown, its flow as an ordered list of named steps, and what each step produces for the ones after it. See [coding-style](../../rules/coding-style.md); the chain agreed here is the one the entry function's body will read as. Settle it in discussion — a missing step is cheapest to find now, and a chain nobody can name in steps is a feature nobody understands yet.
- **Granularity** — each step one actionable or verifiable unit. A step that's still vague is a step that needs splitting.
- **Dependencies** — what blocks what, and how to break any cycle.
- **Assumptions** — what you're both taking for granted, and which of them need validating before work starts.
- **Unknowns** — what's still undecided, and when it has to be decided.
- **Trade-offs** — what got chosen over what, and why.
- **Risks** — what could block this, and any mitigation.

## Phase 2 — Convergence check

Write nothing until every applicable box is ticked. If one isn't, go back to Phase 1.

- [ ] Goal and success criteria agreed by both sides
- [ ] Every step is a single actionable or verifiable unit
- [ ] Each feature's chain named as an ordered list of steps, with what each produces
- [ ] Order defined, no unresolved dependency conflicts
- [ ] Important assumptions named out loud
- [ ] Key trade-offs and decisions clear enough to write down
- [ ] In-scope and out-of-scope agreed

## Phase 3 — Write the plan

Write the plan to `z/plan-<goal-slug>.md` at the repository root — `git rev-parse --show-toplevel` for the root, and a few kebab-case words from the goal for the slug. Create `z/` if it doesn't exist. If that file already exists, overwrite it only after reading it; if it holds an unrelated plan, pick the next free `z/plan-<goal-slug>-2.md`.

Use this structure:

```markdown
# Plan: [Goal title]

**Goal:** [One sentence]
**Success:** [How we know we're done]

## Steps

1. [Step 1]. (Depends: none.)
2. [Step 2]. (Depends: 1.)

## Chain: [Feature name]

1. [Step name] — [what it accomplishes, and what it produces]
2. [Step name] — [what it accomplishes; needs what step 1 produced]

## Assumptions

- [Assumption, and how to validate it if it needs validating]

## Trade-offs & Decisions

- [Decision]: [Brief reason]

## Out of Scope

- [Item]

## Risks / Follow-ups

- [Risk, and its mitigation if there is one]
```

Out of Scope and Risks are optional — drop the heading rather than write "N/A".

**No code blocks in the plan** — no implementation, diffs, config, pseudocode, or fenced diagrams, and that includes the chain. They go stale the moment anyone writes the real thing. Name the file, function, or symbol instead — `parseConfig` in [config.ts](config.ts) — and describe the change in prose. A step or chain that can only be explained by showing code hasn't been decided yet; take it back to Phase 1.

**The document records what was agreed**, not a better plan you thought of while writing it up. If you've changed your mind, that's Phase 1 again.

## After

When implementation turns up new information, update the plan and note what changed and why. A stale plan is worse than no plan, because people keep trusting it.

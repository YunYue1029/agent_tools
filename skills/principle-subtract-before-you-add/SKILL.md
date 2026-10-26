---
name: principle-subtract-before-you-add
description: Apply when sequencing an addition, refactor, or rewrite, sizing a diff, or when tempted to add an abstraction, layer, option, or guard. Remove dead code and redundant complexity first, then build the smallest change that solves the problem on the simpler base.
user-invocable: false
---

# Subtract Before You Add

When evolving a system, remove complexity first, then build. Default to subtraction.

Leave the design slightly simpler — and no less capable — than you found it, behind the same or a smaller surface.

## Sequence

- **Removal before construction.** Delete dead code, unused options, redundant validators, and stub references before laying anything new on top.
- **Cut before you polish.** Get to the minimum first; invest in quality only on what survives.
- When a file, section, or reference has no content of its own, delete it rather than leaving a stub.

## Size

- **Minimise the diff.** Make the smallest change that solves the problem. Fewer lines beat "elegant" boilerplate.
- **Design for observed usage, not speculative edge cases.** No validators, parsers, parameters, or guards beyond what the requirement demands.
- **Consolidate decisions.** Don't repeat the same choice in several places — put it behind one source of truth and pass the result along.

Before adding a layer or an option, ask what it lets you delete. If the answer is nothing, it probably doesn't belong. See the deletion test in [codebase-design](../codebase-design/SKILL.md).

---

Adapted from [pstack](https://github.com/cursor/plugins/tree/main/pstack) (`principle-subtract-before-you-add`, `principle-laziness-protocol`), MIT © Lauren Tan.

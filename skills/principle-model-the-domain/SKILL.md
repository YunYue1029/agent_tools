---
name: principle-model-the-domain
description: Apply before writing stateful logic, or when code branches a lot, grows an if/else chain by one more case, keeps two booleans in sync, or repeats the same shape assumption across files. Encode the domain in a data structure instead of scattered conditionals.
user-invocable: false
---

# Model the Domain

Encode the real domain in a data structure instead of scattering it across conditionals. A structure that matches the domain makes invalid states unrepresentable and deletes branches. Choosing it while writing is cheap; recovering it later reads as a refactor and gets deferred.

**Name the data shape first, before the logic.** Trace how it gets read and choose the structure that serves the dominant paths.

Reach for:

- A **state machine** instead of scattered booleans, phases, or lifecycle checks.
- A **typed model** instead of loose parameters or the same shape assumed in several files.
- A **map, registry, lookup table, or discriminated union** instead of branching spread across files.
- A **reducer or command/event model** instead of ad hoc mutations.
- A module organised around **one body of domain knowledge**, not around a sequence like load → validate → transform → save. Execution order is not ownership.
- The **collection the access pattern calls for** — queue, index, tree, graph, normalised table.

When none fits, write down what the code must never allow and how the data gets read, then find the structure that encodes exactly that.

**Don't force it.** Prefer boring code when the current shape is clear, local, and unlikely to grow. Distrust an abstraction that adds indirection without removing branches, duplicated rules, or invalid states.

**The tell that you skipped this:** a new feature grows an existing if/else chain by one more branch, or adds a second boolean that must stay in sync with the first.

[codebase-design](../codebase-design/SKILL.md) covers the interface a module shows its callers; this principle covers the data shape behind it.

---

Adapted from [pstack](https://github.com/cursor/plugins/tree/main/pstack) (`principle-model-the-domain`, `principle-foundational-thinking`), MIT © Lauren Tan.

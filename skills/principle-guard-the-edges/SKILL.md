---
name: principle-guard-the-edges
description: Apply when wiring validation, error handling, parsing, or framework adapters. Concentrate guards at the system's edges — CLI args, config, network, external APIs, user input — trust internal types, and keep business logic in pure functions the shell just calls.
user-invocable: false
---

# Guard the Edges

Validate, narrow types, and handle errors where data enters the system. Trust internal code. Keep business logic in pure functions; keep the shell thin and mechanical.

An **edge** here is where the system meets the outside world — not a seam between two of its own modules (see [codebase-design](../codebase-design/SKILL.md) for those).

**Why:** scattered validation is noisy, redundant, and gives a false sense of safety. Logic tangled into framework wiring can't be tested without the framework.

## At the edges

CLI arguments, config files, environment variables, HTTP requests, external API responses, files, user input:

- Validate and return errors there, defensively.
- **Parse raw data into domain types** at the edge — once — so everything inside receives typed values.
- Validate config when it's loaded, not inside the logic that uses it.

## Inside

- Typed data, errors propagated, no re-validation. If the edge already checked it, don't null-check it three calls deeper.
- Don't leak the edge's representation inward or outward — no transport, storage, framework, or wire types in the domain's public surface.
- Business logic as pure transforms: parse is bytes → typed state, scoring is state → result, prompt building is state → string.

## The tests

- "Is this data crossing the system's edge right now?" If not, the validation is redundant.
- "Could this be a pure function the shell just calls?" If yes, extract it.

---

Adapted from [pstack](https://github.com/cursor/plugins/tree/main/pstack) (`principle-boundary-discipline`), MIT © Lauren Tan. Renamed because `codebase-design` reserves "boundary".

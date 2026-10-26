---
name: tdd
description: Test-driven development. Use when the user wants to build features or fix bugs test-first, mentions "red-green-refactor", or wants integration tests.
---

# Test-Driven Development

TDD is the red → green loop. This skill is what makes that loop produce tests worth keeping.

## What a good test is

Tests verify behavior through public interfaces, not implementation details. Code can change entirely; tests shouldn't. A good test reads like a specification — "user can checkout with valid cart" tells you exactly what capability exists — and survives refactors because it doesn't care about internal structure.

See [tests.md](tests.md) for examples and [mocking.md](mocking.md) for mocking guidelines.

### The two-edit check

Judge every test — one you're writing, changing, or keeping — against two imagined edits:

- **Break the behavior.** Make the code return a wrong value, skip an effect, or do it twice. If the test still passes, it proves nothing — usually an assertion looser than the contract, or an expected value built by the test itself.
- **Change the internals.** Keep every observable result the same and restructure underneath. If the test now fails, it's implementation-coupled and will need rewriting on every refactor while catching no bug.

Keep a test that catches the first edit and survives the second; rewrite or delete the rest.

**The contract decides, not the shape of the assertion.** The payload a mocked _external_ service receives is behavior — that service observes it. A call to an internal helper is implementation — no caller can see it. An exact error message pins wording no caller reads; the error type callers branch on is the contract. When "no call" is the promise ("an empty cart never charges"), pair it with a case where the call does happen, so the test shows it can see one.

## Seams — where tests go

Tests live at **seams** — where a module's interface lives (see the `codebase-design` skill) — never against internals. The interface is the test surface.

**Test only at pre-agreed seams.** Before writing any test, write down the seams under test and confirm them with the user. No test is written at an unconfirmed seam. You can't test everything — agreeing the seams up front is how testing effort lands on the critical paths and complex logic instead of every edge case.

Ask: "What's the public interface, and which seams should we test?"

## Anti-patterns

- **Implementation-coupled** — mocks internal collaborators, tests private methods, or verifies through a side channel (querying the database instead of using the interface). The tell: the test breaks when you refactor but behavior hasn't changed.
- **Tautological** — the assertion recomputes the expected value the way the code does (`expect(add(a, b)).toBe(a + b)`, a snapshot derived by hand the same way, a constant asserted equal to itself), so it passes by construction and can never disagree with the code. Expected values must come from an independent source of truth — a known-good literal, a worked example, the spec.
- **Horizontal slicing** — writing all tests first, then all implementation. Bulk tests verify _imagined_ behavior: you test the _shape_ of things rather than user-facing behavior, the tests go insensitive to real changes, and you commit to test structure before understanding the implementation. Work in **vertical slices** instead — one test → one implementation → repeat, each test a **tracer bullet** that responds to what the last cycle taught you.

## Rules of the loop

- **Red before green.** Write the failing test first, then only enough code to pass it. Don't anticipate future tests or add speculative features.
- **One slice at a time.** One seam, one test, one minimal implementation per cycle.
- **Refactoring is not part of the loop.** It belongs to the review stage (see the `code-review` skill), not the red → green implementation cycle.

---

Adapted from [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/engineering/tdd`), MIT © Matt Pocock. The `CONTEXT.md` / ADR lookup step was removed.
The two-edit check is adapted from [pstack](https://github.com/cursor/plugins/tree/main/pstack) (`principle-test-behavior-not-implementation`), MIT © Lauren Tan.

---
name: testing
description: Test a backend feature in three layers — unittest for the logic, testing-tool endpoint requests for each route's contract, and a testing-tool flow for the feature end to end — then run them until they pass. Use when the user asks to test a feature or endpoint, gives a test direction ("test that X can't Y"), or after building or changing an API.
---

# Testing

Test a feature in three layers, each catching what the others can't, then run every test you wrote until it passes. The user reviews what the tests check afterwards — so make that easy to see.

Two ways in:

- **After building or changing a feature** — cover what changed.
- **The user gives a direction** ("deleting a project removes its tasks", "a non-admin can't approve") — build exactly that case, in the layer it belongs to, and run it.

Invoking this skill is the request to run the tests it writes. Run those, not the whole suite, and still leave linters and formatters to the user.

## The three layers

| Layer | Tool | Proves | Typical cases |
| --- | --- | --- | --- |
| **Unit** | `unittest` | A function's logic, without HTTP or a server | Rules, calculations, parsing, validation edge cases |
| **Endpoint** | testing-tool saved request | One route answers as its contract says | Status code, response shape, required fields |
| **Flow** | testing-tool flow | Routes work together as a feature | Login → create → read → update → delete; a permission denied mid-chain; the row really in the DB |

**Put each case in exactly one layer** — the lowest that can prove it. An edge case of a calculation is a unit test, not a flow step with a crafted payload. A rule that only shows across calls (create, then the list contains it) is a flow. Before writing anything, list the cases and the layer each goes to.

## 1. Unit

Write `unittest` tests for the logic the feature adds or changes, following the project's existing test layout and naming. What makes one worth keeping — the two-edit check, no tautologies, mocks only at the system's edge — is in the [tdd](../tdd/SKILL.md) skill.

Skip this layer when the change is pure wiring with no logic of its own.

## 2. Endpoint

testing-tool is driven through its MCP server, which delivers its full usage guide on connect — `{{dy_url}}`, folders, auth, variables. Follow it; this skill only says what to put there.

- **Catalogue each route once** in the service's collection: one saved request per route, filed in the folder that matches its path. Check `get_collection` first — the route may already be there.
- **Run each new or changed request** with `run_saved_request` against the environment the feature runs in. A route that won't answer here won't pass in a flow either.

The collection is the API's catalogue, not a test suite: a case of a route (bad payload, another user's id) is a flow step, not a second saved request.

## 3. Flow

Build one flow per feature scenario with `save_flow`:

- **Steps as the MCP guide places them** — inline by default, since a step that exercises a case belongs to its flow, not the catalogue.
- **Extract** the ids and tokens later steps need; **assert** on every step — status alone rarely proves the behavior.
- **Shell steps** for what no response shows: the row in the database, the file written, the job run.
- **Mark cleanup `always: true`**, so a failed run still removes what it created.
- **A description** stating the scenario and what it assumes (existing fixtures, environment).

File it in the flow folder named after the feature.

## 4. Run until green

Run the unit tests you wrote and `run_flow` each flow. When something fails, decide which side is wrong:

- **The test is wrong** — a wrong path, an id not extracted, an assertion against what the contract never promised. Fix the test and run again.
- **The code is wrong** — the test asserts what the feature should do and the code doesn't do it. If you're building the feature, fix the code. Otherwise stop and report it with the failing output — don't change product code you weren't asked to touch.

**Never make a test pass by weakening it** — loosening an assertion, deleting a step, or asserting the buggy value. A test edited to match the code proves nothing.

Done when every test you wrote has passed on a real run.

## Hand back

For the user's review:

- **Cases** — each case, the layer it went to, and why that layer.
- **Where they are** — the unit test files, the saved requests added (collection and folder), the flow names.
- **Runs** — the command or `run_flow` call, and its result. For anything that failed along the way, what was wrong and which side you fixed.
- **Not covered** — what you deliberately left out, and anything only a person can check.

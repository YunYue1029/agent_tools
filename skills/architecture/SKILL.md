---
name: architecture
description: Reverse-engineer a project's architecture from its code into a living document — one overview of the main flows as chains of named steps, then a detail file per step, expanded one at a time. Use when the user wants to map a project's architecture, document its main flows, "畫出架構", "整理專案架構", or expand one step of an existing map.
---

# Architecture

Read the code and write down what the system does as **flows**: chains of named steps from a trigger to a result. The overview holds every flow as a skeleton; each step gets its own detail file only when someone asks to expand it. The map grows one step at a time, and at any moment it shows which parts are understood and which are still just a name.

**The code is the source.** Describe what it does, not what it should do. Don't modify code, and don't fix what you notice on the way — list it under open questions instead.

There are two jobs. Pick the one the user asked for:

- **Map** — no overview exists yet: build the skeleton.
- **Expand** — an overview exists and the user names a step: write that step's detail file.

## What a flow is

A flow is a chain a domain person would name — "agent 消費", "弱點修補" — not a module or a layer. It starts at a trigger (a request, a message, a schedule) and ends at a result, or at a handoff to another flow.

```
agent 消費
  ├─ 接收 agent 回報 ─────────► raw report
  ├─ 寫入原始資料 ────────────► 已持久化的回報
  ├─ 整理／正規化 ────────────► 軟體清單（vendor, product, version）
  ├─ 比對 CPE ────────────────► CPE 對應
  ├─ 比對 CVE ────────────────► 弱點 findings
  └─ 交給修補機制 ────────────► ⇒ 弱點修補
```

Each arrow is what the step produces for the ones after it. `⇒` marks a handoff to another flow. Name steps for what they accomplish, not how — the same naming rule as [coding-style](../../rules/coding-style.md). This chain is that rule one altitude up: expand far enough and a step's sub-chain *is* an entry function's body.

## Map

### 1. Find the triggers

Read whatever the repo documents about itself first — README, `docs/`, existing architecture notes, a glossary — and take its domain vocabulary for your names.

Then find everything that starts work: HTTP routes, queue and topic consumers, scheduled jobs, CLI commands, webhooks. For a large codebase, sweep with the Agent tool (`subagent_type=Explore`) and ask for the list of triggers with their file and symbol.

### 2. Follow the data into flows

From each trigger, follow what it produces until the work ends. The joins that matter are **asynchronous**: a message published and consumed elsewhere, a row written and later picked up by a job, a status that another process polls for. Those joins are where one trigger's work continues as the next step of the same flow — or where one flow hands off to another.

Group triggers into flows by that trail, not by directory. Several routes that feed one pipeline belong to one flow; a single service may host several flows.

### 3. Cut each flow into top-level steps

Keep the skeleton high: a handful of steps per flow, each one a stage a domain person would recognise. If a step needs more than its name and its output to explain, it's already detail — it belongs in that step's own file later. For each step record its output and its **code location** as a path plus symbol — `match_cpe` in [services/cpe.py](services/cpe.py). No line numbers; they go stale.

### 4. Confirm before writing

Show the skeleton in chat: the flows, their chains, and their handoffs. Mark every link you inferred rather than read — a queue whose consumer you couldn't find, a job you suspect is dead. Then settle, one question at a time with your recommended answer, only what the code can't tell you: what a flow is called, where one flow ends and the next begins, whether a suspicious path is still live.

### 5. Decide where it lives

There is no fixed location. Look for what the repo already has — a `docs/` tree, an architecture note, ADRs — and propose a place that fits it, with your reason. Wait for the user to confirm. The overview and its detail files sit together in one directory.

### 6. Write the overview

`<dir>/README.md`, following the template below. Every step starts as a skeleton.

## Expand

1. Find the step in the overview and read its code location, then everything it calls down to the next altitude.
2. Work out its **sub-chain** — the same chain shape, one level down — and the facts in the detail template.
3. If the code disagrees with the overview, the overview is wrong: fix it, and say what changed.
4. Show the result in chat and settle open decisions the same way as in Map. Then write `<dir>/<flow-slug>/<step-slug>.md` and link it from the overview.

A step in a detail file can be expanded the same way into its own file, next to its parent. Stop when the sub-chain would just restate an entry function's body — at that point the code is the better document.

## Templates

### Overview

```markdown
# Architecture: [project]

[One or two sentences: what the system is for.]

## Flows

- [Flow name] — [trigger] → [result]

## Flow: [name]

**Trigger:** [what starts it, with code location]

[chain diagram]

| Step | Produces | Code | Detail |
| --- | --- | --- | --- |
| [name] | [output] | `symbol` in [path](path) | [link] or skeleton |

**Hands off to:** [other flow, and what it passes]

## Open questions

- [Something the code didn't answer]
```

### Detail

```markdown
# [Flow] › [Step]

**Does:** [one sentence]
**In:** [what it receives, and from which step]
**Out:** [what it produces, and for which step]

[sub-chain diagram]

## Data it owns

- [table, queue, cache, or file — and what it holds]

## Failure modes

- [what goes wrong, and what the code does about it — or that it does nothing]

## Code

- `symbol` in [path](path) — [its role in the sub-chain]

## Open questions

- [Something the code didn't answer]
```

Drop a heading that has nothing under it rather than write "N/A".

## Rules

- **Write in the project's language.** Take the first that gives a clear answer: what the user asked for, the repo's existing docs, its recent commit messages, the language the user is writing to you in. Identifiers, paths, and terms the team writes in English stay as they are.
- **Only what the code shows.** An inferred link is marked as inferred; a gap goes under open questions, not into a plausible-sounding guess.
- **Chain diagrams yes, code no.** The diagram is the point of the document. Code snippets aren't — name the symbol and its path instead.
- **One step per expand.** Don't expand steps the user didn't ask for; the skeleton staying a skeleton is how the map shows what's still unknown.

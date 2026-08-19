---
name: coding-style
description: Split a backend flow into one entry function whose body is the chain of steps, with each step a named function holding its own detail. Use when writing or restructuring an API handler, use-case function, pipeline, or job.
---

# Coding Style — the chain is the function

An API request lands in one function. That function's body is nothing but named steps, in order. All the detail lives inside those steps.

Read the entry function and you see the whole chain:

```
run_agent_query
  │
  ├─ load_conversation ─────────────► conversation
  ├─ reject_if_quota_exhausted
  │
  ├─ retrieve_context ──────────────► context
  ├─ build_prompt ──────────────────► prompt
  ├─ call_model ────────────────────► completion
  │
  ├─ parse_answer ──────────────────► answer
  ├─ resolve_citations ─────────────► citations
  │
  └─ unit of work
       ├─ record_turn
       └─ charge_quota
```

Each arrow is a value the entry function holds and hands to a later step, so the body shows the order and the dependencies at once.

## How to split

**Cut wherever you would have written a comment.** The comment becomes the step name and the code below it becomes the body.

**Cut whenever the altitude changes.** A database query, a business rule, and an external API call don't belong in the same function. If one line talks to the ORM and the next decides policy, there's a cut between them.

**Cut fetching apart from deciding.** Load the data in one step, compute the decision from that data in the next. The decision step then takes plain values and needs no database to test.

**Name every step for what it accomplishes, not how it works.** `retrieve_context`, not `call_embedding_api`. `reject_if_quota_exhausted`, not `check_usage_table`. A name that describes the mechanism sends the reader back into every body, which defeats the whole thing.

**Pass values in and out explicitly.** Don't thread one shared mutable object through every step — it hides which step reads what, and turns the ordering into invisible coupling.

**Let steps raise errors.** Map errors to responses in one place at the edge, so the entry function stays a list of steps instead of step, check, step, check.

**Keep the steps in the same file, below the entry function, in call order.** Move one somewhere shared only when a second caller actually needs it.

## How big is a step

No line count. A step is right-sized when it has one job you can name. Stop splitting when the name would only restate the code.

A step earns its place by hiding detail the chain doesn't want:

```
retrieve_context                 ← all the chain sees
  ├─ embed the question
  ├─ search the vector store, scoped to the tenant
  ├─ return empty when nothing matches
  ├─ load the matching documents
  ├─ rerank against the question
  └─ keep the top few             ← all of this stays hidden
```

Tenant scoping, the empty case, reranking, the cut-off — none of it belongs in the entry function, and all of it is one click away.

## Don't

Don't force this onto an endpoint that reads one row and returns it. There's no flow to split.

---

The chain is decided before any code exists — see [planning](../planning/SKILL.md), which agrees it during discussion and records it as named steps. By the time you write the entry function, you're transcribing a chain that's already settled.

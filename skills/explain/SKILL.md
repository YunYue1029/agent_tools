---
name: explain
description: Explain project code — a file, function, flow, or algorithm — or a general technical concept. Use when the user asks "explain this", "what does X do", "how does X work", "what is X", or wants to understand code without reading it closely. Read-only; never modifies anything.
---

# Explain

Two modes, chosen from the question:

- **Code** — a file, function, class, flow, or algorithm in this project. Read it, then explain it.
- **Concept** — a general technical or domain question (cosine similarity, Levenshtein distance, Django ORM, HTTP caching). **Do not read project files.** Answer from knowledge, and only reach for project code if the user explicitly ties the question to it.

Picking the wrong mode is the main failure. Trawling the codebase to answer "what is a B-tree" wastes the turn; answering "what does this function do" from general knowledge invents behaviour that isn't there.

**Read-only.** No edits, no commands with side effects. If the explanation turns up a bug, name it — don't fix it.

## Explaining code

Work from the whole to the parts. Cover these in whatever order the code demands:

- **Purpose** — what problem this solves, and why it exists at all.
- **Place** — where it sits in the project, and which layer owns it (command, service, lib, model).
- **In and out** — the parameters that matter, and both the return value *and* the side effects (DB writes, generated files, logs).
- **Flow** — the steps in order, and the decision points that change the outcome: thresholds, weights, early returns, fallbacks.
- **Knobs** — the constants and defaults someone might actually tune, their current values, and which way each one moves behaviour.
- **Edges** — boundary conditions, and how this differs from an older version if one still lives alongside it (different call interface, a dependency dropped, an algorithm changed).

## Explaining a concept

- **What it is** — the idea and the problem it solves, in a paragraph.
- **Intuition** — an analogy that makes it stick.
- **The formal bit** — the formula or the algorithm's steps, briefly. Don't derive it.
- **Where it's used** — real scenarios; tie it to this project only if the connection is genuine.
- **Trade-offs** — the limitations, and the pitfalls people actually hit.

## Rules

- Explain **why it's built this way** and **what that causes**. A line-by-line translation of the code is not an explanation — the reader has seen the code, they just don't want to read it closely.
- **Don't ask what they meant when the question is already clear.** Answer it.
- Backtick every identifier and path. Short snippets or pseudo-code only; never paste a long block back at them.
- Describe only what the code does. Where you're uncertain, say which part.

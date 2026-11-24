---
name: interrogate
description: Quiz the user on the core of the changes they just made, so they understand what they changed at the big-picture level — why, what behaves differently, what it touches, where things live in the system. Use when the user says "interrogate", "quiz me on this change", "拷問我", "考我", "確認我有沒有搞懂", or after finishing a change they worry they only half understand.
---

# Interrogate

The change works, but do I understand *what I changed*? Quiz me on the core of my own diff until the gaps show.

[grilling](../grilling/SKILL.md) stress-tests a **plan** — the decisions are mine. This tests whether I **understand** a change that already exists — the facts are the system's. [explain](../explain/SKILL.md) tells me the answer; here I answer first.

**Read-only.** Never edit code. If the quiz turns up a bug, name it and move on.

## Altitude

Ask at the level of the **system and the change's purpose**, not the code.

- **Good:** "What problem did this change solve?", "Where does the signing key actually live now, and who puts it there?", "What behaves differently for a user after this?", "What else depends on what you just changed?", "What happens in prod if that key is missing?"
- **Bad:** "What does this function return?", "Which line sets the default?", "What order do these two calls run in?", "What's the name of this variable?"

If a question can only be answered by reading a specific function, it's too low. If I could answer it by explaining the change to a teammate, it's the right height.

## Steps

1. **Read the change.** `git diff` against the base branch plus uncommitted and staged changes (`git status` to see what's in play). Read around the diff only as far as needed to understand what the change means for the system.
2. **Find the core.** Boil the change down to what it really is: the problem, the new behaviour, the pieces it touches. Then look for the system-level facts I could ship without understanding:
   - **Why** — the actual cause or motivation, not "it passes now".
   - **What's different** — the behaviour before and after, as a user or operator sees it.
   - **Where it lives** — for a secret, config, or state: which system holds it (env, secret manager, config repo, DB, CI settings) and who sets it per environment. "Not in the database" is not an answer; the place is.
   - **What it touches** — other services, callers, data, or environments affected.
   - **What can go wrong** — the failure mode that matters, and how it would show up.
   - **Trade-off** — what this approach gives up versus the obvious alternative.
3. **Pick the 3–5 that matter most**, riskiest first. Skip anything the diff makes obvious.
4. **Ask one question at a time.** Wait for my answer. Never ask several at once. Don't hint — a question that contains its own answer tests nothing.
5. **Check my answer against the code and the system.** Look it up; don't judge from memory. Then reply with exactly one of:
   - **Right** — one line confirming.
   - **Partly right** — what I got and what I missed.
   - **Wrong or "don't know"** — the real answer and why it works that way, then a short follow-up to check it stuck.
   Mention a file only when it helps me find the thing; keep the answer about the system, not the code.
6. **If I say "don't know", don't pad.** Teach it, then move on.

## When the questions run out

Hand back a short list:

- **Gaps** — each thing I missed or got wrong, one line each.
- **Solid** — what I clearly understood.
- **Worth writing down** — anything a future reader (me) would trip on again that deserves a README line or config example. Suggest only; don't write it unless I ask.

## Rules

- Ask about **this change**, not the codebase in general.
- Questions stay answerable in a sentence or two, and checkable ("where is the signing key stored in prod?"), never open-ended ("how does auth work?").
- If the answer isn't in the repo (infra, secret manager, CI settings), say so and ask me where it comes from — don't guess.
- Match the language I write in.

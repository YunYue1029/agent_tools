---
name: improve-codebase-architecture
description: Scan a codebase for deepening opportunities — refactors that turn shallow modules into deep ones — present them as a visual HTML report, then grill through whichever one the user picks. Use when the user asks for an architecture review, wants to find refactoring opportunities, or says the codebase is hard to test or navigate.
---

# Improve Codebase Architecture

Surface architectural friction and propose **deepening opportunities** — refactors that turn shallow modules into deep ones. The aim is testability and navigability.

Built on the `codebase-design` skill's vocabulary (**module**, **interface**, **depth**, **seam**, **adapter**, **leverage**, **locality**) and its principles (the deletion test, "the interface is the test surface", "one adapter = hypothetical seam, two = real"). Read that skill first and use its terms exactly — don't drift into "component", "service", "API", or "boundary".

## Process

### 1. Explore

**Scope before you scan — YAGNI.** Deepening a module pays off by making future changes to it easier, so put extra weight on the parts of the codebase that have recently changed. Decide *where* to look before you look:

- If the user named a direction — a module, a subsystem, a pain point — take it, and skip the inference below.
- Otherwise, walk back a good stretch of the commit history (`git log --oneline`) to find the codebase's hot spots: the files and areas that keep coming up. Let those paths pull your attention first. If the changes are scattered with no clear hot spot, widen the net.

Read whatever the repo documents about its own conventions and domain language (`CODING_STANDARDS.md`, `CONTRIBUTING.md`, architecture notes, a glossary) before you start naming things.

Then use the Agent tool with `subagent_type=Explore` to walk the codebase. Don't follow rigid heuristics — explore organically and note where you experience friction:

- Where does understanding one concept require bouncing between many small modules?
- Where are modules **shallow** — interface nearly as complex as the implementation?
- Where have pure functions been extracted just for testability, but the real bugs hide in how they're called (no **locality**)?
- Where do tightly-coupled modules leak across their seams?
- Which parts of the codebase are untested, or hard to test through their current interface?

Apply the **deletion test** to anything you suspect is shallow: would deleting it concentrate complexity, or just move it? A "yes, concentrates" is the signal you want.

### 2. Present candidates as an HTML report

Write a self-contained HTML file to the OS temp directory so nothing lands in the repo. Resolve the temp dir from `$TMPDIR`, falling back to `/tmp` (or `%TEMP%` on Windows), and write to `<tmpdir>/architecture-review-<timestamp>.html` so each run gets a fresh file. Open it for the user — `open <path>` on macOS, `xdg-open <path>` on Linux, `start <path>` on Windows — and tell them the absolute path.

Each candidate gets a card centred on a **before/after diagram** and a recommendation-strength badge; the report ends with a **Top recommendation** — which candidate you'd tackle first and why. [HTML-REPORT.md](HTML-REPORT.md) has the scaffold, the card fields, the diagram patterns, and the tone.

Use the repo's own domain vocabulary for the nouns and the `codebase-design` glossary for the architecture. If the codebase calls it an "Order", talk about "the Order intake module" — not "the FooBarHandler", and not "the Order service".

Do NOT propose interfaces yet. After the file is written, ask the user: "Which of these would you like to explore?"

### 3. Grilling loop

Once the user picks a candidate, run the `grilling` skill to walk the decision tree with them — constraints, dependencies, the shape of the deepened module, what sits behind the seam, what tests survive.

As decisions crystallise:

- **Classify the dependencies** using [DEEPENING.md](../codebase-design/DEEPENING.md) — the category decides whether the seam needs a port at all, and what the tests run against.
- **Exploring alternative interfaces?** Use the design-it-twice parallel sub-agent pattern in [DESIGN-IT-TWICE.md](../codebase-design/DESIGN-IT-TWICE.md).
- **Ready to build?** Hand off to the `tdd` skill — the deepened module's interface is the seam its tests are written at, and the old shallow-module tests get deleted rather than layered on.

---

Adapted from [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/engineering/improve-codebase-architecture`), MIT © Matt Pocock. The `CONTEXT.md` / ADR side effects and the `domain-modeling` handoff were removed.

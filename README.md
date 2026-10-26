# agent_tools

Tooling for AI coding agents: a collection of [Claude Code skills](skills/)
for software-development workflows, plus [rules](rules/) for conventions that
should apply without being asked for.

## Skills

Each skill in [`skills/`](skills/) is a directory with a `SKILL.md`, plus any
supporting files it links to. A skill's body loads only when you invoke it with
`/name` or Claude judges it relevant.

They are meant to be used together — `grilling` is the shared discussion
discipline and `codebase-design` the shared vocabulary, and the larger
skills delegate into both.

| Skill | What it does |
| --- | --- |
| [`grilling`](skills/grilling/SKILL.md) | Relentless one-question-at-a-time interview to stress-test an idea |
| [`planning`](skills/planning/SKILL.md) | Grill a vague goal into ordered steps, then write the plan to `z/` |
| [`codebase-design`](skills/codebase-design/SKILL.md) | Deep-module vocabulary: module, interface, depth, seam, adapter |
| [`improve-codebase-architecture`](skills/improve-codebase-architecture/SKILL.md) | Scan for deepening opportunities, report them visually, then grill |
| [`code-review`](skills/code-review/SKILL.md) | Two-axis review — Standards and Spec — in parallel sub-agents |
| [`blast-radius`](skills/blast-radius/SKILL.md) | What a change breaks outside its diff, with the one safety fact proven by running code |
| [`correct`](skills/correct/SKILL.md) | Turn mistakes agents keep repeating into architecture, types, lints, or tests |
| [`tdd`](skills/tdd/SKILL.md) | The red-green loop, and what makes a test worth keeping |
| [`diagnosing-bugs`](skills/diagnosing-bugs/SKILL.md) | Six-phase loop for hard bugs; build the feedback loop first |
| [`explain`](skills/explain/SKILL.md) | Explain project code, or a general technical concept |
| [`pr-description`](skills/pr-description/SKILL.md) | Reviewer-friendly PR description from the branch diff |
| [`origin-sync`](skills/origin-sync/SKILL.md) | Rebase onto `origin/main` and force-push back safely |

## Principles

Principles sit between skills and rules. Like rules, you never invoke them — but
they are triggered by a **situation** ("about to declare done", "about to add a
layer"), not by a file path, so they ship as skills marked `user-invocable: false`:
hidden from the `/` menu, loaded when Claude judges the situation matches.

| Principle | Applies when |
| --- | --- |
| [`principle-prove-it-works`](skills/principle-prove-it-works/SKILL.md) | Before declaring done — verify the real artifact, not a proxy |
| [`principle-subtract-before-you-add`](skills/principle-subtract-before-you-add/SKILL.md) | Adding, refactoring, or sizing a diff — remove first, then build small |
| [`principle-encode-lessons-in-structure`](skills/principle-encode-lessons-in-structure/SKILL.md) | Writing the same instruction twice — make it a check instead |
| [`principle-model-the-domain`](skills/principle-model-the-domain/SKILL.md) | Writing stateful or branchy logic — encode the domain in a structure |
| [`principle-guard-the-edges`](skills/principle-guard-the-edges/SKILL.md) | Wiring validation or adapters — guard where data enters, trust types inside |

They install with the skills — the `ln -s` command below picks them up.

## Rules

Rules in [`rules/`](rules/) are conventions rather than procedures — you would never
think to invoke them by name, they should simply hold while the relevant code is being
written. Each is a markdown file whose `paths` frontmatter scopes it, so it enters
context only when Claude opens a matching file.

| Rule | Applies to |
| --- | --- |
| [`coding-style`](rules/coding-style.md) | Backend entry points — the chain is the function, nodes hold the detail |

Tune each rule's `paths` globs to the layout of the project you drop it into; a glob
that matches nothing means the rule never loads.

## Installing

Claude Code discovers skills in `~/.claude/skills/` (personal, available in
every project) and `.claude/skills/` (project-local). Symlink them so edits
here take effect immediately:

```bash
ln -s "$PWD/skills"/* ~/.claude/skills/
```

Or copy a single skill if you only want one:

```bash
cp -r skills/grilling ~/.claude/skills/
```

Rules live alongside them, in `~/.claude/rules/` or a project's `.claude/rules/`.
They are usually worth scoping per project rather than installing globally:

```bash
cp rules/coding-style.md /path/to/project/.claude/rules/
```

## Language

The skills and rules are written in English, and answer in English unless asked otherwise.
Artifacts that belong to a project — such as the PR description `pr-description` writes — follow
that project's language instead, so one install works across repos with different conventions.

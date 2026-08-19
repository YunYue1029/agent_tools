# agent_tools

Tooling for AI coding agents: a collection of [Claude Code skills](skills/)
for software-development workflows, plus [rules](rules/) for conventions that
should apply without being asked for.

## Skills

Ten skills in [`skills/`](skills/), each a directory with a `SKILL.md`. A skill's
body loads only when you invoke it with `/name` or Claude judges it relevant, so
length costs nothing until it is used.

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
| [`tdd`](skills/tdd/SKILL.md) | The red-green loop, and what makes a test worth keeping |
| [`diagnosing-bugs`](skills/diagnosing-bugs/SKILL.md) | Six-phase loop for hard bugs; build the feedback loop first |
| [`explain`](skills/explain/SKILL.md) | Explain project code, or a general technical concept |
| [`pr-description`](skills/pr-description/SKILL.md) | Reviewer-friendly PR description from the branch diff |
| [`origin-sync`](skills/origin-sync/SKILL.md) | Rebase onto `origin/main` and force-push back safely |

See [`skills/README.md`](skills/README.md) for the full descriptions and file layout.

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

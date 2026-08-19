# agent_tools

Tooling for AI coding agents. Currently one thing: a collection of
[Claude Code skills](skills/) for software-development workflows.

## Skills

Eleven skills in [`skills/`](skills/), each a directory with a `SKILL.md`.
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
| [`coding-style`](skills/coding-style/SKILL.md) | The chain is the function; nodes hold the detail |
| [`explain`](skills/explain/SKILL.md) | Explain project code, or a general technical concept |
| [`pr-description`](skills/pr-description/SKILL.md) | Reviewer-friendly PR description from the branch diff |
| [`origin-sync`](skills/origin-sync/SKILL.md) | Rebase onto `origin/main` and force-push back safely |

See [`skills/README.md`](skills/README.md) for the full descriptions and file layout.

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

## Language

The skills are written in English and answer in English unless asked otherwise.

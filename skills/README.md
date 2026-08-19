# Agent Skills

A collection of structured skills for AI agents to perform specific tasks in software development workflows. Each skill provides detailed guidelines, instructions, and best practices for consistent and high-quality execution.

## Overview

This repository contains the following skills designed to help AI agents assist developers more effectively:

1. **Explain Skill** - Provides clear explanations of code, flows, and technical concepts
2. **PR Description Skill** - Generates structured, reviewer-friendly PR descriptions
3. **Planning Skill** - Goal decomposition with discussion-first workflow; clarify and decompose through conversation (run as a grilling), agree each feature's chain of steps, then write the plan to `z/` after convergence
4. **Grilling Skill** - Relentless one-question-at-a-time interview to stress-test a plan, decision, or idea; used standalone, and as the discussion discipline inside Planning
5. **Code Review Skill** - Two-axis review of the diff since a fixed point: Standards (repo conventions plus a Fowler smell baseline) and Spec (does it implement what was asked?), run as parallel sub-agents
6. **TDD Skill** - Red-green loop reference: what a good test is, where seams go, the anti-patterns, and the rules of the loop
7. **Diagnosing Bugs Skill** - Six-phase loop for hard bugs and performance regressions: build a red-capable feedback loop first, then reproduce, minimise, hypothesise, instrument, fix, and clean up
8. **Origin Sync Skill** - Rebase the current branch onto the latest `origin/main`, resolve conflicts hunk by hunk, and force-push it back with `--force-with-lease`
9. **Codebase Design Skill** - Shared vocabulary and principles for designing deep modules: a lot of behaviour behind a small interface, placed at a clean seam, testable through that interface
10. **Improve Codebase Architecture Skill** - Scan the codebase for deepening opportunities, present them as a visual before/after HTML report, then grill through whichever one you pick
11. **Coding Style Skill** - Write backend entry points as a legible chain: one function whose body is the flow, with each step a named node holding its own detail

## Usage

Each skill is a standalone Markdown file that can be:
- Used as a reference guide for AI agents
- Integrated into agent workflows
- Customized for specific project needs

### For AI Agents

When an agent needs to perform one of these tasks:
1. Read the corresponding `SKILL.md` file
2. Follow the instructions and guidelines provided
3. Use the checklists to ensure completeness
4. Output results in the specified format

## File Structure

```
skills/
├── README.md                    # This file
├── explain/
│   └── SKILL.md                # Explain skill guidelines
├── pr-description/
│   ├── SKILL.md                # PR description skill guidelines
│   └── EXAMPLE.md              # Good vs bad PR description, side by side
├── planning/
│   └── SKILL.md                # Planning (goal decomposition) skill guidelines
├── grilling/
│   └── SKILL.md                # Interview discipline; drives Planning's discussion phase
├── code-review/
│   └── SKILL.md                # Two-axis code review (Standards / Spec)
├── tdd/
│   ├── SKILL.md                # Test-driven development loop
│   ├── tests.md                # Good vs bad test examples
│   └── mocking.md              # When and how to mock
├── diagnosing-bugs/
│   ├── SKILL.md                # Six-phase diagnosis loop for hard bugs
│   └── scripts/
│       └── hitl-loop.template.sh   # Human-in-the-loop repro harness
├── origin-sync/
│   └── SKILL.md                # Rebase onto origin/main and force-push back
├── coding-style/
│   └── SKILL.md                # The chain is the function; nodes hold the detail
├── codebase-design/
│   ├── SKILL.md                # Deep-module vocabulary and design principles
│   ├── DEEPENING.md            # Deepening a cluster, by dependency category
│   └── DESIGN-IT-TWICE.md      # Parallel sub-agents exploring rival interfaces
└── improve-codebase-architecture/
    ├── SKILL.md                # Scan for deepening opportunities, then grill
    └── HTML-REPORT.md          # Report scaffold, diagram patterns, tone
```

## Language

All skills are written in **English** and default to using English for responses, unless the user specifically requests another language.
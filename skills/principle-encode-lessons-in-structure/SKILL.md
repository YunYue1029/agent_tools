---
name: principle-encode-lessons-in-structure
description: Apply when you catch yourself writing the same instruction a second time, or notice a correction recurring. Encode the rule as a lint, type, metadata flag, runtime check, or script instead of adding more text.
user-invocable: false
---

# Encode Lessons in Structure

An instruction that has to be repeated is a symptom. Text gets skipped; a check that fails does not.

When a lesson recurs, encode it where breaking it fails loudly — a type that can't represent the bad state, a lint whose error names the fix, a runtime assertion, a script that checks it. If the fix is structural, use only the structural fix; don't also add the instruction.

If the lesson needs judgment and can't be encoded, make the instruction more prominent and add an example of the failure it prevents.

For a systematic pass over a repo's recurring mistakes, use the [correct](../correct/SKILL.md) skill.

---

Adapted from [pstack](https://github.com/cursor/plugins/tree/main/pstack) (`principle-encode-lessons-in-structure`), MIT © Lauren Tan.

---
name: apply-coding-guidelines
description: Audit the source code against the platform coding guidelines in `references/coding-guidelines.md` and fix the violations. Use when the user asks to check, enforce, apply or clean up coding conventions or naming, after generating or changing larger amounts of code, or when code may not follow the guidelines. Reports violations per file, applies behaviour-preserving fixes after confirmation and verifies with the project's lint and type check.
---

# Apply coding guidelines

Brings the project's source code in line with the platform coding guidelines without changing behaviour.

## Rules

- [references/coding-guidelines.md](references/coding-guidelines.md) is the single source of truth. Read it at the start of every run. Never copy its rules into this skill, into `.ai/project/` or into code comments, and never edit it in a project.
- Do not weaken, override or disable lint rules, and do not edit the lint configuration to make violations disappear.
- Skip generated files and anything the guidelines declare exempt.
- Fix violations only. Do not refactor, reformat or fix unrelated issues.
- Preserve behaviour. A fix may change names or form, never logic, exports or public signatures.
- Never rename exported symbols, or anything referenced by generated files or by an external system, without asking. Exported names belong to the API of other modules.
- Write changes only after the user has confirmed the audit.

## Steps

1. Read [references/coding-guidelines.md](references/coding-guidelines.md) and `.ai/project/project.md`. Derive a checklist of every rule that can be verified in code, not just the naming rules.
2. Find the project's lint and type-check commands (see `package.json` and `.ai/project/project.md`) and run them to record the baseline. If either fails before any change, report it and stop.
3. Audit every source file, skipping generated files. For each rule in the checklist, list the violations per file with the current and the proposed name or form. Use grep for the candidates, then check each hit by reading the code. Do not rely on grep alone: a non-exported function may be a hook or a component and therefore exempt.
4. Present the audit as a table per file: rule, symbol, proposed fix. Mention rules that the linter does not enforce, so the user knows which ones need this skill. Wait for confirmation. Offer to apply it per file or per rule.
5. Apply the confirmed fixes with the edit tool. For every rename:
   - Change the declaration and every usage. Find all references with code intelligence (for example a usages or rename tool) or with word-boundary grep.
   - Check for name clashes and shadowing. If the new name already exists in the scope, stop and ask.
   - Work file by file and keep each file compiling.
6. Verify with the project's lint and type-check commands. Both must pass. If a check fails, fix the cause or revert that change. Do not claim a pass that was not run.
7. Re-run the audit and confirm no violations remain.
8. Report what changed per file, what was left alone on purpose and why, and which checks were not run.

## Scope tips

- Large files can be handled in separate passes, one rule at a time. Independent files can be split across parallel agents, but verify the result yourself afterwards.
- A rule that the linter cannot check is a candidate to raise with the platform team. Suggest it, do not add the rule to the project.

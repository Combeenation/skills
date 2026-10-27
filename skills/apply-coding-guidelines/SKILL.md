---
name: apply-coding-guidelines
description: Audit source code against the current Combeenation coding guidelines at https://github.com/Combeenation/ai-guidelines/blob/master/custom-code/CODING-GUIDELINES.md. Use ONLY when the user explicitly requests checking, ensuring or enforcing compliance with coding conventions or coding guidelines.
---

# Apply coding guidelines

Brings the project's source code in line with the platform coding guidelines without changing behaviour.

## Rules

- Run this skill only in response to an explicit user request to ensure or audit coding-convention compliance. Never run it proactively as a follow-up to normal code work.
- [Combeenation coding guidelines](https://github.com/Combeenation/ai-guidelines/blob/master/custom-code/CODING-GUIDELINES.md) are the single source of truth. Retrieve and read the current version at the start of every run. Never rely on cached copies or previous reads, copy the rules into this skill, `.ai/project/` or code comments, or edit the guidelines in a customer project. If the guidelines cannot be retrieved or read, report this and stop without auditing or modifying code.
- Do not weaken, override or disable lint rules, and do not edit the lint configuration to make violations disappear.
- Skip generated files and anything the guidelines declare exempt.
- Fix violations only. Do not refactor, reformat or fix unrelated issues.
- Preserve behaviour. A fix may change names or form, never logic, exports or public signatures.
- Never rename exported symbols, or anything referenced by generated files or by an external system, without asking. Exported names belong to the API of other modules.
- Write changes only after the user has confirmed the audit.

## Steps

1. Retrieve and read the full [Combeenation coding guidelines](https://github.com/Combeenation/ai-guidelines/blob/master/custom-code/CODING-GUIDELINES.md) and `.ai/project/project.md`. Derive a checklist of every rule that can be verified in code, not just the naming rules. If the guidelines are unavailable, stop and report the failure.
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

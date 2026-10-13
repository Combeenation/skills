---
name: update-project-description
description: Update the project description in `.ai/project/` from the code base. Use when the user asks to sync, refresh, review or extend the project description, after larger code changes, or when `.ai/project/` may be stale. Reads the repository, compares it with `.ai/project/`, proposes changes and writes them after confirmation.
---

# Update project description

Keeps `.ai/project/` (`project.md` and `glossary.md`) accurate. It is the only place in the repository the agent maintains as project description.

## Rules

- Edit only files in `.ai/project/`. Never edit the reference files of the `combeenation-cpq` or `apply-coding-guidelines` skills.
- Do not restate platform behaviour, platform libraries, platform-wide coding rules or platform terms. Those belong in the references of the `combeenation-cpq` and `apply-coding-guidelines` skills. Link to them instead.
- Separate what the code proves from what only a person knows. Never present an inference as a confirmed decision.
- Describe patterns and point to files. Do not copy code into the description.
- Write changes only after the user has confirmed the proposal.

## Steps

1. Read `.ai/project/` and the platform references: `knowledge.md` and `glossary.md` of the `combeenation-cpq` skill, `coding-guidelines.md` of the `apply-coding-guidelines` skill.
2. Scan the repository for facts relevant to each section of `.ai/project/project.md` and to the terms in `.ai/project/glossary.md`:
   - **Stack and versions:** `package.json`, `tsconfig.json`, `vite.config.js`, `eslint.config.mjs`.
   - **Structure and conventions:** the `src/` folder layout, file naming, hooks and component patterns.
   - **Product:** `src/index.ts`, the root component, and the configurator components used from `src/typings/cfgr-defs.generated.ts`.
   - **Constraints:** unusual dependencies (for example local tarballs), pinned versions, lint or type-check scope.
3. Compare the findings with `.ai/project/` and sort them:
   - **Added:** facts in the code that the description lacks.
   - **Changed:** statements that no longer match the code.
   - **Stale:** paths, scripts, names or components that no longer exist.
   - **Platform content:** anything in `.ai/project/` that duplicates the platform references. Propose removing it.
4. Ask the user about what the code cannot show: purpose, scope and out of scope, reasons behind decisions, constraints without a trace in the code. Ask one focused question at a time. Put unanswered items under "Open questions".
5. Present the proposal grouped as Added, Changed, Stale and Platform content, marking inferred items as such. Wait for confirmation.
6. Apply the confirmed changes. Keep the existing section structure and style. Set "Last reviewed" to today's date.
7. Report what changed, what remains an open question, and which checks were not run.

## Size

If `.ai/project/project.md` grows beyond a few hundred lines, propose splitting it into topic files in `.ai/project/` (for example `architecture.md`, `conventions.md`, `decisions.md`) with `project.md` as the index. Do the split only after the user confirms.

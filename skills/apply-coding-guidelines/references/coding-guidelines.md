# Coding guidelines

> Scope: Platform description. Identical in every project and owned by the platform team. Do not edit or adapt it inside a project.  
> Owner: Platform team  
> Last verified: 2026-10-12

Coding conventions that apply in every project. Project-specific conventions (stack, folder structure) belong in `.ai/project/project.md`. Platform capabilities and approved libraries are in `knowledge.md` of the `combeenation-cpq` skill.

## Naming

- Module-level constants and `let`s: exported ones are `UPPER_SNAKE_CASE` (`CUBE_WIDTH`). Non-exported ones are `UPPER_SNAKE_CASE` with a `_` prefix (`_CUBE_GAP`).
- Non-exported functions are prefixed with `_` (`_syncCubeAnchors`).
- Components are never prefixed, whether exported or not. Hooks and generated files are exempt.

## Linting (ESLint)

Every project extends the shared `@combeenation/eslint-config` in `eslint.config.mjs`. The package is the source of truth for the rules (`node_modules/@combeenation/eslint-config/index.js`). Do not weaken or override them without approval, and do not copy them into this file.

Run the project's lint command before finishing a change. Which files a project lints is set in its own `eslint.config.mjs`.

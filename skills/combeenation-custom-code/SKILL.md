---
name: combeenation-custom-code
description: Custom Code, the TypeScript that runs inside a Combeenation configurator. Use before any platform-dependent change in a Custom Code project (SDK, ValueComponent, CustomControl, generated typings, 3D viewer, entry point, build and sync tooling, approved libraries).
---

# Combeenation Custom Code

This skill is the source of truth for what Custom Code can do and which libraries it may use. Read the references before platform-dependent work, and never edit them in a project:

- [references/knowledge.md](references/knowledge.md): capabilities (entry point, `ValueComponent`, `CustomControl`, generated typings, 3D viewer, build and sync tooling) and approved libraries.
- [references/glossary.md](references/glossary.md): platform terms.

Do not claim a capability that these files do not document without evidence. If they are silent, contradict the project or each other, name the gap and flag it instead of resolving it silently.

Custom Code reads and writes configurator components, which are defined in Hive. When a change needs a new or changed component, use `combeenation-hive` too. Coding rules are in `apply-coding-guidelines`; project specifics are in `.ai/project/`.

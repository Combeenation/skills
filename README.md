# Combeenation AI Skills

Agent skills for the [Combeenation](https://docs.combeenation.com) CPQ platform, in the open [Agent Skills](https://agentskills.io) format.

## Install

```sh
npx skills add combeenation/skills
```

## Skills

| Skill                                                                      | Purpose                                                                  |
| -------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| [`combeenation`](skills/combeenation/SKILL.md)                             | Entry point: links the project to its configurator, routes to the skills |
| [`combeenation-hive`](skills/combeenation-hive/SKILL.md)                   | Hive, the rule language of configurators: model, syntax, traps           |
| [`combeenation-custom-code`](skills/combeenation-custom-code/SKILL.md)     | Custom Code capabilities, SDK, tooling, and approved libraries           |
| [`apply-coding-guidelines`](skills/apply-coding-guidelines/SKILL.md)       | Audit and fix code against the platform coding guidelines                |
| [`update-project-description`](skills/update-project-description/SKILL.md) | Keep `.ai/project/` in sync with the code base                           |

## Authoring

- One folder per skill under `skills/`; the folder name must equal the `name` in `SKILL.md`.
- Keep `SKILL.md` short and push branch-specific material into `references/`.
- The MCP server describes its own tools. Skills carry workflows and domain knowledge, never copies of tool signatures.

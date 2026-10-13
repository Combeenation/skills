# Combeenation AI Skills

Agent skills for the [Combeenation](https://docs.combeenation.com) CPQ platform, in the open [Agent Skills](https://agentskills.io) format.

## Install

```sh
npx skills add combeenation/skills
```

## Skills

| Skill                                                  | Purpose                                                              |
| ------------------------------------------------------ | -------------------------------------------------------------------- |
| [`combeenation-cpq`](skills/combeenation-cpq/SKILL.md) | Entry point for working on a configurator from a Custom Code project |

## Authoring

- One folder per skill under `skills/`; the folder name must equal the `name` in `SKILL.md`.
- Keep `SKILL.md` short and push branch-specific material into `references/`.
- The MCP server describes its own tools. Skills carry workflows and domain knowledge, never copies of tool signatures.

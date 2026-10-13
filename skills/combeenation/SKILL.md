---
name: combeenation
description: Entry point for the Combeenation CPQ platform. Use when the user mentions Combeenation or a configurator, or in a project that has a cbn.config.json.
---

# Combeenation

Combeenation is a SaaS platform for building CPQ configurators. A configurator's business logic is written in Hive, a rule language organised into components that runs on Combeenation servers. Custom Code, TypeScript that runs in the browser, complements it. This skill links you to the configurator behind the project and routes you to the skill for each area.

## Which skill when

- **Hive**: rules, components, queries. Use `combeenation-hive` before reading or writing any Hive.
- **Custom Code**: TypeScript in the project, the SDK, generated typings, the 3D viewer, build tooling, approved libraries. Use `combeenation-custom-code` before any platform-dependent change.
- **Coding rules** for the project's code: `apply-coding-guidelines`.
- **Project specifics** (product, stack, conventions) are in `.ai/project/`, maintained with `update-project-description`.

Many tasks span areas, for example a Custom Code feature that needs a new Hive component. Use every skill the task touches.

## Find the target configurator

A Custom Code project is linked to its configurator in `cbn.config.json` in the project root:

```json
{
  "cfgrLinks": {
    "companies": [{ "id": "ACME", "cfgrs": [{ "id": "SHOP" }] }]
  }
}
```

Read it before using any Combeenation tool. `companies[].id` is the company id and `cfgrs[].id` is a configurator id of that company; ids match case-insensitively.

1. One configurator listed: use that company id and configurator id.
2. Several listed: ask the user which one the task concerns.
3. File missing, unparsable, or listing no configurator: ask the user to link it. In the configurator editor's Custom Code section, "Connect to local IDE" writes the file. Take ids only from this file or from the user.

You are done when you hold exactly one company id and one configurator id. Pass both on every Combeenation tool call that takes them.

## Combeenation tools

Live access to the platform (components, drafts, assets) comes from the Combeenation MCP server. If its tools are not available in this session, read [references/mcp-setup.md](references/mcp-setup.md) and help the user connect it.

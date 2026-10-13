---
name: combeenation-cpq
description: Combeenation CPQ configurators. Use in a Custom Code (TypeScript) project that has a cbn.config.json, when working on Hive or configurator components, or when the user mentions Combeenation.
---

# Combeenation CPQ

Combeenation is a SaaS platform for building CPQ configurators. A configurator's business logic is written in Hive, a backend language organised into components, and complemented by Custom Code: TypeScript that runs in the browser. This skill gives you what you need to work on the configurator behind the project you are in.

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
3. File missing, unparsable, or listing no configurator: ask the user to link it. In the configurator editor, with the IDE connected, "Link configurator" writes the file. Take ids only from this file or from the user.

You are done when you hold exactly one company id and one configurator id. Pass both on every Combeenation tool call that takes them.

## Combeenation tools

Live access to the platform (components, drafts, assets) comes from the Combeenation MCP server. If its tools are not available in this session, read [references/mcp-setup.md](references/mcp-setup.md) and help the user connect it.

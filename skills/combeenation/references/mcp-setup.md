# Connect the Combeenation MCP server

User-facing guide: https://docs.combeenation.com/docs/ai/mcp-server

- Server URL: `https://portal.combeenation.com/mcp` (HTTP transport)
- Auth: header `Authorization: Bearer <token>`, where `<token>` is a Combeenation API token. The user creates it in Combeenation ([docs](https://docs.combeenation.com/docs/integrations-and-api/integrations#bearer-token-authentication)).
- A token belongs to one company. Use a token of the company listed in the project's `cbn.config.json`.

## VS Code

Configure the server per project in `.vscode/mcp.json`. The command "MCP: Open Workspace Folder MCP Configuration" opens it. Add:

```json
{
  "inputs": [
    {
      "type": "promptString",
      "id": "combeenation-api-token",
      "description": "Combeenation API token",
      "password": true
    }
  ],
  "servers": {
    "cbn-mcp": {
      "type": "http",
      "url": "https://portal.combeenation.com/mcp",
      "headers": {
        "Authorization": "Bearer ${input:combeenation-api-token}"
      }
    }
  }
}
```

VS Code prompts for the token when the server first starts and stores it securely, so the file holds no secret and may be committed.

If no prompt appears or authentication fails, replace `${input:combeenation-api-token}` with the literal token and drop the `inputs` block. The file now holds a secret. Before anything else, add `.vscode/mcp.json` to `.gitignore`, and if it is already tracked, run `git rm --cached .vscode/mcp.json`.

## Other agents

If the agent does not read `.vscode/mcp.json` or cannot prompt for input (for example VS Code's Agent Host, Claude Code, Cursor), add the same URL and header to that agent's own MCP config:

- Preferred: a project-level config file that is listed in `.gitignore`.
- Alternative: the agent's user-level config. It holds one token for all projects, so it does not work when the user works with several companies.

## Rules

- Never write a literal token into a file that git tracks. Check with `git check-ignore <file>` first.
- If a token was ever committed, tell the user to revoke it and create a new one. It stays in git history even after removal.
- After setup, confirm the Combeenation tools are available before continuing the task.

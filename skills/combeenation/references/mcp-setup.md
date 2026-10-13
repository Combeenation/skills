# Connect the Combeenation MCP server

The server is configured per project, in the workspace MCP config `.vscode/mcp.json`. In VS Code, run "MCP: Open Workspace Folder MCP Configuration" to open it, and add:

```json
{
  "inputs": [
    {
      "type": "promptString",
      "id": "combeenation-api-token",
      "description": "Combeenation API Key",
      "password": true
    }
  ],
  "servers": {
    "cbn-mcp": {
      "url": "https://127.0.0.1/mcp",
      "type": "http",
      "headers": {
        "Authorization": "Bearer ${input:combeenation-api-token}"
      }
    }
  }
}
```

VS Code prompts for the token when the server first starts and stores it securely, so the file holds no secret. The user gets their token from Combeenation ([docs.combeenation.com](https://docs.combeenation.com/docs/integrations-and-api/integrations#bearer-token-authentication)).

If the user's agent does not read `.vscode/mcp.json` or cannot prompt for input (for example VS Code's Agent Host, Claude Code, Cursor), add the same server to that agent's own MCP config. A literal token may only live in a file outside version control: a user-level config, or a project file listed in `.gitignore` and untracked with `git rm --cached`. If a token was ever committed, tell the user to revoke it; it stays in git history.

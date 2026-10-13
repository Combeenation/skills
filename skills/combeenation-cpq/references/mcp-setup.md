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

VS Code prompts for the token when the server first starts and stores it securely, so the file holds no secret. The user gets their token from Combeenation ([docs.combeenation.com](https://docs.combeenation.com/docs/integrations-and-api/integrations#bearer-token-authentication)). Never write a token into the file itself: `.vscode/` is under version control. If the file already contains a literal token, tell the user to replace it with the input variable and revoke the token.

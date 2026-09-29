# Installing the trip1 MCP server

trip1 is a hosted remote MCP server. There is nothing to clone, build or install, and no API key or OAuth.

Add this entry to the client's MCP configuration:

```json
{
  "mcpServers": {
    "trip1": {
      "type": "streamable-http",
      "url": "https://trip1.com/api/mcp"
    }
  }
}
```

If the client only supports stdio servers, use the bridge instead:

```json
{
  "mcpServers": {
    "trip1": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://trip1.com/api/mcp"]
    }
  }
}
```

Verify by calling `search_hotels` with a destination name and future `check_in` / `check_out` dates in `YYYY-MM-DD` form. A list of hotels with prices means the server is connected.

Before calling `purchase_hotel`, show the user the full booking summary and get explicit confirmation. Collect every guest field from the user; never invent one.

<p align="center"><img src="logo.png" width="96" alt="trip1 logo"></p>

# trip1 MCP server

[trip1](https://trip1.com) lets AI agents search and book hotels: about 3 million properties in 200+ countries. Agents pay per reservation in USDC on Base over [x402](https://x402.org), so there's no account, API key or OAuth to set up.

- **Endpoint:** `https://trip1.com/api/mcp` (Streamable HTTP)
- **Auth:** none
- **Docs:** https://trip1.com/agents
- **MCP Registry:** `com.trip1/mcp`

This repository holds the public listing and install docs. The server itself is hosted by trip1, so there is nothing to run locally.

## Tools

| Tool | What it does |
|------|--------------|
| `search_hotels` | Search available hotels by destination name and dates (`YYYY-MM-DD`). Sort by price, rating or distance. Prices are for 2 adults in one room. |
| `get_hotel_details` | Rooms, rates, cancellation terms and live availability for a hotel from `search_hotels`. Returns the rate IDs that `purchase_hotel` needs. |
| `purchase_hotel` | Create a reservation from a rate ID and guest details. Returns a `payment_url` with an x402 challenge (USDC on Base), or a browser crypto checkout when `payment_service` is `coingate`. |
| `get_order_details` | Poll an order after payment until `ready` is true. |

## Install

### Claude Code

```bash
claude mcp add --transport http trip1 https://trip1.com/api/mcp
```

### Claude Desktop, Cursor, Windsurf, Cline

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

Cursor: `~/.cursor/mcp.json`. Cline: *MCP Servers → Configure → Remote Servers*. Clients that only speak stdio can bridge with `npx -y mcp-remote https://trip1.com/api/mcp`.

### VS Code

```bash
code --add-mcp '{"name":"trip1","type":"http","url":"https://trip1.com/api/mcp"}'
```

### ChatGPT

Search for **trip1** in the ChatGPT apps directory.

## Paying for a booking

1. `purchase_hotel` returns a `payment_url`.
2. Fetch it. The response is an HTTP 402 challenge (x402 v2) for the exact USDC amount on Base.
3. Pay it with any x402 v2 client, which signs the payment and retries the request.
4. Poll `get_order_details` until `ready` is true.

Agents without an x402 wallet can pass `payment_service: "coingate"` and hand the user a browser checkout that takes 120+ cryptocurrencies.

## Links

- Website: https://trip1.com
- Agent docs: https://trip1.com/agents
- Server card: https://trip1.com/.well-known/mcp.json
- x402 metadata: https://trip1.com/.well-known/x402.json
- Support: https://trip1.com/help
- Privacy: https://trip1.com/privacy-policy
- Terms: https://trip1.com/terms-and-conditions

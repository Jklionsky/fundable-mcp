# Fundable MCP Server

Query [Fundable's](https://tryfundable.ai) proprietary venture-capital dataset in natural
language — funding rounds, investors, valuations, founders, firms, and the relationships
between them — directly from Claude or any MCP-compatible client.

This is a **remote, hosted MCP server**. There's nothing to install or run locally; you connect
to the hosted endpoint and authenticate with your Fundable account.

- **Endpoint:** `https://fundable-mcp.jacob-57a.workers.dev/mcp`
- **Transport:** Streamable HTTP (SSE also available at `/sse`)
- **Auth:** OAuth 2.1 (sign in with your Fundable account) — **requires an active Fundable
  subscription**. Don't have one? [Sign up at tryfundable.ai](https://tryfundable.ai).

> This repo contains documentation and the MCP manifest only. The server source is proprietary.

## Tools

| Tool | What it does |
|------|--------------|
| `getDatasetContext` | Returns the dataset schema, business rules, and query guidelines. |
| `listDatasetTables` | Quick overview of the available tables. |
| `getQueryExamples` | Example queries by category (funding, investors, people, industries, geography, and more). |
| `getTableDetails` | Column names, types, and constraints for a specific table. |
| `queryVCData` | Runs a read-only query against the dataset and returns the rows. |

## Connect

### claude.ai (web / desktop)

1. Go to **Settings → Connectors → Add custom connector**.
2. Enter the URL: `https://fundable-mcp.jacob-57a.workers.dev/mcp`
3. Complete the sign-in prompt with your Fundable account.

### Claude Desktop (config file)

Add to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "fundable": {
      "command": "npx",
      "args": ["mcp-remote", "https://fundable-mcp.jacob-57a.workers.dev/mcp"]
    }
  }
}
```

The first connection opens a browser window to sign in with your Fundable account.

### Other MCP clients

Point any client that supports remote Streamable HTTP MCP servers at the endpoint above and
complete the OAuth flow.

## Example prompts

Once connected, just ask in plain language:

- "Show me the 10 most recent Series A deals in fintech."
- "Who led Anthropic's most recent funding round?"
- "Which firms have invested in the most AI companies this year?"
- "Find founders who previously worked at Stripe and now run their own startups."
- "What's the total raised by climate-tech companies headquartered in Europe?"

## About Fundable

Fundable is a venture-capital data platform. Learn more and manage your subscription at
[tryfundable.ai](https://tryfundable.ai).

## Support

Questions or access issues? Reach out via [tryfundable.ai](https://tryfundable.ai).

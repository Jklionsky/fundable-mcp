# Fundable MCP Server

Query [Fundable's](https://tryfundable.ai) proprietary venture-capital dataset in natural
language — funding rounds, investors, valuations, founders, firms, and the relationships
between them — directly from Claude or any MCP-compatible client.

This is a **remote, hosted MCP server**. There's nothing to install or run locally; you connect
to the hosted endpoint and authenticate with your Fundable account.

- **Endpoint:** `https://mcp.tryfundable.ai/`
- **Transport:** Streamable HTTP (the explicit `/mcp` path remains available for compatibility)
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
| `previewData` | Runs a read-only query and returns a free inline preview of up to 15 rows. |
| `exportData` | Exports up to 1,000 rows as a credit-metered CSV after explicit file intent and approval of a credit ceiling. Clerk OAuth only. |
| `unlockPersonEmail` | Unlocks one verified person email after explicit approval. Clerk OAuth only. |
| `unlockPersonEmails` | Unlocks an explicitly approved batch of 1–20 verified person emails with a hard credit ceiling. Clerk OAuth only. |
| `unlockPersonPhone` | Unlocks one verified person phone number after explicit approval. Clerk OAuth only. |
| `unlockPersonPhones` | Unlocks an explicitly approved batch of 1–20 verified person phone numbers with a hard credit ceiling. Clerk OAuth only. |

Authorized API-key sessions receive the dataset and preview tools. Authorized Clerk OAuth
sessions also receive the export and contact-unlock tools. Users without dataset access receive
only `requestAccess`.

## Credit-consuming actions

- **CSV exports:** One credit per row actually exported. The caller must set `maxCredits` from a ceiling the user approved; if the result exceeds it, the export is rejected without a charge. The limit is 1,000 rows. An identical SQL retry within 15 minutes recovers the same export without a second charge.
- **Single email or phone unlock:** A newly unlocked contact costs five credits; a repeat of that same contact type for the same account costs zero. Email and phone are separate unlocks, so unlocking both for one person can cost ten credits. Ask for approval before a paid unlock.
- **Batch email or phone unlock:** Each call covers 1–20 named people and requires approval of the exact people and a `maxCredits` ceiling. Each new contact costs five credits, up to the approved ceiling; repeats, misses, unavailable results, duplicates, and invalid identifiers cost zero. Do not break a batch into single unlocks to bypass its ceiling.

Before confirming any of these actions, show the user what will be exported or whose contact details will be unlocked, the unit cost, and the maximum credits that can be charged. Report the actual credits used afterward.

## Connect

### claude.ai (web / desktop)

1. Go to **Settings → Connectors → Add custom connector**.
2. Enter the URL: `https://mcp.tryfundable.ai/`
3. Complete the sign-in prompt with your Fundable account.

### Claude Desktop (config file)

Add to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "fundable": {
      "command": "npx",
      "args": ["mcp-remote", "https://mcp.tryfundable.ai/"]
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

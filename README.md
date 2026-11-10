# Fundable MCP Server

Query [Fundable's](https://tryfundable.ai) proprietary startup and venture-capital dataset
through the hosted MCP server. Connect with an active Fundable account that has a primary
email. No paid subscription is required, and new signups receive 250 free one-time signup
credits.

This is a **remote, hosted MCP server**. There's nothing to install or run locally; you connect
to the hosted endpoint and authenticate with your Fundable account.

- **Endpoint:** `https://mcp.tryfundable.ai/`
- **Transport:** Streamable HTTP (the explicit `/mcp` path remains available for compatibility)
- **MCP auth:** OAuth 2.1 with your Fundable account. The account must be active and have a
  primary email. No paid subscription is required.
- **API-key auth:** Provisioned API keys connect at `https://mcp.tryfundable.ai/api-mcp` with
  `Authorization: Bearer <key>`. API-key sessions receive dataset and preview tools. Exports
  and contact unlocks require Clerk OAuth.

> This repo contains documentation and the MCP manifest only. The server source is proprietary.

## Dataset coverage

The dataset covers 101,000 startups, 375,000 startup employees, 67,000 investment firms,
36,000 angels, and 49,000 lead partners. Funding records link to source articles, and data
refreshes multiple times daily. [Explore Fundable data](https://www.tryfundable.ai/data).

## Tools

| Tool | What it does |
|------|--------------|
| `getDatasetContext` | Returns the dataset schema, business rules, and query guidelines. |
| `listDatasetTables` | Quick overview of the available tables. |
| `getQueryExamples` | Example queries by category (funding, investors, people, industries, geography, and more). |
| `getTableDetails` | Column names, types, and constraints for a specific table. |
| `previewData` | Runs a read-only query and returns a free inline preview of up to 15 rows. |
| `get_credit_balance` | Shows your current credit balance and purchase options without using credits. Fundable account sign-in required. |
| `exportData` | Exports up to 1,000 rows as a credit-metered CSV after explicit file intent and approval of a credit ceiling. Clerk OAuth only. |
| `unlockPersonEmail` | Unlocks one verified person email after explicit approval. Clerk OAuth only. |
| `unlockPersonEmails` | Unlocks an explicitly approved batch of 1–20 verified person emails with a hard credit ceiling. Clerk OAuth only. |
| `unlockPersonPhone` | Unlocks one verified person phone number after explicit approval. Clerk OAuth only. |
| `unlockPersonPhones` | Unlocks an explicitly approved batch of 1–20 verified person phone numbers with a hard credit ceiling. Clerk OAuth only. |

Clerk OAuth sessions receive dataset, preview, credit-balance, export, and contact-unlock
tools. API-key sessions receive dataset and preview access; paid export and contact-unlock
tools require Clerk OAuth.

## Credit-consuming actions

- **CSV exports:** One credit per row actually exported. The caller must set `maxCredits` from a ceiling the user approved; if the result exceeds it, the export is rejected without a charge. The limit is 1,000 rows. An identical SQL retry within 15 minutes recovers the same export without a second charge.
- **Single email or phone unlock:** A newly unlocked contact costs five credits; a repeat of that same contact type for the same account costs zero. Email and phone are separate unlocks, so unlocking both for one person can cost ten credits. Ask for approval before a paid unlock.
- **Batch email or phone unlock:** Each call covers 1–20 named people and requires approval of the exact people and a `maxCredits` ceiling. Each new contact costs five credits, up to the approved ceiling; repeats, misses, unavailable results, duplicates, and invalid identifiers cost zero. Do not break a batch into single unlocks to bypass its ceiling.

Before confirming any of these actions, show the user what will be exported or whose contact details will be unlocked, the unit cost, and the maximum credits that can be charged. Report the actual credits used afterward.

[Purchase more credits at $0.06 each](https://www.tryfundable.ai/credits).

Pro is $100/month with 2,000 monthly credits, the web app, deal alerts, and integrations. That is $0.05 per included credit; additional purchased credits cost $0.06 each.

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

Fundable is a startup and venture-capital data platform. Learn more or manage your account at
[tryfundable.ai](https://tryfundable.ai).

## Support

Questions or access issues? Reach out via [tryfundable.ai](https://tryfundable.ai).

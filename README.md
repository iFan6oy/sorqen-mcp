# Sorqen MCP

Sportsbook-derived **fair value** over the [Model Context Protocol](https://modelcontextprotocol.io). Sorqen de-vigs quotes from multiple sportsbooks per market, then aggregates them into a versioned, no-vig consensus probability with confidence and provenance, plus history and settlement context.

This repo is the **connector** for the hosted Sorqen MCP server. There is no server to install. Point your MCP client at the remote endpoint and authenticate with a Sorqen API key.

## Connect

Streamable HTTP endpoint:

```
https://sorqen.flashodds.live/mcp
```

Send your API key in the Authorization header:

```
Authorization: Bearer <your_api_key>
```

Anonymous access works for **discovery** (`list_leagues`, `describe_methods`, `describe_plans`, `describe_authentication`) so a client can inspect coverage and method before ever making a data call. Fair-value data requires at least a free key. Get one at **https://sorqen.flashodds.live/keys**.

### Claude Desktop

Add to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "sorqen": {
      "type": "streamable-http",
      "url": "https://sorqen.flashodds.live/mcp",
      "headers": { "Authorization": "Bearer YOUR_API_KEY" }
    }
  }
}
```

## Tools

| Tool | What it returns | Access |
| --- | --- | --- |
| `list_leagues` | Supported league ids, market types, and periods | Anonymous |
| `describe_methods` | The versioned fair-value method and refusal philosophy | Anonymous |
| `describe_plans` | Public plan limits and entitlements | Anonymous |
| `describe_authentication` | The discovery/data boundary and how to attach a key | Anonymous |
| `list_fair_values` | Current no-vig consensus probabilities across supported markets, filterable by league/market/confidence | Free+ |
| `get_fair_value` | One current fair value by canonical outcome id | Free+ |
| `get_fair_value_history` | Historical fair values for one outcome, window bounded by plan | Builder+ |
| `get_settlement_rules` | Settlement-context rules and tie prior for local comparison | Free+ |
| `get_plan` | The caller's own plan, limits, and entitlements | Free+ |

REST and MCP share the same entitlement model, so a key sees the same tier regardless of transport.

## What Sorqen is not

Sorqen is read-only market intelligence: a fair-value number with confidence and provenance, not a sportsbook, a wallet, an execution engine, a betting bot, or a picks service. It does not host or resell prediction-market data.

## Coverage

Leagues: NFL, NBA, MLB, NHL. Markets: full-game moneyline, spread, and total. Every fair value carries a refusal reason instead of a fabricated number when the underlying quotes don't support one.

## Plans

- **Free:** 500 requests/day, 30/min, fair value without evidence, no history
- **Builder:** 20,000 requests/day, evidence included, 7 days history, change feed + webhooks
- **Pro:** 150,000 requests/day, 90 days history, 100 alert rules
- **Desk:** 1,000,000 requests/day, full archive, bulk export, priority streaming
- **Enterprise:** custom volume and SLA

See current pricing at **https://sorqen.flashodds.live/pricing**.

Data is informational only. Sorqen is not a sportsbook and does not accept wagers.

## Links

- Site + pricing: https://sorqen.flashodds.live/
- Docs: https://sorqen.flashodds.live/docs
- OpenAPI: https://sorqen.flashodds.live/openapi.json
- Built by [Flash AI Solutions](https://flashaisolutions.org)

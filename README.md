# Yahoo Finance Quotes and Fundamentals MCP Server

[![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-yahoo-finance-quotes-fundamentals/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-yahoo-finance-quotes-fundamentals) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-yahoo-finance-quotes-fundamentals%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-yahoo-finance-quotes-fundamentals&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-yahoo-finance-quotes-fundamentals)](https://www.npmjs.com/package/@mambalabsdev/mcp-yahoo-finance-quotes-fundamentals) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-yahoo-finance-quotes-fundamentals)](https://www.npmjs.com/package/@mambalabsdev/mcp-yahoo-finance-quotes-fundamentals) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-yahoo-finance-quotes-fundamentals)](https://github.com/mambalabsdev/mcp-yahoo-finance-quotes-fundamentals/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-yahoo-finance-quotes-fundamentals)

An MCP server for the Mamba Labs Yahoo Finance Quotes and Fundamentals actor on Apify. Read Yahoo Finance for your tickers and get a summary row per symbol plus flat statement and price rows.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Give tickers or company names. For each symbol you get one summary row: price, change, volume, day and 52 week range, market cap, valuation ratios, sector, and the latest annual revenue, net income, EBITDA, free cash flow, assets, debt, cash, EPS, and shares. Ask for fundamentals, price_history, or dividends to add flat rows per statement line and period, per price bar, or per dividend.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-yahoo-finance-quotes-fundamentals": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-yahoo-finance-quotes-fundamentals"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `get_yahoo_finance_data` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "What are Apple's market cap, P/E ratio, and last annual revenue?"
- "Compare net income and free cash flow for MSFT, GOOGL, and META over the last 4 years."
- "Give me daily closing prices for NVDA over the last 3 months."

## Tool

`get_yahoo_finance_data`. One tool. Example arguments:

```json
{
  "symbols": [
    "AAPL",
    "MSFT"
  ]
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `symbols` | array | no | Yahoo Finance symbols, up to 500 per run with company_names. Stocks (AAPL), other exchanges (SAP.DE, 7203.T, VOD.L), ETFs (SPY), indexes (^GSPC), crypto (BTC-USD), currencies (EURUSD=X), and futures (GC=F). |
| `company_names` | array | no | Optional. Company names to look up, such as Tesla or Siemens. Each takes the listed equity whose name matches, else the first one Yahoo Finance returns, shown in the symbol column. Use symbols when you know the ticker. |
| `outputs` | array | no | Any of quote, fundamentals, price_history, dividends. Default: quote. quote: one summary row per symbol with price, change, volume, day and 52 week range, valuation ratios, sector, and the latest annual figures. fundamentals: also one row per statement line and period. price_history: one row per bar. dividends: one row per dividend in the history range. |
| `statement_period` | string | no | annual (about 4 years), quarterly (about 5 quarters), or both. Applies to the fundamentals rows. |
| `history_range` | string | no | For price_history and dividends: 1d, 5d, 1mo, 3mo, 6mo, 1y, 2y, 5y, 10y, ytd, or max. |
| `history_interval` | string | no | Bar size for price_history: 1m, 2m, 5m, 15m, 30m, 60m, 90m, 1h, 1d, 5d, 1wk, 1mo, or 3mo. Yahoo serves minute bars for recent days only. |
| `max_charge_usd` | string | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |
| `proxy_mode` | string | no | auto (default): requests go through the Apify proxy pool. residential: every request uses residential addresses. A reliability setting only; it does not change what you pay. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `symbol-report`, one symbol with data: its summary row | $0.0015 | $0.001425 | $0.00135 | $0.001275 |
| `detail-rows`, each block of up to 100 statement, price, or dividend rows on one symbol | $0.003 | $0.00285 | $0.0027 | $0.00255 |
| `apify-actor-start`, per run start | $0.0025 | $0.0025 | $0.0025 | $0.0025 |

An unknown symbol, a name with no match, a refused or partly read symbol, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/yahoo-finance-quotes-fundamentals](https://apify.com/mambalabs/yahoo-finance-quotes-fundamentals)

## License

MIT. See [LICENSE](LICENSE).

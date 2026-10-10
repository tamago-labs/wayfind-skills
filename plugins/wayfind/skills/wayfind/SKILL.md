---
name: wayfind
description: >
  Use Wayfind for tokenized stock data, portfolio risk analysis, and Solana token swaps.
  Provides wallet operations (balances, swaps), market data (tokenized stocks, pre-IPO, market overview),
  and risk intelligence (personalized risk profiles). Use this skill whenever the user mentions
  Wayfind, tokenized stocks, tokenized equities, risk profile, risk analysis, portfolio risk,
  Solana swaps, OKX DEX, market overview, pre-IPO markets, or on-chain stock data.
  Also invoke when the user wants to check balances, swap tokens, or analyze their portfolio.
---

# Wayfind

Wayfind provides the data layer and trading tools for AI agents on Solana tokenized equities. Connect a wallet, get personalized risk intelligence, and trade with context.

## Wayfind platform overview

Wayfind solves the problem of trading tokenized stocks with AI. One company like Tesla can have multiple on-chain representations (TSLAx, TSLAon, rTSLA) with different prices, liquidity, and issuers. Wayfind provides:

- **Real-time data** — Tokenized stock prices powered by CoinMarketCap
- **Risk intelligence** — AI-powered personalized risk analysis using GPT-6 Astra
- **Trading tools** — Swap execution through OKX DEX Router
- **Market coverage** — Tokenized equities, pre-IPO markets, RWA tokens

For full platform details, see [overview.md](references/overview.md).

## Setup

For installation, configuration, and environment variables, see [setup.md](references/setup.md).

Quick start:

```bash
npm install -g @tamago-labs/wayfind-mcp
```

Set your API key from [wayfind.click](https://wayfind.click/) dashboard.

## Tool routing

Wayfind exposes 10 MCP tools. Route by user intent:

### Wallet operations

| User intent | Tool |
|---|---|
| "What's my wallet address?" | `solana_get_wallet_address` |
| "Show my balances" | `solana_get_balance` |
| "How much X for Y?" | `solana_get_swap_quote` |
| "Swap X to Y" | `solana_swap_tokens` |

### Market data

| User intent | Tool |
|---|---|
| "Show all tokenized stocks" | `wayfind_get_tokens` |
| "Get AAPL tokens" | `wayfind_get_tokenized_stocks` |
| "Market overview" | `wayfind_get_market_overview` |
| "Pre-IPO markets" | `wayfind_get_pre_ipo` |

### Risk analysis

| User intent | Tool |
|---|---|
| "Show my risk profile" | `wayfind_get_risk_profile` |
| "Analyze my portfolio risk" | `wayfind_get_risk_profile` |

## Intent-based routing

Route by user intent before running tools.

**Balance check intent** ("check my balance", "show my wallet", "what do I have"):
- Use `solana_get_balance` to get SOL and SPL token balances
- Symbol resolution is automatic for known tokens

**Swap intent** ("swap X to Y", "trade X for Y", "convert X to Y"):
1. Use `solana_get_swap_quote` first to show the user the rate
2. Confirm with the user before proceeding
3. Use `solana_swap_tokens` to execute (this signs and sends the transaction)

**Market data intent** ("show stocks", "market data", "what's trending"):
- Use `wayfind_get_tokens` for all tokenized stocks
- Use `wayfind_get_market_overview` for market stats
- Use `wayfind_get_tokenized_stocks` with a specific ticker

**Risk analysis intent** ("risk profile", "analyze my risk", "portfolio risk"):
- Use `wayfind_get_risk_profile` — returns the risk profile created from the Wayfind dashboard
- The API key determines which user's risk profile is returned

## Wallet auto-creation

When `SOLANA_PRIVATE_KEY` is not set, the MCP server generates a new Solana wallet:

1. Generates a 64-byte secret key using `@solana/kit`
2. Saves to `.wallet.json` in the working directory (gitignored, mode `0600`)
3. Reuses the saved key on subsequent restarts

**Note:** Anyone with access to `.wallet.json` can control the wallet's funds. Do not expose this file.

## Execution rules

1. For swaps, always get a quote first with `solana_get_swap_quote` before executing
2. Confirm swap details with the user before calling `solana_swap_tokens`
3. The risk profile is tied to the API key — use the user's key for their profile
4. Token symbol resolution works automatically for base tokens (SOL, USDC), RWA stocks, and pre-IPO tokens
5. After swaps, verify the result with `solana_get_balance`

## Response format

For all operational responses, return:
1. What was done (action and scope)
2. The result (balances, quotes, risk scores, etc.)
3. What to do next (or confirmation that the task is complete)

Keep output concise. Include tool evidence only when it helps the user understand what happened.

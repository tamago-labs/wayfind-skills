# Wayfind Platform Overview

## What is Wayfind?

Wayfind is the data layer and trading tools for AI agents on Solana tokenized equities. It solves the problem of trading tokenized stocks with AI — one company like Tesla can have multiple on-chain representations (TSLAx, TSLAon, rTSLA) with different prices, liquidity, issuers, and on-chain dynamics.

## How it works

### 1. Build your risk profile
Answer a short AI questionnaire about your goals, time horizon, and risk tolerance. Wayfind uses your answers to understand what risk means for you.

### 2. Get your API key
Your API key links your risk profile to every bot you create. One setup, all your bots trade with your risk context.

### 3. Set up your bot
Import a Grok Bot template, set your API key, and create an agentic wallet. Connect your wallet to discover holdings or use a demo account to explore.

### 4. Schedule rebalancing jobs
Your bot monitors your portfolio and prepares swap quotes through OKX DEX when positions drift. You review and approve each trade before it executes.

## Key features

- **Real-time data** — Tokenized stock prices powered by CoinMarketCap
- **Risk intelligence** — AI-powered personalized risk analysis using GPT-6 Astra
- **Trading tools** — Swap execution through OKX DEX Router
- **Market coverage** — Tokenized equities, pre-IPO markets, RWA tokens
- **Multi-chain** — Solana (v1.0), Ethereum and BNB Chain (coming soon)

## Token types

| Type | Description | Example |
|------|-------------|---------|
| Base tokens | Native and stablecoins | SOL, USDC, USDT, USDG |
| RWA stocks | Tokenized real-world equities | TSLAx (Tesla), SPCXx (SpaceX) |
| Pre-IPO | Pre-IPO market tokens | ANDURil (Anduril) |

## Supported networks

- **Solana** — v1.0 (active)
- **Ethereum** — Coming soon
- **BNB Chain** — Coming soon

## Resources

- [Wayfind dashboard](https://wayfind.click/dashboard)
- [Wayfind MCP on npm](https://www.npmjs.com/package/@tamago-labs/wayfind-mcp)
- [GitHub](https://github.com/tamago-labs/wayfind)

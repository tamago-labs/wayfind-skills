# Wayfind Tools Reference

## Wallet Operations

### solana_get_wallet_address

Get the wallet address derived from the private key.

**Example prompts:**
- "What's my wallet address?"
- "Show my Solana address"

**Returns:** `walletAddress` — Solana public key string

---

### solana_get_balance

Get SOL and SPL token balances with automatic symbol resolution.

**Example prompts:**
- "Show my wallet balances"
- "What tokens do I have?"
- "Check my balance"

**Returns:**
- `solBalance` — SOL balance in SOL
- `solLamports` — SOL balance in lamports
- `tokens` — Array of `{ mint, symbol, amount, decimals }`

**Symbol resolution covers:**
- Base tokens: SOL, USDC, USDT, USDG
- RWA stocks: 243 tokenized equities (TSLAx, SPCXx, etc.)
- Pre-IPO tokens: ANDURIL, etc.

---

### solana_get_swap_quote

Get an OKX DEX swap quote for a token pair.

**Parameters:**
- `fromSymbol` — Token to swap from (e.g., "SOL", "USDC")
- `toSymbol` — Token to swap to (e.g., "USDC", "TSLAx")
- `amount` — Amount in base units (lamports for SOL, smallest unit for tokens)

**Example prompts:**
- "How much USDC for 1 SOL?"
- "Get a swap quote from SOL to TSLAx"

**Returns:** Quote with expected output amount, price impact, and route info

---

### solana_swap_tokens

Swap tokens on Solana via OKX DEX. Gets the swap instruction, signs with the private key, and sends the transaction.

**Parameters:**
- `fromSymbol` — Token to swap from
- `toSymbol` — Token to swap to
- `amount` — Amount in base units
- `slippagePercent` — Optional slippage tolerance (default: 0.5)

**Example prompts:**
- "Swap 0.1 SOL to USDC"
- "Trade 1 SOL for TSLAx"

**Returns:** Transaction signature

**Flow:**
1. Gets swap instruction from OKX DEX API
2. Decodes the base58-encoded transaction
3. Signs with the wallet's private key
4. Sends the signed transaction to Solana

---

## Market Data

### wayfind_get_tokens

List all tokenized stocks with prices and market data.

**Parameters:**
- `chain` — Optional filter by blockchain (e.g., "solana")

**Example prompts:**
- "Show me all tokenized stocks"
- "List all Solana tokenized stocks"

**Returns:** Array of tokenized stocks with price, market cap, volume

---

### wayfind_get_tokenized_stocks

Get all tokenized versions of a stock ticker across blockchains.

**Parameters:**
- `ticker` — Stock ticker symbol (e.g., "AAPL", "TSLA", "GOOG")

**Example prompts:**
- "Get all AAPL tokens"
- "Show TSLA tokenized stocks"

**Returns:** Array of tokenized versions on different chains

---

### wayfind_get_market_overview

Get market statistics including total market cap, 24h volume, gainers, losers, and trending tokens.

**Example prompts:**
- "Show market overview"
- "What's the market doing?"
- "Show trending tokens"

**Returns:** Market stats with top gainers, losers, and trending

---

### wayfind_get_pre_ipo

List pre-IPO markets with mark prices and valuations.

**Example prompts:**
- "Show pre-IPO markets"
- "What pre-IPO tokens are available?"

**Returns:** Array of pre-IPO markets with mark price and valuation

---

## Risk Analysis

### wayfind_get_risk_profile

Get the user's portfolio risk profile created from the Wayfind dashboard.

**Example prompts:**
- "Show my risk profile"
- "What's my risk score?"
- "Analyze my portfolio risk"

**Returns:**
- `score` — Overall risk score
- `label` — Risk label (e.g., "Low", "Medium", "High")
- `factors` — Breakdown by risk factors (concentration, liquidity, etc.)

**Note:** The risk profile is tied to the `WAYFIND_API_KEY`. Use the demo key for a sample profile, or the user's key for their real profile.

---

## Tool routing summary

| User intent | Tool |
|---|---|
| Check wallet address | `solana_get_wallet_address` |
| Check balances | `solana_get_balance` |
| Get swap quote | `solana_get_swap_quote` |
| Execute swap | `solana_swap_tokens` |
| List all stocks | `wayfind_get_tokens` |
| Search by ticker | `wayfind_get_tokenized_stocks` |
| Market overview | `wayfind_get_market_overview` |
| Pre-IPO markets | `wayfind_get_pre_ipo` |
| Risk profile | `wayfind_get_risk_profile` |

## Swap workflow

For token swaps, follow this two-step flow:

1. **Get quote** — Call `solana_get_swap_quote` to show the user the expected output and rate
2. **Confirm** — Show the user the quote details and ask for confirmation
3. **Execute** — Call `solana_swap_tokens` to perform the swap
4. **Verify** — Call `solana_get_balance` to confirm the new balances

## Dependencies

| Package | Purpose |
|---------|---------|
| `@modelcontextprotocol/sdk` | MCP server + StdioServerTransport |
| `@solana/kit` | Solana SDK (RPC, keypairs, transactions) |
| `bs58` | Base58 encoding |
| `zod` | Tool parameter schemas |

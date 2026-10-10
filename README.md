# Wayfind Skills

Agent skills and plugin manifests for [Wayfind](https://wayfind.click) — the data layer and trading tools for AI agents on Solana tokenized equities.

## What's included

- **`plugins/wayfind/skills/wayfind/`** — Main Wayfind skill (SKILL.md + references)
- **`plugins/wayfind/.grok-plugin/`** — Grok Bot plugin manifest
- **`plugins/wayfind/.claude-plugin/`** — Claude Code plugin manifest (future)
- **`plugins/wayfind/.cursor-plugin/`** — Cursor plugin manifest (future)

## Quick start

1. Install the MCP server:

```bash
npm install -g @tamago-labs/wayfind-mcp
```

2. Get your API key from [wayfind.click](https://wayfind.click/) dashboard.

3. Configure for your agent:

### Grok Bot

Install from the xAI plugin marketplace (coming soon) or add manually:

```json
{
  "mcpServers": {
    "wayfind-solana": {
      "command": "npx",
      "args": ["-y", "@tamago-labs/wayfind-mcp"],
      "env": {
        "WAYFIND_API_KEY": "your_api_key_here"
      }
    }
  }
}
```

### Claude Code

```text
/plugin marketplace add tamago-labs/wayfind-skills
/plugin install wayfind@wayfind-skills
/reload-plugins
```

### Cursor

Add `https://github.com/tamago-labs/wayfind-skills` as a plugin source in Cursor settings.

## Available tools

| Tool | Description |
|------|-------------|
| `solana_get_wallet_address` | Get wallet address |
| `solana_get_balance` | SOL + SPL token balances |
| `solana_get_swap_quote` | OKX DEX swap quote |
| `solana_swap_tokens` | Swap tokens (sign + send) |
| `wayfind_get_tokens` | All tokenized stocks |
| `wayfind_get_tokenized_stocks` | Tokenized stocks by ticker |
| `wayfind_get_market_overview` | Market stats |
| `wayfind_get_pre_ipo` | Pre-IPO markets |
| `wayfind_get_risk_profile` | User risk profile |

## License

MIT

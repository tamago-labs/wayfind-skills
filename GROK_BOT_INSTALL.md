# Wayfind for Grok Bot (stdio on the bot computer)

Wayfind runs as a **local stdio MCP** on the Grok Bot computer. Do not use OAuth or a remote HTTPS MCP for this path. The Solana private key must stay on that machine.

## What ships in the plugin

`mcp.json` only references env placeholders. It never contains a real API key or private key.

```json
{
  "mcpServers": {
    "wayfind-solana": {
      "command": "npx",
      "args": ["-y", "@tamago-labs/wayfind-mcp"],
      "env": {
        "WAYFIND_API_KEY": "${WAYFIND_API_KEY}",
        "SOLANA_PRIVATE_KEY": "${SOLANA_PRIVATE_KEY}"
      }
    }
  }
}
```

## First-time setup (bot owner)

1. Create a Grok Bot (or import a Wayfind template).
2. Install the Wayfind plugin from Connect Apps / marketplace, or tell the bot to add the MCP server from this `mcp.json`.
3. Set these secrets on **that bot's computer** (bot chat → secrets / env). Never paste them into chat, the manifest, or a public repo.

| Secret | Required | Notes |
|--------|----------|--------|
| `WAYFIND_API_KEY` | Yes for real risk profile | From [wayfind.click](https://wayfind.click/) dashboard → API Keys |
| `SOLANA_PRIVATE_KEY` | Yes for balances and swaps | Base58-encoded 64-byte Solana secret key. Omit only if you accept an auto-created wallet on the bot computer (not recommended for real funds). |
| `SOLANA_RPC_URL` | No | Defaults to `https://api.mainnet-beta.solana.com`. Set a paid RPC for production. |

4. Restart the MCP / bot so `npx -y @tamago-labs/wayfind-mcp` picks up the env.
5. Ask: "What's my wallet address?" to confirm the key loaded.

## Safety rules the skill must enforce

- Market data (`wayfind_get_*`) can run with only `WAYFIND_API_KEY`.
- Never call `solana_swap_tokens` without an explicit user yes after showing a `solana_get_swap_quote`.
- Never log, print, or commit `SOLANA_PRIVATE_KEY` or `.wallet.json`.
- Treat the bot computer as the wallet's custody boundary: anyone with access to that machine can move funds.

## Marketplace note

Publish the plugin with placeholders only. Each owner supplies their own keys after install. Do not ship a demo private key.

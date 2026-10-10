# Wayfind MCP Setup

## Install

```bash
npm install -g @tamago-labs/wayfind-mcp
```

## Configuration

### Environment variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `SOLANA_PRIVATE_KEY` | No* | Auto-generated | Base58-encoded 64-byte Solana private key |
| `SOLANA_RPC_URL` | No | `https://api.mainnet-beta.solana.com` | Solana RPC endpoint |
| `WAYFIND_API_KEY` | No* | Demo default | Wayfind API key (UserProfile ID) — get from [wayfind.click](https://wayfind.click/) dashboard |
| `WAYFIND_API_URL` | No | Wayfind Lambda URL | Wayfind API endpoint |

*`SOLANA_PRIVATE_KEY` auto-creates a wallet if omitted. `WAYFIND_API_KEY` uses a demo key by default (returns sample risk profile).

### Getting an API key

1. Go to [wayfind.click](https://wayfind.click/) and sign in
2. Navigate to **API Keys** in the dashboard
3. Create a new API key (this is your UserProfile ID)
4. Use it as `WAYFIND_API_KEY`

### Wallet setup

**Option A — Use your own wallet:**

Set `SOLANA_PRIVATE_KEY` to your base58-encoded 64-byte Solana secret key.

**Option B — Auto-create a wallet:**

Omit `SOLANA_PRIVATE_KEY`. The server generates a new wallet and saves it to `.wallet.json` in the working directory. The key is reused on subsequent restarts.

**Warning:** Anyone with access to `.wallet.json` can control the wallet's funds. Do not expose this file.

## Platform-specific setup

### Grok Bot

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

Add the marketplace and install:

```text
/plugin marketplace add tamago-labs/wayfind-skills
/plugin install wayfind@wayfind-skills
/reload-plugins
```

### Cursor

Add `https://github.com/tamago-labs/wayfind-skills` as a plugin source in Cursor settings.

### OpenCode

Add to `~/.config/opencode/opencode.jsonc`:

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "servers": {
      "wayfind-solana": {
        "type": "local",
        "command": ["npx", "-y", "@tamago-labs/wayfind-mcp"],
        "environment": {
          "WAYFIND_API_KEY": "your_api_key_here"
        }
      }
    }
  }
}
```

## Building from source

```bash
git clone https://github.com/tamago-labs/wayfind-mcp.git
cd wayfind-mcp
npm install
npm run build
npm start
```

## Troubleshooting

| Issue | Solution |
|-------|----------|
| No wallet found | Set `SOLANA_PRIVATE_KEY` or let auto-create |
| Demo risk profile | Set `WAYFIND_API_KEY` to your real API key |
| Swap fails | Check SOL balance for gas fees |
| Token symbol unknown | Token may not be in the static data files |

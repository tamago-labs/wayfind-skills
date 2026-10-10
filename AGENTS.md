# Wayfind Agent Skills

Agent skills and plugin manifests for the Wayfind platform.

## Plugin model

The shared plugin payload lives in `plugins/wayfind`.

- **Grok Build / xAI marketplace**: `plugins/wayfind/.grok-plugin/plugin.json` + `plugins/wayfind/.grok-plugin/mcp.json`
- **Claude Code**: `plugins/wayfind/.claude-plugin/plugin.json`
- **Cursor**: `plugins/wayfind/.cursor-plugin/plugin.json`

## Skill model

The plugin ships one Wayfind skill:

- `plugins/wayfind/skills/wayfind/SKILL.md`

`wayfind` is route-first. Intent routing is defined in `SKILL.md`, and execution details are split into action-oriented references.

## Reference loading pattern

1. Read `plugins/wayfind/skills/wayfind/SKILL.md`.
2. Choose the minimum reference set needed for the request.
3. For multi-step requests, load multiple references and compose one response.

| Intent | Reference | Use for |
|---|---|---|
| Platform overview | `references/overview.md` | What Wayfind is, how it works, pricing |
| Setup and configuration | `references/setup.md` | Install MCP, env vars, API key, wallet |
| Tool usage and examples | `references/tools.md` | All 10 tools with examples and routing |

## MCP server

The MCP server is published as [`@tamago-labs/wayfind-mcp`](https://www.npmjs.com/package/@tamago-labs/wayfind-mcp) on npm.

- Transport: stdio (local)
- Runtime: `npx -y @tamago-labs/wayfind-mcp`

### Environment variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `SOLANA_PRIVATE_KEY` | No* | Auto-generated | Base58-encoded Solana private key |
| `SOLANA_RPC_URL` | No | `https://api.mainnet-beta.solana.com` | Solana RPC endpoint |
| `WAYFIND_API_KEY` | No* | Demo default | Wayfind API key from dashboard |
| `WAYFIND_API_URL` | No | Wayfind Lambda URL | Wayfind API endpoint |

*`SOLANA_PRIVATE_KEY` auto-creates a wallet if omitted. `WAYFIND_API_KEY` uses a demo key by default.

## Authoring guidance

When editing this plugin:

- Keep `SKILL.md` focused on routing, preflight, composition, and common operations.
- Keep references organized by information type (overview, setup, tools).
- Keep references action-oriented with reasoning. Explain why, not only what.
- Keep plugin versions aligned across Grok, Claude, and Cursor manifests.
- Bump `version` in plugin manifests when skill content changes.

## References

- [Wayfind](https://wayfind.click)
- [Wayfind MCP on npm](https://www.npmjs.com/package/@tamago-labs/wayfind-mcp)
- [Agent Skills specification](https://agentskills.io/specification)

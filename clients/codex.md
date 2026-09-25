# Popcraft MCP in Codex (app and CLI)

Your agent does the setup itself.

**Paste this into Codex**

> Help me connect the Popcraft MCP at https://popcraft.ai/api/mcp — add it as a streamable HTTP server named popcraft, then walk me through the sign-in.

By hand (Codex CLI):

```
codex mcp add popcraft --url https://popcraft.ai/api/mcp
```

**Approve in the browser** — the first connection opens the Popcraft sign-in; press **Approve** while Codex is waiting and the login completes on its own.

**Ask away**

> use popcraft to generate a 9:16 product shot, preview the credit cost first

## Notes

- The server entry lands in `~/.codex/config.toml` under `[mcp_servers.popcraft]` with `url = "https://popcraft.ai/api/mcp"`; edit there if you need to rename or remove it.
- Codex CLI flags change between releases — if `codex mcp add` rejects `--url`, run `codex mcp add --help` and use the streamable-HTTP form it prints.

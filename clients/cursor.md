# Popcraft MCP in Cursor

One click from the browser, or a config file — your call.

**One-click** — the "Add to Cursor" button on https://popcraft.ai/mcp opens Cursor with Popcraft pre-filled; confirm and you are done.

**Or add the config** — put this in `~/.cursor/mcp.json` (global) or `.cursor/mcp.json` in a project:

```json
{
  "mcpServers": {
    "popcraft": {
      "url": "https://popcraft.ai/api/mcp"
    }
  }
}
```

**Enable and sign in** — open Cursor Settings → MCP, switch **popcraft** on; the first use triggers the browser OAuth sign-in. Press **Allow**.

Then in the agent panel:

> use popcraft to generate a 9:16 product shot, preview the credit cost first

## Notes

- Cursor shows the tool list under the server once it connects; if it stays empty, toggle the server off and on after signing in.
- Give the agent the file path of any reference in the workspace — it uploads with the `popcraft_media_upload` shell recipe.
- Keep generation in the Agent mode (not the inline edit) so the result cards and cost previews are visible.

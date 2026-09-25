# Popcraft MCP in Claude Code

Terminal-native: paste one message and the agent wires itself up.

**Paste this into Claude Code**

> Set up the Popcraft MCP for me: run `claude mcp add --transport http popcraft https://popcraft.ai/api/mcp`, confirm it's registered, then remind me to run /mcp to sign in.

Or by hand, one command:

```
claude mcp add --transport http popcraft https://popcraft.ai/api/mcp
```

**Authenticate** — run `/mcp` → select **popcraft** → **Authenticate**. The browser opens the Popcraft sign-in; press **Allow**.

**Ask away** — every tool is available in the session:

> use popcraft to generate a 9:16 product shot, preview the credit cost first

Try: *"Use Popcraft to generate three 5-second product loops of ./shots/bottle.png with Seedance 2.0 Fast, 1:1, and save them to ./out/."* — Claude Code uploads the file with the `popcraft_media_upload` shell recipe (`curl -T` to the proxy URL, so the bytes never pass through the context window), previews the cost, generates, and downloads the results with `curl`.

## Notes

- Scope: `claude mcp add` registers the server for the current project by default; add `-s user` to make it available everywhere.
- `claude mcp list` shows the connection state; `claude mcp remove popcraft` and re-add if a token ever gets stuck.
- Local files: Claude Code has a shell, so it should always use the `popcraft_media_upload` recipe for videos and large references, and clip or convert out-of-envelope media with ffmpeg before uploading.

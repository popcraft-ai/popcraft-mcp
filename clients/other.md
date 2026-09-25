# Any other MCP client (OpenClaw, Hermes, your own agent)

Popcraft speaks **Streamable HTTP** with **OAuth 2.1** and publishes standard discovery metadata — compliant clients connect from the URL alone:

```
https://popcraft.ai/api/mcp
```

**Sign in once** — the first connection opens the Popcraft sign-in in your browser. Dynamic client registration means there are no client IDs to request; press **Allow** and you are live, with automatic token refresh from then on.

## Building your own client or agent loop

- Discovery: the endpoint advertises its OAuth authorization server and resource metadata; any SDK that implements the MCP authorization spec (Python, TypeScript) handles the flow.
- Treat `popcraft_models_explore` as the source of truth for what a model accepts — reference counts, media envelopes, durations, aspect ratios and rates — and validate before you call `popcraft_generate_*`.
- Always call the generate tool with `get_cost: true` first and surface the number to the human; video is the expensive medium.
- Jobs are asynchronous: keep the `job_id`, call `popcraft_job_display` once when you need the result, never in a tight loop.
- Any result URL can be passed straight back as a reference (`medias[].value`) — that is how upscale, animate and edit chains work.

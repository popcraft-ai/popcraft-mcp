# Popcraft MCP in Claude (web and desktop)

The one-click connector experience.

1. **Copy the endpoint** — `https://popcraft.ai/api/mcp`. One URL is the whole integration: no API key, no SDK, no config beyond a paste.
2. **Add the connector** — Settings → Connectors → **Add custom connector** → name it `Popcraft`, paste the URL. (https://popcraft.ai/mcp has a button that opens the right screen.)
3. **Approve** — click **Connect**; your browser opens the Popcraft sign-in, press **Allow**, and every tool goes live for that account.

Start a fresh chat and brief it like a producer:

> Use the Popcraft MCP to make a 10-second cinematic product teaser — preview the credit cost first.

Claude will browse the catalogue with `popcraft_models_explore`, quote the cost with `get_cost: true`, submit the job and drop the result card into the chat when it lands.

## Notes

- Enable the connector per chat if your workspace asks (the "Search and tools" menu). Team/Enterprise admins can add it for the whole org from the admin connectors page.
- References: attach a file and ask Claude to upload it with `popcraft_media_upload_widget`, or paste a public URL — Claude imports it with `popcraft_media_import_url`.
- Long jobs: the result card refreshes on its own; there is no need to ask Claude to "check again".
- Sign-in expired or the tools disappeared? Remove and re-add the connector — it is a 20-second reconnect, nothing else to configure.

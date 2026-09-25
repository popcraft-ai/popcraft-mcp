# Popcraft MCP in ChatGPT

ChatGPT web and desktop — connectors live behind Developer mode.

1. **Turn on Developer mode** — Settings → Apps & Connectors → Advanced settings → **Developer mode** (a one-time toggle).
2. **Create the connector** — back in Apps & Connectors, choose **Create**, name it `Popcraft`, paste `https://popcraft.ai/api/mcp`.
3. **Sign in and start** — the first use opens the Popcraft sign-in; press **Allow**, then ask ChatGPT for an image, a video or a track.

> Turn this tumbler into a 15-second vertical UGC ad — hook first, end on a call to action.

## Notes

- Add the connector to a chat from the "+" / tools menu when it is not on by default.
- Developer-mode connectors can write (generate, upload) — ChatGPT will ask you to confirm the first write actions; approve them and it remembers per chat.
- Public image URLs are the easiest references in ChatGPT: the agent imports them with `popcraft_media_import_url`.

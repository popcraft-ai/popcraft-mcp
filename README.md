# Popcraft MCP

Generate video, images, music and sound effects from any MCP client — Claude, Claude Code, Cursor, ChatGPT, Codex — with Seedance 2.5, Veo 3.1, Kling 3.0 Omni, Nano Banana Pro, GPT Image 2.5, ElevenLabs and 20+ more models behind one endpoint.

```
https://popcraft.ai/api/mcp
```

OAuth 2.1 sign-in, no API key, token auto-refresh. 100 free credits on sign-up. Docs and one-click setup: https://popcraft.ai/mcp

## Install (30 seconds)

| Client | How |
|---|---|
| Claude (web / desktop) | Settings → Connectors → Add custom connector → name `Popcraft`, URL above → Connect → Allow. [Details](clients/claude.md) |
| Claude Code | `claude mcp add --transport http popcraft https://popcraft.ai/api/mcp`, then `/mcp` → popcraft → Authenticate. [Details](clients/claude-code.md) |
| Cursor | Add to `~/.cursor/mcp.json`: `{"mcpServers":{"popcraft":{"url":"https://popcraft.ai/api/mcp"}}}`, enable it, sign in on first use. [Details](clients/cursor.md) |
| ChatGPT | Settings → Apps & Connectors → Advanced → Developer mode on → Create → name `Popcraft`, URL above → Allow. [Details](clients/chatgpt.md) |
| Codex | Paste: *"Help me connect the Popcraft MCP at https://popcraft.ai/api/mcp — add it as a streamable HTTP server named popcraft, then walk me through the sign-in."* [Details](clients/codex.md) |
| Anything else (OpenClaw, Hermes, …) | Streamable HTTP + OAuth 2.1 with standard discovery metadata and dynamic client registration — the URL alone is enough. [Details](clients/other.md) |

Every client connects the same endpoint; only the "add server" step differs per app. The first tool call opens the Popcraft sign-in in your browser — press **Allow** once and the connection stays live.

## Try it

Paste any of these into a fresh chat once the connector is on:

- *"Use the Popcraft MCP to make a 10-second cinematic product teaser — preview the credit cost first."*
- *"Make me a 15-second vertical UGC ad for my product on Popcraft — hook people in the first second."* (loads the `ugc-ad` skill)
- *"I need a YouTube thumbnail for my video about why cheap mechanical keyboards beat expensive ones — make it pop, using Popcraft."* (loads the `thumbnail-pro` skill)
- *"Shoot a brand image set for the citrus soda — hero can, lifestyle scene, pattern tile — then voice a 15-second tagline."*

## What your agent gets

**Create** — `popcraft_generate_video` (text-to-video, image-to-video, first/last frame, reference-driven, audio-driven), `popcraft_generate_image` (every frontier image model, edits with references), `popcraft_generate_audio` (TTS, sound effects, music), `popcraft_generate_3d`, `popcraft_effects_show`, `popcraft_list_voices`.

**Enhance** — `popcraft_upscale_image` (Topaz 2×/4×, nine modes), `popcraft_upscale_video` (2×/4× with fps control), `popcraft_upgrade_video` (re-render a Seedance 2.5 draft at 1080p), `popcraft_postprocess_3d`.

**Media & references** — `popcraft_media_upload` (signed direct upload; returns shell recipes so a 200 MB reference streams from disk, not through the context window), `popcraft_media_import_url` (any public URL → reference), `popcraft_media_upload_widget`, `popcraft_media_upload_inline`, `popcraft_media_confirm`, `popcraft_show_medias`, `popcraft_show_elements` (reusable characters and styles).

**Jobs & catalog** — `popcraft_models_explore` (capabilities, reference limits, aspect ratios, durations and credit costs per model), `popcraft_job_display` (result cards that refresh themselves — no polling loops), `popcraft_show_generations` (revisit and chain past results), `popcraft_projects`.

**Skills, served live** — `popcraft_get_workflow_instructions` and `popcraft_get_workflow_bundle_file`. Ask for a deliverable rather than an asset and the agent loads the matching production recipe before it generates anything: `ugc-ad` (product intake → creator identity → 4-panel storyboard → realism pass → cost preview → one Seedance clip with native speech and sound → QA) and `thumbnail-pro` (16 concept frameworks → an 11-block house prompt with identity lock for supplied face photos → parallel variants → micro-edits). Model prompt guides ship the same way (`h3-prompting` for Hailuo 3, `gpt-image-25-prompting` for GPT Image 2.5).

**Account** — `popcraft_balance`, `popcraft_show_plans_and_credits`, `popcraft_transactions`.

## Cost preview on every spend

Every generate tool accepts `get_cost: true` and returns the exact credit price without submitting anything. Video and audio models bill per second of output, image models per image — `popcraft_models_explore` carries the rates. As of 2026-09-25 (720p video, per second): Seedance 2.0 Mini 14 · Seedance 2.0 Fast 20 · Seedance 2.0 26 · Seedance 2.5 42 (24 at 480p draft) · Veo 3.1 58 · Veo 3.1 Fast 15 · Kling 3.0 Omni 16 · Wan 3.0 20 · Hailuo 3 20. Images: Nano Banana 2 15 · Nano Banana Pro 20 (35 at 4K) · GPT Image 2.5 11 (Fast 10) · Seedream 4.0 5. Plan discounts apply automatically; Pro and Max include a daily Seedance 2.0 Fast & Mini allowance at no credit cost — see https://popcraft.ai/unlimited-ai-video and https://popcraft.ai/pricing.

## How a good agent run looks

1. `popcraft_models_explore` — pick the model for the job; read its `reference_limits`, `video_input` / `image_input` / `audio_input` envelope and durations.
2. Get references in: `popcraft_media_import_url` for anything with a public URL, otherwise `popcraft_media_upload` and run the returned shell recipe (`curl -T file "<proxy_upload_url>"`). Pre-check each file against the model's envelope — an out-of-envelope reference is rejected, or worse, wastes credits.
3. Bind references in the prompt with the model's tokens (`Image 1`, `Video 1`, `Audio 1` for the Seedance family; `<Picture 1>` for Hailuo 3).
4. `get_cost: true`, show the number, then generate. The result card refreshes itself; do not poll in a loop.
5. Chain: any result URL is a valid `medias[].value` for the next call (upscale it, animate it, edit it).

## Recipes

Each recipe is a complete run — the tool calls, the prompts, the credits it cost, and the outputs.

1. [UGC ad from one product photo](recipes/01-ugc-ad-from-one-photo.md) — the `ugc-ad` skill end to end.
2. [Thumbnail with identity lock](recipes/02-thumbnail-with-identity-lock.md) — `thumbnail-pro` with your own face photo held constant.
3. [Seedance 2.5 single take, 480p draft → 1080p](recipes/03-seedance-2-5-single-take.md) — timestamped shot lists, draft mode, `popcraft_upgrade_video`.
4. [Product image set with an exact wordmark](recipes/04-product-image-set.md) — packshot → lifestyle → background swap → poster → product loop, 190 credits.
5. [Batch product images](recipes/05-batch-product-images.md) — N products × K scenes in one loop, with consistency rules and cost math.

## FAQ

**Does it need a paid plan?** No — the connector works on the free tier (100 credits on sign-up). Some premium models are reserved for paid plans; the agent gets a clear message and can pick an alternative from the catalogue.
**Where do generations go?** Into your Popcraft account: everything made over MCP appears in your web-app history for editing on the Canvas, upscaling or downloading.
**How long does a job take?** Images a minute or two (Midjourney longer); video a few minutes depending on model and length. Jobs are asynchronous — the agent gets a job id instantly and the card updates when the result lands.
**Real people in references?** The Seedance 2.0 family runs a real-person verification on reference media before generating; a reference that fails (typically a recognisable public figure) is rejected before any charge.

## Links

Docs https://popcraft.ai/mcp · Models https://popcraft.ai/models · Pricing https://popcraft.ai/pricing · Unlimited https://popcraft.ai/unlimited-ai-video

MIT licence — see [LICENSE](LICENSE). Model names belong to their owners.

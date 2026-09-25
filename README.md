<p align="center">
  <a href="https://popcraft.ai/mcp"><img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/8bfb213e-f975-408f-9ebd-9b10418e1591/readme-hero.png" alt="Popcraft MCP — generate video, images, music and sound effects from any AI agent. One endpoint, 20+ frontier models, no API key. https://popcraft.ai/api/mcp" width="100%"></a>
</p>

<p align="center">
  <img alt="MCP · Streamable HTTP" src="https://img.shields.io/badge/MCP-Streamable%20HTTP-FF6B42">
  <img alt="Auth · OAuth 2.1" src="https://img.shields.io/badge/Auth-OAuth%202.1%2C%20no%20API%20key-3A3430">
  <img alt="Models · 20+" src="https://img.shields.io/badge/Models-Seedance%20%C2%B7%20Veo%20%C2%B7%20Kling%20%C2%B7%20Nano%20Banana%20%C2%B7%20GPT%20Image-FF9D3D">
  <img alt="License · MIT" src="https://img.shields.io/badge/License-MIT-green">
</p>

<p align="center">
  <a href="https://popcraft.ai/mcp">Docs &amp; one-click setup</a> ·
  <a href="https://popcraft.ai/models">Models</a> ·
  <a href="https://popcraft.ai/pricing">Pricing</a> ·
  <a href="#recipes">Recipes</a>
</p>

```
https://popcraft.ai/api/mcp
```

<p align="center">
  <img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/cf0c3f3e-04c4-49f4-b1c7-9652692eb7c9/readme-made-with-popcraft-mcp.jpg" alt="Packshot, lifestyle scene, campaign poster and product loop — all generated through the connector" width="100%">
</p>
<p align="center"><sub>One product photo in, a whole listing out — packshot, lifestyle scene, campaign poster and product loop, generated through the connector in <a href="recipes/04-product-image-set.md">recipe 04</a>.</sub></p>

---

## Quick start

Every client connects the same endpoint; only the "add server" step differs. The first tool call opens the Popcraft sign-in in your browser — press **Allow** once and the connection stays live with automatic token refresh. New accounts get **100 free credits**.

| Client | Setup | Guide |
|---|---|---|
| **Claude** (web / desktop) | Settings → Connectors → **Add custom connector** → name `Popcraft` → paste the URL → **Connect** → **Allow** | [clients/claude.md](clients/claude.md) |
| **Claude Code** | `claude mcp add --transport http popcraft https://popcraft.ai/api/mcp` then `/mcp` → popcraft → **Authenticate** | [clients/claude-code.md](clients/claude-code.md) |
| **Cursor** | Add to `~/.cursor/mcp.json` (below), enable **popcraft** in MCP settings, sign in on first use | [clients/cursor.md](clients/cursor.md) |
| **ChatGPT** | Settings → Apps & Connectors → Advanced → **Developer mode** on → **Create** → name `Popcraft` → paste the URL | [clients/chatgpt.md](clients/chatgpt.md) |
| **Codex** | Paste: *"Help me connect the Popcraft MCP at https://popcraft.ai/api/mcp — add it as a streamable HTTP server named popcraft, then walk me through the sign-in."* | [clients/codex.md](clients/codex.md) |
| **Anything else** (OpenClaw, Hermes, your own agent) | Streamable HTTP + OAuth 2.1 with discovery metadata and dynamic client registration — the URL alone is enough | [clients/other.md](clients/other.md) |

<details>
<summary>Cursor config snippet</summary>

```json
{
  "mcpServers": {
    "popcraft": {
      "url": "https://popcraft.ai/api/mcp"
    }
  }
}
```
</details>

### Try it

Paste any of these into a fresh chat once the connector is on:

> Use the Popcraft MCP to make a 10-second cinematic product teaser — preview the credit cost first.

> Make me a 15-second vertical UGC ad for my product on Popcraft — hook people in the first second. *(loads the `ugc-ad` skill)*

> I need a YouTube thumbnail for my video about why cheap mechanical keyboards beat expensive ones — make it pop, using Popcraft. *(loads the `thumbnail-pro` skill)*

> Shoot a brand image set for the citrus soda — hero can, lifestyle scene, pattern tile — then voice a 15-second tagline.

---

## What your agent gets

| Group | Tools | What they do |
|---|---|---|
| **Create** | `popcraft_generate_video`<br>`popcraft_generate_image`<br>`popcraft_generate_audio`<br>`popcraft_generate_3d`<br>`popcraft_effects_show`<br>`popcraft_list_voices` | Text-to-video, image-to-video, first/last frame, reference- and audio-driven video<br>Every frontier image model, edits with references<br>TTS, sound effects, music<br>3D generation<br>Curated video effects<br>Playable voice samples |
| **Enhance** | `popcraft_upscale_image`<br>`popcraft_upscale_video`<br>`popcraft_upgrade_video`<br>`popcraft_postprocess_3d` | Topaz 2×/4×, nine modes<br>2×/4× with fps control<br>Re-render a Seedance 2.5 draft at 1080p<br>3D post-processing |
| **Media & references** | `popcraft_media_upload`<br>`popcraft_media_import_url`<br>`popcraft_media_upload_widget`<br>`popcraft_media_upload_inline`<br>`popcraft_media_confirm`<br>`popcraft_show_medias`<br>`popcraft_show_elements` | Signed direct upload — shell recipes, so a 200 MB reference streams from disk, not through the context window<br>Any public URL → reference<br>Interactive file picker<br>Small files without a shell<br>Verify an upload landed<br>Your media library<br>Reusable characters and styles |
| **Jobs & catalog** | `popcraft_models_explore`<br>`popcraft_job_display`<br>`popcraft_show_generations`<br>`popcraft_projects` | Capabilities, reference limits, aspect ratios, durations and credit costs per model<br>Result cards that refresh themselves — no polling loops<br>Revisit and chain past results<br>File results into projects |
| **Skills** | `popcraft_get_workflow_instructions`<br>`popcraft_get_workflow_bundle_file` | Production playbooks served live from the server — see below<br>Per-step reference modules |
| **Account** | `popcraft_balance`<br>`popcraft_show_plans_and_credits`<br>`popcraft_transactions` | Credits and plan before spending<br>Plans and one-click top-up<br>Every charge and refund |

### Skills, served live

Ask for a *deliverable* rather than an asset and the agent loads the matching production recipe before it generates anything. Nothing to install, nothing to paste.

| Skill | What it produces |
|---|---|
| `ugc-ad` | One vertical 9:16 creator-style ad, up to 15 s, with speech and sound rendered natively in the clip.<br><sub>intake → creator identity → script → storyboard → realism pass → cost preview → one Seedance 2.0 clip → QA</sub> |
| `thumbnail-pro` | Click-optimised YouTube / Instagram thumbnails with an identity lock for your own face.<br><sub>16 concept frameworks → one intake question → 11-block prompt → parallel variants → micro-edits → 4K upscale</sub> |
| `h3-product-ad` | Minimalist, typography-led product ad on Hailuo 3, 5–15 s, with a native score.<br><sub>intake → copy → three anchor photos → storyboard → cost preview → one generation → QA</sub> |
| `voiceover` | Narration that does not sound like TTS — voice lock, words-per-second budget, per-line direction.<br><sub>script rewrite → voice pick → line-by-line delivery tags</sub> |
| `sd25-prompting`<br>`sd25-prompting-zh` | ByteDance's official Seedance 2.5 prompt optimizer, served verbatim (English and Chinese editions) — compiles your brief and references into one submission-ready prompt. |
| `gpt-image-25-prompting`<br>`h3-prompting` | Model-specific prompt formats for GPT Image 2.5 (change-vs-preserve structure) and Hailuo 3 (structured format with sound and speech fields). |

---

## Cost preview on every spend

Every generate tool accepts `get_cost: true` and returns the exact credit price without submitting anything. Rates come from `popcraft_models_explore`; the numbers below are as of 2026-09-25 and apply before plan discounts.

| Video model | Credits / second (720p) | Image model | Credits / image |
|---|---:|---|---:|
| Seedance 2.0 Mini | 14 | Seedream 4.0 | 5 |
| Seedance 2.0 Fast | 20 | GPT Image 2.5 Fast | 10 |
| Seedance 2.0 | 26 | GPT Image 2.5 | 11 |
| Seedance 2.5 · 480p draft | 42 · 24 | Nano Banana 2 | 15 |
| Veo 3.1 Fast · Veo 3.1 | 15 · 58 | Nano Banana Pro · 4K | 20 · 35 |
| Kling 3.0 Omni | 16 | | |
| Wan 3.0 · Hailuo 3 | 20 | | |

Pro and Max include a daily Seedance 2.0 Fast & Mini allowance at no credit cost, and Nano Banana / Seedream images are unlimited from Plus upward — see [unlimited-ai-video](https://popcraft.ai/unlimited-ai-video) and [pricing](https://popcraft.ai/pricing).

---

## How a good agent run looks

1. **Pick the model** — `popcraft_models_explore`: read its `reference_limits`, the `video_input` / `image_input` / `audio_input` envelope, durations and aspect ratios.
2. **Get references in** — `popcraft_media_import_url` for anything with a public URL; otherwise `popcraft_media_upload` and run the returned shell recipe (`curl -T file "<proxy_upload_url>"`). Pre-check each file against the envelope — an out-of-envelope reference is rejected, or worse, wastes credits.
3. **Bind references in the prompt** with the model's tokens — `Image 1`, `Video 1`, `Audio 1` for the Seedance family; `<Picture 1>` for Hailuo 3.
4. **Preview, then spend** — `get_cost: true`, show the number, then generate. The result card refreshes itself; do not poll in a loop.
5. **Chain** — any result URL is a valid `medias[].value` for the next call: upscale it, animate it, edit it.

---

## Recipes

Each recipe is a complete run — the tool calls, the prompts and the outputs, shown inline.

| # | Recipe | What you get |
|---|---|---|
| 01 | [UGC ad from one product photo](recipes/01-ugc-ad-from-one-photo.md) | The `ugc-ad` skill end to end: intake → identity → storyboard → one Seedance 2.0 clip with native speech |
| 02 | [Thumbnail with identity lock](recipes/02-thumbnail-with-identity-lock.md) | `thumbnail-pro` with your own face held constant across variants |
| 03 | [Seedance 2.5 single take](recipes/03-seedance-2-5-single-take.md) | Timestamped shot list, 480p draft, then `popcraft_upgrade_video` to 1080p |
| 04 | [Product image set with an exact wordmark](recipes/04-product-image-set.md) | Packshot → lifestyle → background swap → poster → product loop, with the brand's exact wordmark |
| 05 | [Batch product images](recipes/05-batch-product-images.md) | N products × K scenes in one loop, with consistency rules and cost math |

---

## FAQ

<details>
<summary><b>Does it need a paid plan?</b></summary>
No. The connector works on the free tier (100 credits on sign-up). Some premium models are reserved for paid plans; the agent gets a clear message and can pick an alternative from the catalogue.
</details>

<details>
<summary><b>Where do generations go?</b></summary>
Into your Popcraft account. Everything made over MCP appears in your web-app history for editing on the Canvas, upscaling or downloading; <code>popcraft_show_generations</code> lists them inside the chat.
</details>

<details>
<summary><b>How long does a job take?</b></summary>
Images a minute or two (Midjourney longer); video a few minutes depending on model and length. Jobs are asynchronous — the agent gets a job id instantly and the card updates when the result lands.
</details>

<details>
<summary><b>Can I use my own images, video or audio as references?</b></summary>
Yes — via <code>popcraft_media_import_url</code> for public URLs or the <code>popcraft_media_upload</code> shell recipe for local files. Generated results chain directly too.
</details>

<details>
<summary><b>What about real people in references?</b></summary>
The Seedance 2.0 family runs a real-person verification on reference media before generating; a reference that fails (typically a recognisable public figure) is rejected before any charge.
</details>

---

<p align="center">
  <a href="https://popcraft.ai/mcp">popcraft.ai/mcp</a> · <a href="https://popcraft.ai/models">Models</a> · <a href="https://popcraft.ai/pricing">Pricing</a> · <a href="https://popcraft.ai/unlimited-ai-video">Unlimited</a><br>
  <sub>MIT licence — see <a href="LICENSE">LICENSE</a>. Model names belong to their owners.</sub>
</p>

# Recipe 01 — UGC ad from one product photo (the `ugc-ad` skill, end to end)

One vertical 9:16 creator-style ad, up to 15 seconds, with the speech and sound rendered natively in the clip — from a single product photo. This is the recipe the server hands your agent when you ask for "a UGC ad", "a creator ad", "a short vertical ad" or "make an ad for my product on Popcraft". This page shows what actually happens, call by call.

**One prompt starts it:**

> Make me a 15-second vertical UGC ad for my product on Popcraft — hook people in the first second. Here is the product photo.

<table>
  <tr>
    <td align="center"><img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/f8dba7f7-cff0-4437-8203-3b0c0387870b/recipe04-packshot.jpg" width="200" alt="Product intake"><br><sub>1 · Product intake</sub></td>
    <td align="center"><img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/7e5a9aca-fb52-4b3c-be37-28b7949abf36/recipe02-portrait.jpg" width="200" alt="Creator identity"><br><sub>2 · Creator identity</sub></td>
    <td align="center"><img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/f6783e88-8372-4cc7-91da-09f302879e6e/recipe01-tokyo.gif" width="200" alt="Vertical clip with native sound"><br><sub>7 · One vertical clip, native sound</sub></td>
  </tr>
</table>

## What the agent does

**0. Loads the skill** — `popcraft_get_workflow_instructions { workflow: "ugc-ad" }`. The workflow owns the production recipe: model choices, reference discipline, the credit preview and the QA pass. Nothing is generated before this.

**1. Product intake** — your photo goes in with `popcraft_media_upload` (shell recipe: `curl -T product.png "<proxy_upload_url>"`) or `popcraft_media_import_url` for a public URL. The intake also picks the format: a **creator talking on camera** about the product, or a **product-only piece carried by an off-screen voiceover**.

A clean packshot is the best input. If your photo has clutter, the agent can make one first with GPT Image 2.5 — this is the packshot from [recipe 04](04-product-image-set.md), with the brand's exact wordmark:

<img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/f8dba7f7-cff0-4437-8203-3b0c0387870b/recipe04-packshot.jpg" width="240" alt="Packshot">

**2. Creator identity (optional)** — for the on-camera format the agent locks one creator so every frame, and any later variant, shows the same person. It generates a portrait with Nano Banana Pro and re-attaches it as the reference from then on. Real example of a locked identity — portrait, then two more scenes with the portrait attached (same face, same jacket, same earring):

<table>
  <tr>
    <td><img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/7e5a9aca-fb52-4b3c-be37-28b7949abf36/recipe02-portrait.jpg" width="200" alt="Portrait"></td>
    <td><img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/a0cc4371-8e1c-4467-9f4a-871417958b85/recipe02-bike.jpg" width="200" alt="Same person, riding"></td>
    <td><img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/299ad9d9-9eb5-4b5f-ac0f-05a20cb6152b/recipe02-door.jpg" width="200" alt="Same person, at the door"></td>
  </tr>
</table>

Your own face photo works the same way — attach it and the identity lock applies to you. (The Seedance 2.0 family verifies real-person references before generating.)

**3. Script** — hook in the first second, one benefit demonstrated, a call to action; written to a words-per-second budget so the read fits the clip length. For a voiceover format the agent chains the `voiceover` workflow (voice lock, ear-first rewriting, per-line delivery direction).

**4. One 4-panel storyboard sheet** — a single image with the four beats (hook / product in hand / benefit / CTA), so you approve the shape of the ad before the expensive step.

**5. Realism cleanup pass** — the storyboard frames are checked and corrected for the things that read as "AI" (hands, product geometry, label text) before they become references for the clip.

**6. Cost preview** — `popcraft_generate_video { model: "seedance-2-0", aspect_ratio: "9:16", duration: 15, resolution: "720p", get_cost: true, medias: [...] }`. The agent shows you the exact number before anything is spent (Seedance 2.0 bills per second of output; Pro and Max also include a daily Seedance 2.0 Fast & Mini allowance at no credit cost) and waits for a yes.

**7. One Seedance 2.0 generation** — references bound in the prompt as `Image 1` (creator) / `Image 2` (product) / `Image 3` (storyboard), speech and ambient sound rendered natively in the clip: no separate TTS, no mixing. What a single vertical Seedance 2.0 take looks like — one continuous handheld shot, natural street sound, the turn-to-camera landing at about 1.2 s:

<img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/f6783e88-8372-4cc7-91da-09f302879e6e/recipe01-tokyo.gif" width="270" alt="Vertical Seedance 2.0 Fast clip">

[▶ MP4, 720p with audio](https://cdn.popcraft.ai/videogen/d2a0eae0-f22a-441c-998c-ba3b0ed1b12f/handheld-phone-camera-shot-a-woman.mp4) · Seedance 2.0 Fast, 9:16, 5 s ·

> Handheld phone-camera shot, a woman in a yellow raincoat crossing a rain-soaked Tokyo street at night, neon signs reflected in the puddles, she turns toward the camera and laughs, one continuous take, natural street sound, no subtitles, no background music.

**8. Frozen-frame QA** — the agent pulls frames from the result, checks the product, the hands and the on-screen text, and either passes it or re-rolls the one beat that failed.

**9. Hosted URL** — the ad lands as an `https://cdn.popcraft.ai/videogen/...` URL in the result card, in your Popcraft history, and in your folder if the client has a shell (`curl -o ad.mp4 <url>`).

## Variants that cost one more call

"New hook" re-uses every reference and regenerates the clip; "16:9 version" is a second generation with `aspect_ratio: "16:9"`; an A/B pair is `count: 2` on the same prompt.

## Run it

Steps 1, 2 and 7 above show real outputs from this account. The finished ad is yours to make — paste the prompt at the top of this page into any connected client with your product photo and say yes to the cost preview.

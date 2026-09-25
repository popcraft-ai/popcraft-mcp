# Recipe 01 — UGC ad from one product photo (the `ugc-ad` skill, end to end)

Goal: one vertical 9:16 creator-style ad, up to 15 seconds, with the speech and sound rendered natively in the clip — from a single product photo. This is the recipe the server hands your agent when you ask for "a UGC ad", "a creator ad", "a short vertical ad" or "make an ad for my product on Popcraft"; the point of this page is to show what actually happens, call by call.

**One prompt starts it:**

> Make me a 15-second vertical UGC ad for my product on Popcraft — hook people in the first second. Here is the product photo.

## What the agent does

**0. Loads the skill** — `popcraft_get_workflow_instructions { workflow: "ugc-ad" }`. The workflow owns the production recipe: model choices, reference discipline, the credit preview and the QA pass. Nothing is generated before this.

**1. Product intake** — your photo goes in with `popcraft_media_upload` (shell recipe: `curl -T product.png "<proxy_upload_url>"`) or `popcraft_media_import_url` for a public URL. The intake also picks the format: a **creator talking on camera** about the product, or a **product-only piece carried by an off-screen voiceover**.

A clean packshot is the best input. If your photo has clutter, the agent can make one first with GPT Image 2.5 (this is the packshot we use in [recipe 04](04-product-image-set.md) — 15 credits):
https://cdn.popcraft.ai/imagegen/9e3f56a3-ee2f-44c7-b3e3-a979db9cf5a0/product-packshot-for-a-catalog.png

**2. Creator identity (optional)** — for the on-camera format the agent locks one creator so every frame, and any later variant, shows the same person. It generates a portrait with Nano Banana Pro and re-attaches it as the reference from then on. Real example of a locked identity from our batch (portrait, then two more scenes with the portrait attached — same face, same jacket, same earring):
https://cdn.popcraft.ai/imagegen/ed5174ab-e907-4955-8c34-de99b1c6e384/character-portrait-for-a-consistent.png ·
https://cdn.popcraft.ai/imagegen/bbfafc23-cc2f-4356-b9ad-36bd4e141823/same-woman-as-the-attached.png ·
https://cdn.popcraft.ai/imagegen/a29ce97e-b493-4bf1-9aee-18adf67d3fe3/same-woman-as-the-attached.png
(Your own face photo works the same way — attach it and the identity lock applies to you. The Seedance 2.0 family verifies real-person references before generating.)

**3. Script** — hook in the first second, one benefit demonstrated, a call to action; written to a words-per-second budget so the read fits the clip length. For a voiceover format the agent chains the `voiceover` workflow (voice lock, ear-first rewriting, per-line delivery direction).

**4. One 4-panel storyboard sheet** — a single image with the four beats (hook / product in hand / benefit / CTA), so you approve the shape of the ad before the expensive step.

**5. Realism cleanup pass** — the storyboard frames are checked and corrected for the things that read as "AI" (hands, product geometry, label text) before they become references for the clip.

**6. Cost preview** — `popcraft_generate_video { model: "seedance-2-0", aspect_ratio: "9:16", duration: 15, resolution: "720p", get_cost: true, medias: [...] }`. Seedance 2.0 bills 26 credits per second at 720p at list rate: a 15-second ad previews at 390 credits, a 10-second one at 260 (Pro/Max Seedance discounts apply automatically; on those plans Seedance 2.0 Fast and Mini also have a daily no-credit allowance). The agent shows you the number and waits.

**7. One Seedance 2.0 generation** — references bound in the prompt as `Image 1` (creator) / `Image 2` (product) / `Image 3` (storyboard), speech and ambient sound rendered natively in the clip: no separate TTS, no mixing.

**8. Frozen-frame QA** — the agent pulls frames from the result, checks the product, the hands and the on-screen text, and either passes it or re-rolls the one beat that failed.

**9. Hosted URL** — the ad lands as an `https://cdn.popcraft.ai/videogen/...` URL in the result card, in your Popcraft history, and in the folder if your client has a shell (`curl -o ad.mp4 <url>`).

## Status

Steps 1–2 above link to real outputs from our 2026-09-25 batch. The final ad render is the one step that costs real money (≈ 390 credits at list), so run it on your own product — paste the prompt at the top of this page into any connected client and say yes to the cost preview.

## Variants that cost one more call

"New hook" re-uses every reference and regenerates the clip (another 390); "16:9 version" is a second generation with `aspect_ratio: "16:9"`; an A/B pair is `count: 2` on the same prompt.

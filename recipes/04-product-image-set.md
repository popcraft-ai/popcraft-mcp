# Recipe 04 — Product image set with an exact wordmark (packshot → lifestyle → background swap → poster → product loop)

Goal: one product, one wordmark, five deliverables an e-commerce listing needs — with the brand's real letterforms on every asset. Run on 2026-09-25; every output below is the actual result. Total: **190 credits** (≈ US$1.90 at list rates).

**Why the wordmark is a reference, not a word in the prompt.** Ask an image model to "print the Popcraft logo on the bottle" and it invents a typeface. Attach the wordmark PNG as a reference with a stated role and it reproduces the letterforms. GPT Image 2.5 is the model for exact in-frame text and reference fidelity, so it makes the packshot; Nano Banana Pro then carries that packshot through the scenes.

## 0. Get the wordmark in

```
popcraft_media_upload { filename: "popcraft_wordmark_4x1_white.png" }
# run the returned recipe, e.g.  curl -T popcraft_wordmark_4x1_white.png "<proxy_upload_url>"
```
→ media URL `https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/b52aaebc-5c73-4d5a-9317-ff059574aa30/popcraft_wordmark_4x1_white.png`

(No file on disk? `popcraft_media_import_url` takes any public URL.)

## 1. Packshot with the exact wordmark — GPT Image 2.5, 1:1, 2K — 15 credits

`popcraft_generate_image { model: "gpt-image-2-5", aspect_ratio: "1:1", resolution: "2K", medias: [{ role: "reference", value: <wordmark> }] }`

> Product packshot for a catalog. Subject: a matte-black insulated water bottle, standing upright and centered on a seamless pure white background, soft studio light from the top-left, a subtle shadow under the base, photoreal.
>
> Reference image 1 is a wordmark on a white background that reads "Popcraft" in a serif typeface. Reproduce that wordmark exactly — same letterforms, same spelling "Popcraft" — as a small, crisp, white silk-screen print on the bottle, horizontally centered on the lower third of the body, about one quarter of the bottle's width. Do not add any other text, icon or logo. Keep everything else clean: no props, no reflections of other objects.

→ https://cdn.popcraft.ai/imagegen/9e3f56a3-ee2f-44c7-b3e3-a979db9cf5a0/product-packshot-for-a-catalog.png

Check at 100% zoom before going further: the wordmark is the first thing to drift on a reference-driven product shot. Here it came back in the real DM Serif letterforms on the first try.

## 2. Lifestyle scene — Nano Banana Pro, 4:5, 2K — 20 credits

`popcraft_generate_image { model: "nano-banana-pro", aspect_ratio: "4:5", resolution: "2K", medias: [{ role: "reference", value: <packshot> }] }`

> Place the attached matte-black water bottle on a sunlit wooden café table, morning light from the left, a blurred street with passers-by behind, editorial lifestyle product photography, keep the bottle's shape, finish and the white "Popcraft" wordmark exactly as in the reference — same letterforms, same size and position on the bottle.

→ https://cdn.popcraft.ai/imagegen/584c491d-7332-434c-b4dc-6212b4422dcf/place-the-attached-matte-black-water.png

## 3. Background swap — same call, 1:1 — 20 credits

> Keep the attached product exactly as it is — same bottle, same matte finish, same white "Popcraft" wordmark with the same letterforms, same angle and lighting on the object — and replace only the background with a white marble kitchen counter and soft morning window light, with a natural contact shadow under the base.

→ https://cdn.popcraft.ai/imagegen/122a38bd-9d66-4f84-871c-e65499452658/keep-the-attached-product-exactly.png

## 4. Campaign poster with a logo lock-up — Nano Banana Pro, 4:5, 4K — 35 credits

Two references this time: the packshot and the wordmark PNG.

`popcraft_generate_image { model: "nano-banana-pro", aspect_ratio: "4:5", resolution: "4K", medias: [{ role: "reference", value: <packshot> }, { role: "reference", value: <wordmark> }] }`

> Campaign poster, portrait format. Hero image: a surfer walking out of the sea at golden hour, board under one arm, wet sand reflecting the sky. Headline "Made for Mornings" set top-left in a bold grotesk sans-serif, white. Bottom-right: the attached matte-black water bottle rendered small at about 8% of the poster width as the product lock-up, and directly beneath it the word "Popcraft" in the same serif letterforms as the wordmark printed on the bottle, white. No other text. Clean, minimal, premium outdoor-brand poster, sharp readable type.

→ https://cdn.popcraft.ai/imagegen/13ec8762-97a5-4add-a4aa-c784ce133f14/campaign-poster-portrait-format-hero.png

The lock-up came back in the real letterforms — the wordmark reference did the work; without it the model invents a typeface.

## 5. Product loop for the listing — Seedance 2.0 Fast, 1:1, 5 s, 720p — 100 credits

`popcraft_generate_video { model: "seedance-2-0-fast", aspect_ratio: "1:1", duration: 5, resolution: "720p", medias: [{ role: "image", value: <packshot> }] }` (Seedance 2.0 Mini does the same job for 70; preview with `get_cost: true` first)

> Image 1 is the product. Slow 180-degree orbit around the matte-black water bottle on a seamless white studio background, soft studio light, shallow depth of field, the white "Popcraft" wordmark stays sharp and readable as it passes the camera, the bottle never leaves the frame, seamless loop feel, no text overlays, no music.

→ https://cdn.popcraft.ai/videogen/84c23f8a-930d-4ba0-8154-74784ef89d5b/image-1-is-the-product.mp4 (poster frame: https://cdn.popcraft.ai/thumbnails/1790315561309-ccaee837-2bc-vid-thumb-1790315561308-yzfs0b0wq1.webp)

Seedance's real-person verification ran on the reference and passed (it is a bottle). Note: with "the wordmark stays sharp and readable" in the prompt the model favours a gentle ~40° turn over a full 180° orbit — drop that clause if the label may pass out of view.

## The rules that made the difference

- Give every reference a role in the prompt ("Reference image 1 is a wordmark…", "Image 1 is the product") and separate what must **change** from what must **stay**.
- Make the packshot once with the text-accurate model, then reuse it as the reference for every scene — consistency comes from the reference, not from repeating the description.
- Total for the set: 15 + 20 + 20 + 35 + 100 = **190 credits** (160 with the Mini loop). Nano Banana Pro images are unlimited on Plus, Pro and Max, which takes this to 15 + 100 on those plans.

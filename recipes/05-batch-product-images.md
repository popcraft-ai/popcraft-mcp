# Recipe 05 — Batch product images: N products × K scenes in one loop

Goal: a catalogue's worth of consistent scene images from packshots, run by an agent with a shell (Claude Code, Codex, Cursor) so uploads stream from disk and results land in a folder. Recipe 04 is one product through this pipeline; this recipe is the loop.

## Inputs

- `products/<sku>.png` — one packshot per product on white (make them with GPT Image 2.5 if you only have photos with clutter: see recipe 04, step 1).
- `brand/wordmark.png` — attached wherever the wordmark must render.
- A scene list, e.g. `scenes.json`:

```json
[
  { "id": "cafe",    "ar": "4:5", "prompt": "on a sunlit wooden café table, morning light from the left, blurred street behind, editorial lifestyle product photography" },
  { "id": "marble",  "ar": "1:1", "prompt": "on a white marble kitchen counter, soft morning window light, natural contact shadow under the base" },
  { "id": "outdoor", "ar": "4:5", "prompt": "on a flat rock beside a mountain trail at golden hour, shallow depth of field, premium outdoor-brand catalogue" }
]
```

## The loop (what the agent runs)

1. Upload each packshot once: `popcraft_media_upload { filename }` → run the returned `curl -T` recipe → keep the media URL per SKU. (Public URLs: `popcraft_media_import_url` instead.)
2. Pre-check the model once: `popcraft_models_explore { action: "get", model_id: "nano-banana-pro" }` — reference limit, aspect ratios, resolutions.
3. Preview the bill: `popcraft_generate_image { model: "nano-banana-pro", get_cost: true, ... }` for one call, multiply by N × K, show the number, get a yes.
4. For each product × scene:

   `popcraft_generate_image { model: "nano-banana-pro", aspect_ratio: <scene.ar>, resolution: "2K", medias: [{ role: "reference", value: <packshot url> }], project_name: "Catalogue Q4" }`

   > Place the attached product <scene.prompt>. Keep the product's shape, finish, colour and the printed wordmark exactly as in the reference — same letterforms, same size and position. No other text, no extra props touching the product.

5. Collect result URLs from each job card (or `popcraft_show_generations` afterwards), download with `curl -o out/<sku>_<scene>.png <url>`, and write a manifest row: sku, scene, model, credits, url, prompt.

Concurrency: the plan's concurrent-job limit applies (Pro 8, Max 10) — submit in batches of that size rather than one at a time.

## Consistency rules that survive a 100-image batch

- **Reference, don't describe.** The packshot in every call is what keeps the product identical; the prompt only describes the scene and repeats the preserve-clause.
- **Separate change from preserve** in every prompt — the sentence starting "Keep …" is not optional.
- **One model for the whole batch.** Mixing image models mid-batch changes the rendering of the same product.
- **Fixed camera language per scene** ("three-quarter view, eye level, 50mm look") so a scene reads the same across SKUs.
- **QA the wordmark at 100%** on a 5% sample before downloading the rest; regenerate only the misses (a re-roll with the same reference costs one image).

## Cost math

Nano Banana Pro at 2K: 20 credits/image on Free and Starter; **unlimited on Plus, Pro and Max**. 50 SKUs × 3 scenes = 150 images = 3,000 credits at list, or 0 credits of your allowance on Plus+. GPT Image 2.5 (11 credits at 1K) when exact text must render inside the scene; Nano Banana 2 (15) or Seedream 4.0 (5) for drafts you will not ship.

## Worked example (one SKU, real outputs)

Packshot → café → marble → poster → 5-second product loop: https://cdn.popcraft.ai/imagegen/9e3f56a3-ee2f-44c7-b3e3-a979db9cf5a0/product-packshot-for-a-catalog.png · https://cdn.popcraft.ai/imagegen/584c491d-7332-434c-b4dc-6212b4422dcf/place-the-attached-matte-black-water.png · https://cdn.popcraft.ai/imagegen/122a38bd-9d66-4f84-871c-e65499452658/keep-the-attached-product-exactly.png · https://cdn.popcraft.ai/imagegen/13ec8762-97a5-4add-a4aa-c784ce133f14/campaign-poster-portrait-format-hero.png · https://cdn.popcraft.ai/videogen/84c23f8a-930d-4ba0-8154-74784ef89d5b/image-1-is-the-product.mp4 — prompts and credits in [recipe 04](04-product-image-set.md).

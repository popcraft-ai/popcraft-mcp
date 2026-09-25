# Recipe 04 — Product image set with an exact wordmark

One product, one wordmark, five deliverables an e-commerce listing needs — with the brand's real letterforms on every asset. Every image below is the actual output of the calls shown; nothing was retouched.

<table>
  <tr>
    <td align="center"><img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/f8dba7f7-cff0-4437-8203-3b0c0387870b/recipe04-packshot.jpg" width="220" alt="Packshot"><br><sub>1 · Packshot</sub></td>
    <td align="center"><img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/cb7eb4e6-eaf6-4c6e-bcc4-d2e8e48b8862/recipe04-cafe.jpg" width="220" alt="Lifestyle scene"><br><sub>2 · Lifestyle scene</sub></td>
    <td align="center"><img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/8d039326-88a0-4f43-b22c-fa73fa5bec24/recipe04-marble.jpg" width="220" alt="Background swap"><br><sub>3 · Background swap</sub></td>
    <td align="center"><img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/c28f576f-2af9-40c9-b7bd-492bb77f84f4/recipe04-poster.jpg" width="220" alt="Campaign poster"><br><sub>4 · Campaign poster</sub></td>
    <td align="center"><img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/e64fdae8-35de-4d07-8678-07f99caea4ee/recipe04-orbit.gif" width="220" alt="Product loop"><br><sub>5 · Product loop</sub></td>
  </tr>
</table>

**Why the wordmark is a reference, not a word in the prompt.** Ask an image model to "print the Popcraft logo on the bottle" and it invents a typeface. Attach the wordmark PNG as a reference with a stated role and it reproduces the letterforms. GPT Image 2.5 is the model for exact in-frame text and reference fidelity, so it makes the packshot; Nano Banana Pro then carries that packshot through the scenes.

## 0. Get the wordmark in

```
popcraft_media_upload { filename: "popcraft_wordmark_4x1_white.png", content_type: "image/png" }
# run the recipe it returns, e.g.  curl -T popcraft_wordmark_4x1_white.png "<proxy_upload_url>"
```

<img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/b52aaebc-5c73-4d5a-9317-ff059574aa30/popcraft_wordmark_4x1_white.png" width="240" alt="The wordmark reference">

The media URL it returns is the reference for steps 1 and 4. (Already online? `popcraft_media_import_url` takes any public URL.)

## 1. Packshot with the exact wordmark — GPT Image 2.5

```
popcraft_generate_image {
  model: "gpt-image-2-5", aspect_ratio: "1:1", resolution: "2K",
  medias: [{ role: "reference", value: <wordmark> }]
}
```

> Product packshot for a catalog. Subject: a matte-black insulated water bottle, standing upright and centered on a seamless pure white background, soft studio light from the top-left, a subtle shadow under the base, photoreal.
>
> Reference image 1 is a wordmark on a white background that reads "Popcraft" in a serif typeface. Reproduce that wordmark exactly — same letterforms, same spelling "Popcraft" — as a small, crisp, white silk-screen print on the bottle, horizontally centered on the lower third of the body, about one quarter of the bottle's width. Do not add any other text, icon or logo. Keep everything else clean: no props, no reflections of other objects.

<img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/f8dba7f7-cff0-4437-8203-3b0c0387870b/recipe04-packshot.jpg" width="360" alt="Packshot result">

[Full size](https://cdn.popcraft.ai/imagegen/9e3f56a3-ee2f-44c7-b3e3-a979db9cf5a0/product-packshot-for-a-catalog.png) · 15 credits · first attempt

Check the wordmark at 100% zoom before going further — on a reference-driven product shot it is the first thing to drift. Here it came back in the real DM Serif letterforms.

## 2. Lifestyle scene — Nano Banana Pro

```
popcraft_generate_image {
  model: "nano-banana-pro", aspect_ratio: "4:5", resolution: "2K",
  medias: [{ role: "reference", value: <packshot> }]
}
```

> Place the attached matte-black water bottle on a sunlit wooden café table, morning light from the left, a blurred street with passers-by behind, editorial lifestyle product photography, keep the bottle's shape, finish and the white "Popcraft" wordmark exactly as in the reference — same letterforms, same size and position on the bottle.

<img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/cb7eb4e6-eaf6-4c6e-bcc4-d2e8e48b8862/recipe04-cafe.jpg" width="360" alt="Lifestyle scene result">

[Full size](https://cdn.popcraft.ai/imagegen/584c491d-7332-434c-b4dc-6212b4422dcf/place-the-attached-matte-black-water.png) · 20 credits (unlimited on Plus, Pro and Max)

## 3. Background swap — same call, 1:1

> Keep the attached product exactly as it is — same bottle, same matte finish, same white "Popcraft" wordmark with the same letterforms, same angle and lighting on the object — and replace only the background with a white marble kitchen counter and soft morning window light, with a natural contact shadow under the base.

<img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/8d039326-88a0-4f43-b22c-fa73fa5bec24/recipe04-marble.jpg" width="360" alt="Background swap result">

[Full size](https://cdn.popcraft.ai/imagegen/122a38bd-9d66-4f84-871c-e65499452658/keep-the-attached-product-exactly.png) · 20 credits

## 4. Campaign poster with a logo lock-up — Nano Banana Pro, 4K

Two references this time: the packshot and the wordmark PNG.

```
popcraft_generate_image {
  model: "nano-banana-pro", aspect_ratio: "4:5", resolution: "4K",
  medias: [{ role: "reference", value: <packshot> }, { role: "reference", value: <wordmark> }]
}
```

> Campaign poster, portrait format. Hero image: a surfer walking out of the sea at golden hour, board under one arm, wet sand reflecting the sky. Headline "Made for Mornings" set top-left in a bold grotesk sans-serif, white. Bottom-right: the attached matte-black water bottle rendered small at about 8% of the poster width as the product lock-up, and directly beneath it the word "Popcraft" in the same serif letterforms as the wordmark printed on the bottle, white. No other text. Clean, minimal, premium outdoor-brand poster, sharp readable type.

<img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/c28f576f-2af9-40c9-b7bd-492bb77f84f4/recipe04-poster.jpg" width="360" alt="Campaign poster result">

[Full size, 4K](https://cdn.popcraft.ai/imagegen/13ec8762-97a5-4add-a4aa-c784ce133f14/campaign-poster-portrait-format-hero.png) · 35 credits at 4K

The lock-up came back in the real letterforms — the wordmark reference did the work; without it the model invents a typeface.

## 5. Product loop for the listing — Seedance 2.0 Fast

```
popcraft_generate_video {
  model: "seedance-2-0-fast", aspect_ratio: "1:1", duration: 5, resolution: "720p",
  medias: [{ role: "image", value: <packshot> }],
  get_cost: true   // preview first, then drop this line to submit
}
```

> Image 1 is the product. Slow 180-degree orbit around the matte-black water bottle on a seamless white studio background, soft studio light, shallow depth of field, the white "Popcraft" wordmark stays sharp and readable as it passes the camera, the bottle never leaves the frame, seamless loop feel, no text overlays, no music.

<img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/e64fdae8-35de-4d07-8678-07f99caea4ee/recipe04-orbit.gif" width="360" alt="Product loop preview">

[▶ MP4, 720p](https://cdn.popcraft.ai/videogen/84c23f8a-930d-4ba0-8154-74784ef89d5b/image-1-is-the-product.mp4) · 5 s · Seedance 2.0 Fast is 20 credits per second at 720p (Pro and Max include a daily Seedance 2.0 Fast & Mini allowance at no credit cost; Mini does the same job for 14 per second)

Seedance's real-person verification ran on the reference and passed (it is a bottle). With "the wordmark stays sharp and readable" in the prompt the model favours a gentle turn over a full 180° orbit — drop that clause if the label may pass out of view.

## The rules that made the difference

- Give every reference a role in the prompt ("Reference image 1 is a wordmark…", "Image 1 is the product") and separate what must **change** from what must **stay**.
- Make the packshot once with the text-accurate model, then reuse it as the reference for every scene — consistency comes from the reference, not from repeating the description.
- Preview video with `get_cost: true`; images on Nano Banana Pro are unlimited on Plus, Pro and Max, so iterate on the scenes freely and spend the care on the packshot.

# Recipe 01 — UGC ad from one product photo (the `ugc-ad` skill, end to end)

One vertical 9:16 creator-style ad, up to 15 seconds, with the speech and sound rendered natively in the clip — from a single product photo. This is the recipe the server hands your agent when you ask for "a UGC ad", "a creator ad", "a short vertical ad" or "make an ad for my product on Popcraft". Below is one real run, call by call, with every output as it came back.

**One prompt starts it:**

> Make me a 15-second vertical UGC ad for my product on Popcraft — hook people in the first second. Here is the product photo.

<table>
  <tr>
    <td align="center"><img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/f8dba7f7-cff0-4437-8203-3b0c0387870b/recipe04-packshot.jpg" width="190" alt="The product photo"><br><sub>The product photo in</sub></td>
    <td align="center"><img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/7e5a9aca-fb52-4b3c-be37-28b7949abf36/recipe02-portrait.jpg" width="190" alt="The locked creator"><br><sub>The creator, locked</sub></td>
    <td align="center"><img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/a8f00d6a-a0e8-4211-ace0-ed01a2a78a32/recipe01-storyboard.jpg" width="190" alt="Four-panel storyboard"><br><sub>The storyboard</sub></td>
    <td align="center"><img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/c187d112-5b5a-4a95-8ea9-ee1b25a2204d/recipe01-ad.gif" width="190" alt="The finished ad"><br><sub>The finished ad — 15 s, native speech</sub></td>
  </tr>
</table>

## What the agent does

**0. Loads the skill** — `popcraft_get_workflow_instructions { workflow: "ugc-ad" }`. The workflow owns the production recipe: model choices, reference discipline, the credit preview and the QA pass. Nothing is generated before this.

**1. Product intake** — your photo goes in with `popcraft_media_upload` (shell recipe: `curl -T product.png "<proxy_upload_url>"`) or `popcraft_media_import_url` for a public URL. The intake also picks the format: a **creator talking on camera** about the product, or a **product-only piece carried by an off-screen voiceover**. This run: creator on camera.

A clean packshot is the best input. If your photo has clutter, the agent can make one first with GPT Image 2.5 — this is the packshot from [recipe 04](04-product-image-set.md), with the brand's exact wordmark:

<img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/f8dba7f7-cff0-4437-8203-3b0c0387870b/recipe04-packshot.jpg" width="220" alt="Packshot">

**2. Creator identity** — for the on-camera format the agent locks one creator so every frame, and any later variant, shows the same person. It generates a portrait with Nano Banana Pro and attaches it as a reference to every call from here on. Our creator is a bicycle courier (she fits the product story):

<img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/7e5a9aca-fb52-4b3c-be37-28b7949abf36/recipe02-portrait.jpg" width="220" alt="Creator portrait">

> Character portrait for a consistent character sheet: a red-haired bicycle courier in her late twenties, freckles, a small silver hoop earring in the left ear, wearing a forest-green windbreaker with a reflective chest strap, standing on a city rooftop at golden hour, looking slightly off-camera with a relaxed half-smile, waist-up, photoreal, 85mm lens look, soft background blur.

Your own face photo works the same way — attach it and the identity lock applies to you. (The Seedance 2.0 family verifies real-person references before generating.)

**3. Script** — hook in the first second, one benefit shown, a call to action, written to a words-per-second budget so the read fits 15 seconds (30 words here, about two a second):

| Beat | Line | Action |
|---|---|---|
| 0–3 s | "Eight hours on the bike in this heat—" | holds the bottle up to the camera, slightly out of breath |
| 3–7 s | "and there's still ice in here." | shakes the bottle by her ear, ice rattles |
| 7–11 s | "Matte grip, never sweats, so my bag stays dry." | turns the bottle in the light, pats the courier bag |
| 11–15 s | "The Popcraft bottle. Link's below." | bottle raised beside her face, points down |

**4. One 4-panel storyboard sheet** — a single Nano Banana Pro image with the four beats, portrait and packshot attached as references, so you approve the shape of the ad before the expensive step:

<img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/a8f00d6a-a0e8-4211-ace0-ed01a2a78a32/recipe01-storyboard.jpg" width="520" alt="Four-panel storyboard sheet">

`popcraft_generate_image { model: "nano-banana-pro", aspect_ratio: "1:1", resolution: "2K", medias: [{ role: "reference", value: <portrait> }, { role: "reference", value: <packshot> }] }`

> A four-panel storyboard sheet for a 15-second vertical UGC video ad, laid out as a clean 2×2 grid of 9:16 phone-camera frames on a white background, each panel with a small grey label in the corner: "1 · 0–3 s", "2 · 3–7 s", "3 · 7–11 s", "4 · 11–15 s". The presenter in every panel is exactly the woman in reference image 1 — same face, freckles, red hair, silver hoop earring, forest-green windbreaker with reflective chest strap. The product in every panel is exactly the bottle in reference image 2 — matte-black insulated water bottle with the small white "Popcraft" wordmark on the lower body, same letterforms. Panel 1: sunlit city street, midday, she is slightly out of breath, bike behind her, holding the bottle up to the phone camera at arm's length, mouth open mid-sentence. Panel 2: close-up, she shakes the bottle next to her ear and grins. Panel 3: her hand gripping the bottle, matte finish in sharp focus, the outside of the bottle dry, her courier bag visible and dry beside it. Panel 4: she smiles straight into the camera, bottle raised beside her face, other hand pointing down toward where a link would be. Photoreal, handheld phone-camera look, natural daylight, consistent lighting across panels, no text other than the four corner labels, no logos other than the wordmark on the bottle.

**5. Realism pass and the first frame** — the agent checks the board for the things that read as "AI" (hands, product geometry, label text) and then renders the opening frame as a proper 9:16 still, with the same two references. That still becomes the clip's `start_image`, which is what keeps the product and the face steady from the first frame:

<img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/4c9494a2-4224-424e-8090-7328a6c2b214/recipe01-first-frame.jpg" width="270" alt="First frame">

> First frame of a vertical 9:16 UGC phone video, photoreal. The woman from reference image 1 — identical face, freckles, red hair tied back, silver hoop earring in the left ear, forest-green windbreaker with a reflective chest strap — stands on a sunlit city street at midday with her bicycle just behind her, slightly out of breath, looking straight into the phone camera about to speak. In her right hand, held up toward the camera at chest height, is exactly the product from reference image 2: a matte-black insulated water bottle with the small white "Popcraft" wordmark on the lower body, same letterforms, same size and position, label facing the camera, sharp. Handheld selfie-camera framing from just above eye level, waist-up, natural daylight, soft shadows, shallow depth of field on the street behind. Bottle proportions and finish exactly as in the reference; five natural fingers; no text, no captions, no other logos.

**6. Cost preview** — `popcraft_generate_video { model: "seedance-2-0", aspect_ratio: "9:16", duration: 15, resolution: "720p", get_cost: true, medias: [...] }` → the exact number, shown to you before anything is spent (Seedance 2.0 bills per second of output; Pro and Max also include a daily Seedance 2.0 Fast & Mini allowance at no credit cost). You say yes.

**7. One Seedance 2.0 generation** — the first frame as `start_image`, the portrait and the packshot as `image` references bound in the prompt as `Image 1` and `Image 2`, the script inside the prompt with its timings. Speech, the ice rattle and the street are rendered natively in the clip: no separate TTS, no mixing.

```
popcraft_generate_video {
  model: "seedance-2-0", aspect_ratio: "9:16", duration: 15, resolution: "720p",
  medias: [
    { role: "start_image", value: <first frame> },
    { role: "image", value: <portrait> },
    { role: "image", value: <packshot> }
  ]
}
```

> Vertical 9:16 UGC creator video, one continuous handheld selfie-camera take, photoreal, starting exactly on the provided first frame. The presenter is the woman in Image 1 — same face, freckles, red hair, silver hoop earring, forest-green windbreaker with reflective strap — and stays identical for the whole clip. The product is exactly the bottle in Image 2: matte-black insulated water bottle with a small white "Popcraft" wordmark on the lower body; keep its shape, finish and wordmark unchanged whenever it is in frame.
> 0-3s: on a sunlit city street with her bicycle behind her, slightly out of breath, she holds the bottle up toward the camera and says, a little breathless and amused: "Eight hours on the bike in this heat—"
> 3-7s: she lifts the bottle next to her ear and shakes it, we hear ice rattle inside, she grins and says: "and there's still ice in here."
> 7-11s: she turns the bottle in her hand so the matte surface catches the light, taps the dry outside of it, pats the courier bag on her hip and says: "Matte grip, never sweats, so my bag stays dry."
> 11-15s: she looks straight into the lens, raises the bottle beside her face with the wordmark facing the camera, points down with her other hand and says with a warm smile: "The Popcraft bottle. Link's below."
> Natural midday daylight, gentle handheld sway, her voice recorded on the phone mic with light street ambience, the ice rattle audible at 3-7s. No background music, no subtitles, no captions, no on-screen text, no extra logos.

<img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/c187d112-5b5a-4a95-8ea9-ee1b25a2204d/recipe01-ad.gif" width="270" alt="The finished ad, preview">

[▶ Watch the ad — MP4, 720×1280, 15 s, with sound](https://cdn.popcraft.ai/videogen/b055dac7-7249-464f-93db-ce10deef69e2/vertical-9-16-ugc-creator-video.mp4) · rendered in about 6 minutes

**8. Frozen-frame QA** — the agent pulls frames from the result and checks the product, the hands and any text. This run: the same courier in every frame, the bottle's shape and finish unchanged, the wordmark reads "Popcraft" at 1 s, 12 s and 14 s, all four beats land inside their windows, no captions or music appeared. One nit a client cut would fix with a re-roll: a faint garment-brand mark on the windbreaker's chest.

**9. Hosted URL** — the ad lands as an `https://cdn.popcraft.ai/videogen/...` URL in the result card, in your Popcraft history, and in your folder if the client has a shell (`curl -o ad.mp4 <url>`).

## Variants that cost one more call

"New hook" re-uses every reference and regenerates the clip; "16:9 version" is a second generation with `aspect_ratio: "16:9"`; an A/B pair is `count: 2` on the same prompt.

## Run it on your product

Paste the prompt at the top of this page into any connected client with your own product photo (and your face photo, if you want to be the creator). The agent loads the skill, walks these steps with you, previews the cost and stops for your yes before the render.

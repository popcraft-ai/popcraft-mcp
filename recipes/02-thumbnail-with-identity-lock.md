# Recipe 02 — YouTube thumbnail with an identity lock (the `thumbnail-pro` skill)

A click-optimised thumbnail (or Instagram cover) that keeps **your** face — not a model's idea of a face — across every variant, sized for your own title overlay. Ask for "a thumbnail", "a YouTube thumbnail", "cover image" or "video preview image" and the server loads this workflow before anything is generated.

**One prompt starts it:**

> I need a YouTube thumbnail for my video about why cheap mechanical keyboards beat expensive ones — make it pop, using Popcraft. Here's my face photo.

## The identity lock, demonstrated

This is the mechanism the workflow relies on — a portrait first, then new scenes generated with the portrait attached and a preserve-clause in the prompt. Same face, same freckles, same earring, same jacket, three different scenes:

<table>
  <tr>
    <td align="center"><img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/7e5a9aca-fb52-4b3c-be37-28b7949abf36/recipe02-portrait.jpg" width="260" alt="Portrait"><br><sub>Portrait — text prompt only</sub></td>
    <td align="center"><img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/a0cc4371-8e1c-4467-9f4a-871417958b85/recipe02-bike.jpg" width="260" alt="Riding through traffic"><br><sub>Portrait attached as reference</sub></td>
    <td align="center"><img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/299ad9d9-9eb5-4b5f-ac0f-05a20cb6152b/recipe02-door.jpg" width="260" alt="Handing over a parcel"><br><sub>Portrait attached as reference</sub></td>
  </tr>
</table>

Portrait (Nano Banana Pro, 4:5, 1K):

> Character portrait for a consistent character sheet: a red-haired bicycle courier in her late twenties, freckles, a small silver hoop earring in the left ear, wearing a forest-green windbreaker with a reflective chest strap, standing on a city rooftop at golden hour, looking slightly off-camera with a relaxed half-smile, waist-up, photoreal, 85mm lens look, soft background blur.

Second and third images, with the portrait in `medias` as `{ role: "reference" }`:

> Same woman as the attached reference — identical face, freckles, red hair, silver hoop earring, forest-green windbreaker with reflective chest strap — now riding a bicycle through city traffic at midday, mid-pedal, looking ahead, motion blur on the cars behind her, photoreal, 50mm lens look.

Consistency comes from the reference, not from repeating the description. Your own face photo works the same way: attach it and the lock applies to you.

## What the agent does

**0. Loads the skill** — `popcraft_get_workflow_instructions { workflow: "thumbnail-pro" }`.

**1. Concept brainstorm across 16 named frameworks** — contrast, the "impossible object", the reaction shot, the before/after split, the giant prop, and so on — and proposes the three that fit the video's promise.

**2. One bundled intake question** — everything it needs in a single reply from you: which concept, your face photo (if any), the channel's palette, whether text is baked in or you overlay your own title.

**3. Face photo in** — `popcraft_media_upload` or `popcraft_media_import_url`. From here the identity lock holds: the photo rides along as a reference on every generation and every edit, with a preserve-clause for the face in the prompt.

**4. The 11-block house prompt** — subject, expression, pose, prop, background, lighting, palette, composition, text zone, style, negative — assembled from the brief. Locked model routing: **GPT Image 2.5** for design-led briefs and any text baked into the image; **Nano Banana Pro at 2K** for photoreal people.

**5. Parallel variants** — `popcraft_generate_image { count: 2 }` or two prompts in flight — with the same reference, so the variants differ in concept, not in who is in them.

**6. Surgical micro-edits** — re-attach the chosen variant and change one thing ("swap the keyboard for a red one, keep everything else identical"); the edit stays local.

**7. Optional 4K upscale** — `popcraft_upscale_image` (Topaz 2×/4×) for print or a channel banner.

## Text in the frame

When the title is baked into the image, the workflow routes to GPT Image 2.5 because it renders exact text. The same property, shown on packaging with Nano Banana Pro — every character correct on the first try, including the em dash and the comma:

<img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/d307f3b0-c9ce-4079-a56c-ad355c29ec2a/recipe02-bag.jpg" width="320" alt="Kraft coffee bag with exact printed text">

> A kraft-paper coffee bag standing upright, front view, with the words "ALTITUDE ROAST — 1,900 m" printed in a condensed sans-serif in dark brown ink, a small line of text "Single origin · Washed · Medium" beneath it, soft studio light from the left, shallow depth of field, warm neutral background, photoreal product photography.

## Cost

Images are the cheap part of Popcraft: Nano Banana Pro is 20 credits an image and unlimited on Plus, Pro and Max; GPT Image 2.5 is 11 at 1K. The workflow spends its care on the brief, not the bill.

## Run it

The identity-lock chain above is real output; the keyboard thumbnail itself is yours to make — paste the prompt at the top of this page with your photo and pick a concept when the agent asks.

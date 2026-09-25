# Recipe 02 — YouTube thumbnail with an identity lock (the `thumbnail-pro` skill)

Goal: a click-optimised thumbnail (or Instagram cover) that keeps **your** face — not a model's idea of a face — across every variant, sized for your own title overlay. Ask for "a thumbnail", "a YouTube thumbnail", "cover image" or "video preview image" and the server loads this workflow before anything is generated.

**One prompt starts it:**

> I need a YouTube thumbnail for my video about why cheap mechanical keyboards beat expensive ones — make it pop, using Popcraft. Here's my face photo.

## What the agent does

**0. Loads the skill** — `popcraft_get_workflow_instructions { workflow: "thumbnail-pro" }`.

**1. Concept brainstorm across 16 named frameworks** — contrast, the "impossible object", the reaction shot, the before/after split, the giant prop, and so on — and proposes the three that fit the video's promise.

**2. One bundled intake question** — everything it needs in a single reply from you: which concept, your face photo (if any), the channel's palette, whether text is baked in or you overlay your own title.

**3. Face photo in** — `popcraft_media_upload` / `popcraft_media_import_url`. From here the identity lock holds: the photo is attached as a reference on every generation and every edit, and the prompt carries a preserve-clause for the face.

**4. The 11-block house prompt** — subject, expression, pose, prop, background, lighting, palette, composition, text zone, style, negative — assembled from the brief. Locked model routing: **GPT Image 2.5** for design-led briefs and any text baked into the image; **Nano Banana Pro at 2K** for photoreal people.

**5. Parallel variants** — `popcraft_generate_image { count: 2 }` or two prompts in flight — with the same reference, so the variants differ in concept, not in who is in them.

**6. Surgical micro-edits** — re-attach the chosen variant and change one thing ("swap the keyboard for a red one, keep everything else identical"); the edit stays local.

**7. Optional 4K upscale** — `popcraft_upscale_image` (Topaz 2×/4×) for print or a channel banner.

## The identity lock, demonstrated

The same mechanism, from our 2026-09-25 batch — portrait first, then two new scenes generated with the portrait attached and a preserve-clause in the prompt:

- Portrait (Nano Banana Pro, text only): https://cdn.popcraft.ai/imagegen/ed5174ab-e907-4955-8c34-de99b1c6e384/character-portrait-for-a-consistent.png
- Same person, riding through traffic (portrait attached): https://cdn.popcraft.ai/imagegen/bbfafc23-cc2f-4356-b9ad-36bd4e141823/same-woman-as-the-attached.png
- Same person, handing over a parcel (portrait attached): https://cdn.popcraft.ai/imagegen/a29ce97e-b493-4bf1-9aee-18adf67d3fe3/same-woman-as-the-attached.png

Prompt shape that held: *"Same woman as the attached reference — identical face, freckles, red hair, silver hoop earring, forest-green windbreaker — now [new scene], photoreal, 50mm lens look."* Consistency comes from the reference, not from repeating the description.

## Cost

Nano Banana Pro 20 credits per image at 2K (unlimited on Plus, Pro and Max); GPT Image 2.5 11 at 1K. A typical run — two variants, two micro-edits — is 60–80 credits on Free/Starter and 0 on Plus+. Preview with `get_cost: true`; images are cheap enough that the workflow spends its care on the brief, not the bill.

## Status

The identity-lock chain above is real output; the keyboard thumbnail itself is yours to run — paste the prompt at the top of this page with your photo and pick a concept when it asks.

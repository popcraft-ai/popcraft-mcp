# Recipe 03 — Seedance 2.5 single take: timestamped shot list, 480p draft, then 1080p

Goal: direct a multi-beat shot as one generation, test it cheaply, then re-render the same clip at full resolution without re-rolling. Seedance 2.5 takes whole-second timestamps in the prompt, up to 30 seconds in one job, and up to 10 video + 10 audio references — the model for anything longer than a social hook.

## 1. Draft at 480p — 4 s, 16:9 — 96 credits

`popcraft_generate_video { model: "seedance-2-5", draft: true, aspect_ratio: "16:9", duration: 4, count: 1 }`

Draft mode renders a fast 480p preview at the 480p rate (24 credits/s instead of 42) and keeps a draft id for 7 days.

> 0-2s: a lighthouse keeper in a heavy oilskin coat pushes open a thick wooden door into a storm, wind tearing at the door, rain driving sideways. 2-4s: cut to the lighthouse beam sweeping across black waves below the cliff. Cinematic, cold blue palette, wind and surf audio only, no music, no subtitles.

→ https://cdn.popcraft.ai/videogen/900a526c-43a6-4d0d-bd95-f588d44d7e79/0-2s-a-lighthouse-keeper-in.mp4 · 854×480, 4.06 s, generated in about a minute (poster frame: https://cdn.popcraft.ai/thumbnails/1790314281820-b6223988-2bc-vid-thumb-1790314281820-pju3190un4.webp)

Both beats landed on their timestamps; the cut at 2 s is a real cut, not a dissolve.

## 2. Promote the draft to 1080p

`popcraft_upgrade_video { generation_id: <id of the draft job> }`

Re-renders the **same** clip at 1080p, billed at the 1080p rate — no re-roll, no drift from the version you approved. Cheaper than rendering at 1080p directly whenever you would reject more than about one in five first tries.

## 3. The longer form — one take, 15–30 s

`popcraft_generate_video { model: "seedance-2-5", aspect_ratio: "16:9", duration: 30, resolution: "720p", get_cost: true }` → 1,260 credits at list (42/s); Pro takes 40% off, Max 60% (annual rates). Preview, then drop `get_cost`.

Prompt pattern that holds over 30 seconds: one beat per 3–5 seconds, written as `start-end s:` lines; name the camera move in each; say "no subtitles, no background music" unless you want them (those are the two things a model adds unasked); ask for audio by type ("wind and surf", "engine and rain") rather than "sound".

Beyond a casual one-liner, load `popcraft_get_workflow_instructions { workflow: "sd25-prompting" }` first: it is ByteDance's official Seedance 2.5 prompt optimizer served verbatim, and it dictates the parameter pairings for edit and extend tasks (`aspect_ratio: "adaptive"`, `duration: -1`) that otherwise fail upstream after the queue wait. Chinese briefs: `sd25-prompting-zh`.

## References

Seedance 2.5 accepts images, video (≤ 15 s per file and combined) and audio (≤ 15 s combined) — check the envelope with `popcraft_models_explore { action: "get", model_id: "seedance-2-5" }` before attaching, bind them in the prompt as `Image 1` / `Video 1` / `Audio 1`, and set `task_intent` ("generation" / "extend" / "edit") whenever a video reference is attached.

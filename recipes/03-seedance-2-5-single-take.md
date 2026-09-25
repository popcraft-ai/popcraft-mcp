# Recipe 03 — Seedance 2.5 single take: timestamped shot list, 480p draft, then 1080p

Direct a multi-beat shot as one generation, test it cheaply, then re-render the same clip at full resolution without re-rolling. Seedance 2.5 takes whole-second timestamps in the prompt, up to 30 seconds in one job, and up to 10 video + 10 audio references — the model for anything longer than a social hook.

<img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/fd3d58de-135f-4335-990c-1ba621b5fa18/recipe03-lighthouse.gif" width="480" alt="Lighthouse keeper, two beats in one take">

<sub>Two beats, one generation, 480p draft — the prompt below, unedited. [▶ MP4](https://cdn.popcraft.ai/videogen/900a526c-43a6-4d0d-bd95-f588d44d7e79/0-2s-a-lighthouse-keeper-in.mp4)</sub>

## 1. Draft at 480p — 4 s, 16:9

```
popcraft_generate_video {
  model: "seedance-2-5", draft: true,
  aspect_ratio: "16:9", duration: 4, count: 1
}
```

Draft mode renders a fast 480p preview at the 480p rate (24 credits per second instead of 42) and keeps a draft id for 7 days.

> 0-2s: a lighthouse keeper in a heavy oilskin coat pushes open a thick wooden door into a storm, wind tearing at the door, rain driving sideways. 2-4s: cut to the lighthouse beam sweeping across black waves below the cliff. Cinematic, cold blue palette, wind and surf audio only, no music, no subtitles.

<img src="https://cdn.popcraft.ai/mcp-uploads/69981e960cde3209e9fb0e37/a5a4a947-6ba4-4fb4-90b8-33361724de21/recipe03-lighthouse-frame.jpg" width="480" alt="Frame from the draft">

Result: 854×480, 4.06 s, back in about a minute. Both beats landed on their timestamps; the cut at 2 s is a real cut, not a dissolve.

## 2. Promote the draft to 1080p

```
popcraft_upgrade_video { generation_id: <id of the draft job> }
```

Re-renders the **same** clip at 1080p, billed at the 1080p rate — no re-roll, no drift from the version you approved. Cheaper than rendering at 1080p directly whenever you would reject more than about one in five first tries.

## 3. The longer form — one take, 15–30 s

```
popcraft_generate_video {
  model: "seedance-2-5", aspect_ratio: "16:9", duration: 30, resolution: "720p",
  get_cost: true
}
```

Preview the number, then drop `get_cost` to submit. Seedance 2.5 bills 42 credits per second at 720p at list rate; Pro takes 40% off and Max 60% (annual rates).

**Prompt pattern that holds over 30 seconds**

- One beat per 3–5 seconds, written as `start-end s:` lines.
- Name the camera move in each beat.
- Ask for audio by type ("wind and surf", "engine and rain") rather than "sound".
- Say "no subtitles, no background music" unless you want them — those are the two things a model adds unasked.

Beyond a casual one-liner, load `popcraft_get_workflow_instructions { workflow: "sd25-prompting" }` first: it is ByteDance's official Seedance 2.5 prompt optimizer served verbatim, and it dictates the parameter pairings for edit and extend tasks (`aspect_ratio: "adaptive"`, `duration: -1`) that otherwise fail upstream after the queue wait. Chinese briefs: `sd25-prompting-zh`.

## References

Seedance 2.5 accepts images, video (≤ 15 s per file and combined) and audio (≤ 15 s combined). Check the envelope with `popcraft_models_explore { action: "get", model_id: "seedance-2-5" }` before attaching, bind references in the prompt as `Image 1` / `Video 1` / `Audio 1`, and set `task_intent` (`generation` / `extend` / `edit`) whenever a video reference is attached.

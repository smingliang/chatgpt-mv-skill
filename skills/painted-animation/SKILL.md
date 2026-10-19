---
name: painted-animation
description: Create code-rendered hand-painted watercolor animations, music videos and synchronized lyric videos with p5.js, p5.brush, Chrome and ffmpeg. Use for this procedural animation style, rather than generic video editing or image-model video generation.
---

# Painted animation for Codex / GPT-6.1

Create a complete animation project using the bundled renderer, character kit and examples. Every frame is a pure function of time. User instructions take precedence over this skill's artistic defaults. The skill does not select or change the model; use the user's selected GPT-6.1 model.

## Environment and tools

Use the available shell/file tools to edit and execute code, and an image-viewing tool to inspect rendered previews. Resolve this skill's directory from its actual SKILL.md location; never assume a Claude installation path. Requires Node.js/npm, Chrome or Chromium, ffmpeg, and Python 3 with numpy for tempo measurement. Inspect availability before execution. If execution tools or dependencies are unavailable, deliver the project and precise commands, clearly stating which steps remain unverified. Do not claim a video was rendered without an actual output.

This is procedural animation; no image/video generation API or model API key is required. Preserve the bundled engine unless the task needs a change. Clawd is an optional inherited example character, not a requirement or an OpenAI mascot.

## Workflow

1. Clarify only missing information that materially affects the result. Reuse details already supplied. For music videos obtain audio; for lyric videos obtain an LRC or user-provided timed lyrics. Instrumental videos do not require lyrics. Never invent missing lyrics or claim estimated word timing is measured alignment. Ask about creative direction in one concise combined question only if needed; otherwise state a reasonable interpretation and proceed.
2. Scaffold with `bash "<skill-dir>/scripts/new_project.sh" "<project-dir>"` (`--keep-demo` retains the demo). On Windows use an available Bash environment or copy `template/`, comment out the demo script in studio.html, and run `npm install` in the project. Read the copied `ANIMATION_GUIDE.md` and inspect `docs/emotions.jpg` and `docs/views.jpg`. For music or longer videos read [references/music-video.md](references/music-video.md). Measure tempo with `python3 "<skill-dir>/scripts/beat_grid.py" song.mp3 --lrc=song.lrc`; inspect its help for clipping options.
3. Write `STORYBOARD.md`: logline, world, palette arc, motif, character arc, and timed shots with viewer reads and transitions. Share it at a useful review point; proceed when existing instructions authorize implementation. Use [examples/xiaozhen/](examples/xiaozhen/) for scene and storyboard examples.
4. Set duration, bpm, offset and audio in `src/config.js`. Build one IIFE-wrapped scene file per shot group, register it with `shots(...)`, and include it in studio.html. Block poses before motion. Shared characters, palette and props should remain consistent. Default to working sequentially; use agents only when authorized by the user or host instructions.
5. Render small previews and inspect the actual images. Cover every shot with a sheet, key motions and transitions with strips, and important expressions with crops. Check readable events, character scale, action timing, contact, transitions and lyric placement. Fix observed defects and review again.

```bash
node render.mjs --sheet=0.1,0.8,1.6,2.4 --cols=4 --w=480 --out=out/check/a.jpg
node render.mjs --strip=2.1:2.6 --out=out/check/strip.jpg
node render.mjs --sheet=2.3 --crop=760,420,500,400 --w=500 --out=out/check/face.jpg
```

6. Render the final video in the environment already selected or authorized by the user. Honor an explicit local/cloud choice; never silently move user assets or jobs to another environment. If location is undecided and materially affects time, cost or access, ask once after previews, explaining the measured estimate. Reuse existing authorization; do not demand repeat confirmation. Do not start a paid external job without authorization. For local handoff give commands appropriate to the known OS and include previews.

```bash
node render.mjs --clip --out=out/video.mp4
# Longer videos: render frames, then encode (run sequentially).
node render.mjs --frames --workers=4
node render.mjs --encode --out=out/video.mp4 --audio=assets/song.mp3
```

Omit `--audio` for silent output. For separate `--encode`, pass audio explicitly: the encoder does not read PROJECT.audio. Use the same fps for frame rendering and encoding. Existing frames are reused; when scenes or settings change, invalidate affected frames or use a fresh output project before rendering. Report output, duration, measured performance and any remaining limitations; link actual deliverables.

## Artistic defaults

Use watercolor/ink tools (`paint`, `inkLine`, `ribbon`, `glow`) and flat drawn views. Default to visual storytelling without captions except karaoke and occasional SFX; honor requested text or styles. Give every shot an event and reaction, sufficient viewer read time, expressive motion and a purposeful transition. Keep one world, palette arc and emotional thread across the piece.

## Engine gotchas

- **Pure functions of `t`.** Frames render in parallel and out of order: no state across frames, no `Math.random()`, no frame-by-frame physics. `hash(i)` for stable randomness, `jit()` for boil.
- **`boilSeed(key)`** before each separate background element/prop, or a moving thing makes everything drawn after it re-boil every frame (jitter).
- **Don't name globals after p5's.** p5 defines `line`, `OVERLAY`, `text`, `color`, `scale`, … as globals; a top-level `function line()` fails with `Cannot redefine property` and the page never becomes ready (the renderer times out). Keep helpers inside the scene IIFE or give them distinct names.
- **Layering in doorways, windows and screens:** draw what's inside, then paint the surrounding wall/frame back over it, so nothing wider than the opening shows outside it (the train's `inDoor` + door jambs, the TV bezel pieces).
- A `NaN` in a point list throws `Failed to construct 'OffscreenCanvas'` — guard degenerate geometry.
- Under camera zoom ≳ 2, huge shapes lose outlines; outline weight scales with zoom, so scale `sw` down in close-ups.
- Cost = number of fills and strokes. Aim for ≤ ~1.5–2.5 s/frame (the log prints ms/frame). Prefer fewer, bigger shapes. On machines without a real GPU, watercolour fills are slow — consider fewer `fill` shapes, or `--soft-gl` as a last resort.
- macOS: the renderer uses `--use-angle=metal` and finds `/Applications/Google Chrome.app` automatically; otherwise pass `--chrome=<path>`.

## Adaptation references

Use `src/karaoke.js` and `window.overlayHook` for lyric overlays. Its line-based fill timing is approximate unless supplied with finer alignment. For loops register `LOOPS.name = t => {...}` and `LOOPS.name.len`, then use `node render.mjs --loop=name --png` and encode with ffmpeg. Consult the bundled guide for the complete drawing and character API.

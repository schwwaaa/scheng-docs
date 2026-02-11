# Video Scrub + Keyboard Transport

**Example crate:** `shadecore-example-video-scrub-keyboard-transport`

---

## What This Example Is

A video playback instrument with manual transport control.

It demonstrates:
- File-based video decode
- External time ownership
- Keyboard-driven transport (pause, scrub)

---

## Signal Flow

```
VideoDecodeSource -> ShaderPass -> PixelsOut
```

---

## Controls

### Keyboard

- Space: Play / Pause
- Left Arrow: Scrub backward
- Right Arrow: Scrub forward

---

## Key Pattern Demonstrated

- `FrameCtx.time` is overridden by a local transport state
- Decoder internals are untouched
- Time does not need to be monotonic

---

## Run Command

```
cargo run -p shadecore-example-video-scrub-keyboard-transport -- <video_config.json>
```

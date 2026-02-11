# Frame Context (`FrameCtx`)

This document defines the execution-time context passed to the runtime.

---

## Purpose

`FrameCtx` provides per-frame inputs to the engine.

It allows the caller to control:
- Output dimensions
- Time
- Frame index

---

## Fields

- `width`: Render width in pixels
- `height`: Render height in pixels
- `time`: Time in seconds
- `frame`: Frame counter

---

## Time Semantics

`time` is fully caller-controlled.

Valid uses include:
- Wall-clock time
- Manual scrubbing
- Paused playback
- Looping or jumping

The engine does not enforce monotonicity.

---

## Frame Index

`frame` is provided for:
- Debugging
- Shader variation
- Optional temporal logic

The engine does not interpret frame values.

---

## Design Guarantees

- `FrameCtx` is immutable during execution
- All values are read-only
- No hidden state is derived from it

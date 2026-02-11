# Transport and Time Pattern

This document describes how instruments control time and transport in ShadeCore.

Time is external and fully controlled by the instrument.
The engine treats `FrameCtx.time` as an opaque value.

---

## Goals

- Support pause, play, and scrubbing
- Allow non-real-time playback
- Keep transport logic out of engine crates

---

## Core Pattern

An instrument maintains a local transport state:

- `paused: bool`
- `playhead: f32` (seconds)
- Optional: speed, pending scrubs, loop points

On each frame:

1. Compute delta time (`dt`) from a monotonic clock
2. Update transport state
3. Write `playhead` into `FrameCtx.time`

---

## Basic Transport State

A minimal transport struct generally includes:

- A `playhead` position in seconds
- A `paused` flag

Optional fields:

- `speed` multiplier
- `pending_scrub` offsets
- Loop start/end points

---

## Input Integration

Transport responds to inputs such as:

- Keyboard (e.g. space toggles pause, arrows scrub)
- OSC (e.g. `/param/speed`, `/param/seek`)

Inputs only mutate transport state, not engine internals.

---

## FrameCtx Usage

After updating transport:

- Set `FrameCtx.time = playhead`
- Keep `width` and `height` in sync with the window
- Increment or manage `frame` as needed

This allows instruments to:

- Run normally (time increases)
- Pause (time stays fixed)
- Scrub (time jumps arbitrarily)
- Loop (wrap time within a range)

---

## Design Principles

- Time is caller-owned
- Transport logic is instrument-level
- Engine does not enforce or assume real-time playback

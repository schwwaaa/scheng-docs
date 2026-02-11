# Crate Map

This document describes the high-level crate structure of ShadeCore / Schengine.

It is intended for contributors who need to understand where responsibilities live.
It is **not** an API reference.

---

## Core Crates

### shadecore-core

- Fundamental types and traits shared across the engine
- Minimal dependencies
- No GL or windowing concerns

Treat this crate as the conceptual foundation.

---

### shadecore-graph

- Graph construction types
- Node IDs and port naming
- Graph validation and compilation into plans

Graphs describe topology only. No execution occurs here.

---

### shadecore-runtime

- Abstract runtime traits
- Execution interfaces that other runtimes (e.g. GL) implement

This crate defines what “executing a plan” means in abstract terms.

---

### shadecore-runtime-glow

- Concrete GL runtime implementation using `glow`
- Resource management (textures, programs, framebuffers)
- Integration with `shadecore-graph` and `shadecore-runtime`

This crate is responsible for actually running graphs on the GPU.

---

## Input / Output Crates

### shadecore-input-video

- Video decode sources driven by external configuration
- Integration with ffmpeg-based pipelines
- Provides video frames as textures into the graph

This crate does not own time or playback logic.

---

### Other Outputs

Output handling (e.g. readback, PixelsOut) lives primarily in `shadecore-runtime-glow`
and node implementations. External outputs (e.g. Syphon/Spout) are integrated at the
instrument or platform level.

# Examples Index

This section documents the **executable examples** shipped with ShadeCore.

Examples are not SDK reference. They are **reference instruments** that demonstrate:
- How to compose graphs
- How to drive shaders
- How to integrate OSC and input
- How to structure real applications

All examples are runnable binaries under `examples/`.

---

## Categories

### Instrument-Grade Examples

These examples are intended to be *played* and extended.

- `video_scrub_keyboard_transport`
- `graph_matrix_mix4_webcam`
- `graph_webcam_luma_key`

Each of these has a dedicated document describing its behavior and control surface.

---

### Minimal / Support Examples

These examples demonstrate isolated engine features.

- Video decode minimal
- Webcam minimal
- Shader pass minimal
- OSC minimal

They are useful for debugging and learning, but are not instruments.

---

## Conventions

- All interactive examples should expose OSC
- All examples log control input
- Shader logic lives in GLSL files or inline strings
- No example modifies engine internals

---

## Running Examples

All examples are run via:

```
cargo run -p <example-crate> [-- <args>]
```

Refer to each example document for exact commands.

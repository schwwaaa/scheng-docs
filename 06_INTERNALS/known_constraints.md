# Known Constraints and Non-Goals

This document records intentional constraints and non-goals of the current design.

It should be consulted before attempting deep engine changes.

---

## Time and Transport

- The engine does not own time
- All transport logic (play, pause, scrub, loop) is external
- `FrameCtx.time` may move forwards or backwards

If you find yourself adding time management to engine crates,
revisit the instrument-level transport pattern instead.

---

## Control Protocols

- OSC is not part of engine internals
- No protocol-specific logic should live in core crates
- Control is applied via `NodeProps` and local state only

New control schemes should be implemented at the instrument layer.

---

## Device Management

- Capture devices (webcams, cameras) are not managed by the engine
- External processes (e.g. ffmpeg) are responsible for feeding data
- The engine sees only textures and buffers

Do not add device-specific logic to runtime crates.

---

## Node Explosion

- New node kinds require careful design and maintenance
- Many behaviors can be implemented via shader overrides
- Prefer new shaders + NodeProps over new node types

If a behavior can be implemented with an existing node and a custom shader,
favor that approach.

---

## Performance Considerations

- Per-frame allocation in hot paths should be avoided
- Shader recompilation should not occur every frame
- Large resource changes should be batched

Profiling and optimization should focus on instrument patterns
before reaching for engine modifications.

---

## Summary

When in doubt:
- Keep engine crates small and stable
- Push behavior into instruments, shaders, and NodeProps
- Treat internals as a last resort, not a playground

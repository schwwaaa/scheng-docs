# Execution Model Internals

This document describes the internal execution lifecycle of a ShadeCore frame.

It is intended for contributors and advanced users.

---

## High-Level Flow

For each frame, a typical instrument does:

1. Handle input events (OSC, keyboard, etc.)
2. Update local state (transport, parameters)
3. Update `NodeProps`
4. Construct a `FrameCtx`
5. Call `execute_plan_outputs(...)`
6. Present the resulting image

Only step 5 is handled by the engine internals.

---

## Plan Execution

Internally, execution follows the compiled plan:

- Nodes are visited in a topologically sorted order
- For each node:
  - Inputs are resolved
  - Required resources are bound or created
  - Node-specific logic is executed

No dynamic graph changes occur during execution.

---

## Resource Caching

The runtime caches:

- Compiled shader programs
- Allocated textures
- Framebuffers

Caches are keyed by node identity and configuration.
This minimizes per-frame overhead.

---

## Error Paths

Internal execution may fail due to:

- Shader compilation errors
- Invalid resource states
- Misconfigured NodeProps

Errors are surfaced to the caller as `Result` values.

---

## Threading Assumptions

The runtime assumes:

- A current GL context exists on the calling thread
- Execution is performed on that thread

It does not create or manage threads internally.

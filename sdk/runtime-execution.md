---
layout: default
title: Runtime Execution
parent: SDK
nav_order: 7
permalink: /sdk/runtime-execution/
---

# Runtime Execution

The runtime evaluates a compiled execution plan for one frame. It implements execution semantics defined by the SDK. It does not schedule frames or manage time. This functionality is defined in `scheng-runtime` and fulfilled by backend implementations (e.g., `scheng-runtime-glow`).

---

## Responsibilities

The runtime:

- Executes nodes in deterministic topological order
- Resolves input dependencies
- Binds GPU resources
- Dispatches shader programs
- Produces output textures

The runtime does not mutate topology.

---

## Execution Model

Each invocation:

- Accepts a `FrameCtx`
- Reads current `NodeProps`
- Evaluates the compiled execution plan
- Returns resolved outputs

No implicit time accumulation occurs.  
All temporal semantics are external.

For detailed per-frame behavior, see **Per-Frame Execution Lifecycle**.

---

## Resource Management

Resource management is implementation-defined by the backend.

Typical behavior includes:

- Lazy shader compilation
- Per-node program caching
- Texture and framebuffer reuse across frames

Resource lifetime follows the compiled plan and backend strategy.

---

## Threading Model

Execution requires an active GL context on the calling thread.

The runtime:

- Is internally single-threaded
- Performs no implicit parallelism

External synchronization is the caller’s responsibility.

---

## Error Model

Execution may fail due to:

- Shader compilation or link errors
- Missing required inputs
- Invalid output configuration
- Backend resource failures

Errors are returned to the caller immediately.  
The runtime does not attempt silent recovery.

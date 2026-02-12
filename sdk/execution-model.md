---
layout: default
title: Execution Model Internals
parent: SDK Reference
nav_order: 2
permalink: /sdk/execution-model/
---

# Execution Model Internals

This document defines the internal execution lifecycle of one frame.

It describes what the engine does — and what it explicitly does not do.

---

# Frame Boundary

Each invocation of:

    execute_plan_outputs(...)

represents exactly one frame.

The runtime does not:

- Schedule frames
- Manage vsync
- Accumulate time

All timing originates externally via `FrameCtx`.

---

# Internal Execution Steps

Given a compiled plan, execution proceeds:

1. Read `FrameCtx`
2. Read `NodeProps`
3. Iterate nodes in deterministic topological order
4. For each node:
   - Resolve input textures
   - Bind shader program
   - Set uniforms
   - Issue draw call
5. Write final outputs

No graph mutation occurs during execution.

---

# Determinism

For identical:

- Graph topology
- NodeProps state
- FrameCtx values

Execution order and resource usage are deterministic (subject to deterministic shaders).

---

# Resource Lifecycle

Resources are managed lazily:

- Shaders compile on first use
- Programs are cached per node
- Textures/FBOs allocated on demand
- Reused across frames

Reallocation occurs only if:

- Resolution changes
- Shader source changes
- Node configuration invalidates cache

---

# Error Handling

Errors surface immediately and are returned to the caller.

Common error classes:

- Shader compile/link failure
- Missing required configuration
- GL context absence
- Invalid bindings

The runtime does not attempt silent recovery.

---

# Threading Assumptions

- Execution must occur on a thread with a current GL context.
- The runtime is single-threaded internally.
- External synchronization is the caller's responsibility.

Violating these assumptions results in undefined behavior.

---

# Design Guarantee

The runtime is a deterministic execution engine.

It is not:

- A scheduler
- A transport controller
- A device manager

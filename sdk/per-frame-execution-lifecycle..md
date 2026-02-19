---
layout: default
title: Per Frame Execution Lifecycle
parent: SDK
nav_order: 8
permalink: /sdk/per-frame-execution-lifecycle/
---

# Per Frame Execution Lifecycle

This document defines the per-frame execution lifecycle. It describes how a compiled execution plan is evaluated.   It does not define scheduling, timing, or graph mutation.

---

## Frame Boundary

Each invocation of:

    execute_plan_outputs(...)

represents exactly one frame.

The runtime does not:

- Schedule frames
- Manage vsync
- Accumulate time

All temporal state is supplied externally via `FrameCtx`.

---

## Execution Steps

Given a compiled plan, evaluation proceeds in deterministic order:

1. Read `FrameCtx`
2. Read `NodeProps`
3. Iterate nodes in topological order
4. For each node:
   - Resolve input textures
   - Bind shader program
   - Set uniforms
   - Issue draw call
5. Write final outputs

The execution plan is immutable.  
No topology changes occur during evaluation.

---

## Determinism

For identical:

- Graph topology
- `NodeProps` state
- `FrameCtx` values

Execution order and resource usage are deterministic (assuming deterministic shaders).

Structural determinism is guaranteed by `scheng-graph`.  
Execution determinism is enforced by `scheng-runtime`.

---

## Resource Lifecycle

GPU resources are managed lazily and cached:

- Shaders compile on first use
- Programs are cached per node
- Textures and FBOs are allocated on demand
- Resources are reused across frames

Reallocation occurs only if:

- Resolution changes
- Shader source changes
- Node configuration invalidates cache

The runtime does not pre-allocate unused resources.

---

## Error Handling

Errors are returned to the caller immediately.

Common failure cases:

- Shader compilation or link failure
- Missing required configuration
- GL context absence
- Invalid bindings

The runtime does not attempt silent recovery.

---

## Threading Model

- Execution must occur on a thread with a current GL context.
- The runtime is internally single-threaded.
- External synchronization is the caller’s responsibility.

Violating these assumptions results in undefined behavior.

---

## Design Scope

The runtime is a deterministic execution engine.

It is not:

- A scheduler
- A transport controller
- A device manager
- A time source

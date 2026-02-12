---
layout: default
title: Runtime Execution
parent: SDK Reference
nav_order: 6
permalink: /sdk/runtime-execution/
---

# Runtime Execution

The runtime executes a compiled graph for one frame.

It is a deterministic execution engine, not a scheduler.

---

## Responsibilities

- Execute nodes in topologically sorted order
- Bind resources
- Dispatch shader programs
- Produce output textures

---

## Execution Model

Each call:

- Accepts `FrameCtx`
- Reads `NodeProps`
- Executes compiled plan
- Returns outputs

No implicit time accumulation occurs.

---

## Resource Management

- Lazy shader compilation
- Per-node resource caching
- Texture reuse across frames

---

## Threading Model

Requires active GL context on calling thread.

No internal multithreading is performed.

---

## Error Model

Execution may fail due to:

- Shader compile/link errors
- Missing inputs
- Invalid output configuration

Errors are returned immediately.

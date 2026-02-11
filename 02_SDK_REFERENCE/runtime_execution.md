# Runtime Execution

This document describes how ShadeCore executes a compiled graph.

---

## Execution Entry Point

Execution is performed via:

```
execute_plan_outputs(...)
```

This function:
- Executes the compiled plan once
- Processes all nodes in order
- Produces output textures

---

## Execution Scope

Each call represents **one frame**.

The runtime:
- Does not maintain temporal state across frames
- Does not assume real-time pacing

---

## Resource Management

The runtime manages:
- GL programs
- Textures
- Framebuffers

Resources are:
- Lazily created
- Cached per node
- Reused across frames

---

## Error Handling

Execution errors include:
- Shader compilation failures
- Missing inputs
- Invalid output configuration

Errors are returned to the caller.

---

## Threading

Execution is expected to occur on a thread
with a current GL context.

The runtime is not internally multi-threaded.

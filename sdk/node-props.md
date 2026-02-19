---
layout: default
title: NodeProps Reference
parent: SDK
nav_order: 5
permalink: /sdk/node-props/
---

# NodeProps Reference

`NodeProps` defines the complete dynamic configuration surface for nodes & supplies per-node configuration used during evaluation. Topology is defined by `scheng-graph`.  Execution is performed by `scheng-runtime`.  

All dynamic behavior flows through this structure.

---

## Ownership Model

`NodeProps` is:

- Owned and managed by the caller
- Read-only during execution
- Never mutated by the runtime

The runtime consumes configuration. It does not store behavioral state internally. This separation ensures deterministic frame evaluation.

---

## Responsibilities

`NodeProps` supplies per-node configuration, including:

- Shader sources
- Uniform and parameter values
- Mixer weights
- Video decode configuration
- External texture bindings
- Output naming

`NodeProps` does not define topology.  
It configures nodes within an already compiled graph.

---

## Runtime Interaction

During per-frame execution:

1. The runtime reads configuration by `NodeId`
2. Binds shader programs
3. Sets uniforms and bindings
4. Executes draw calls

Node behavior is derived entirely from:

- Graph topology
- `NodeProps`
- `FrameCtx`

No hidden state persists inside nodes across frames.

---

## Update Semantics

Changes to `NodeProps`:

- Take effect on the next frame
- Do not invalidate compiled topology
- May trigger shader recompilation if shader sources change

Structural changes require graph recompilation.

---

## Failure Modes

Incorrect configuration may result in:

- Shader compilation or link errors
- Missing required input bindings
- Invalid uniform values

Errors are surfaced to the caller.  
The runtime does not attempt silent recovery.

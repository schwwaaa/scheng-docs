---
layout: default
title: NodeProps Reference
parent: SDK Reference
nav_order: 4
permalink: /sdk/node-props/
---

# NodeProps Reference

`NodeProps` is the complete configuration surface for node behavior.

All dynamic behavior flows through this structure.

---

## Ownership Model

- Owned by the caller
- Read-only during execution
- Not mutated by the runtime

This ensures predictable frame execution.

---

## Responsibilities

`NodeProps` supplies:

- Shader sources
- Parameter values
- Mixer weights
- Video decode configuration
- External texture bindings
- Output naming

---

## Runtime Interaction

During execution:

1. Runtime reads relevant entries by `NodeId`
2. Binds shader programs
3. Sets uniforms
4. Executes draw calls

No hidden state persists inside nodes.

---

## Update Semantics

Changes to `NodeProps`:

- Take effect on the next frame
- Do not invalidate compiled topology
- May trigger shader recompilation if source changes

---

## Failure Modes

Incorrect configuration may result in:

- Shader compile errors
- Missing input bindings
- Undefined uniform values

Errors are surfaced to the caller.

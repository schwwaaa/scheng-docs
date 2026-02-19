---
layout: default
title: Graph API
parent: SDK
nav_order: 3
permalink: /sdk/graph-api/
---

# Graph API

The Graph API defines execution topology. It specifies how nodes connect and how structure is validated. It does not define node behavior or perform execution.

*This functionality lives in `scheng-graph`.*

---

## Topology

A graph is a directed set of nodes connected by typed ports.

Each node exposes:

- Input ports
- Output ports

Connections define data flow between nodes.

Example:

Input → ColorCorrect → Blur → Output

The Graph API defines and validates this structure before runtime execution begins.

---

## Responsibilities

The graph:

- Creates node instances
- Defines port layouts
- Connects output ports to input ports
- Validates topology
- Compiles into an immutable execution plan

Once compiled, topology is fixed until recompilation.

---

## Compilation

Calling `compile()` performs:

1. Cycle detection
2. Port validation
3. Deterministic topological sorting
4. Execution plan construction

The resulting plan is immutable and safe for repeated per-frame evaluation.

---

## Separation of Concerns

The Graph API does NOT define:

- Shader code
- Parameter values
- Execution timing
- Control input
- GPU resource management

Behavior is provided via `NodeProps` and `FrameCtx`.  
Execution is handled by `scheng-runtime`.  
GPU fulfillment is handled by `scheng-runtime-glow`.

---

## Determinism

Given identical:

- Graph topology
- `NodeProps`
- `FrameCtx`

Execution order is deterministic (assuming deterministic shaders).

Topology determinism is guaranteed at compile time.

---

## Design Rule

Topology is static.  
Configuration is dynamic.

Structure must be compiled before execution.

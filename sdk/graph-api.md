---
layout: default
title: Graph API
parent: SDK Reference
nav_order: 2
permalink: /sdk/graph-api/
---

# Graph API

The Graph API defines the topology of execution.

It describes how nodes connect, not how they behave.

---

## Responsibilities

The graph:

- Creates node instances
- Defines port layouts
- Connects output ports to input ports
- Validates topology
- Compiles into an immutable execution plan

---

## Separation of Concerns

The graph does NOT define:

- Shader code
- Parameter values
- Execution timing
- Control input

Behavior is supplied via `NodeProps` and `FrameCtx`.

---

## Compilation Model

Calling `compile()` performs:

1. Cycle detection
2. Port validation
3. Deterministic topological sorting
4. Plan construction

The resulting plan is immutable.

---

## Determinism Guarantees

Given identical:

- Graph topology
- `NodeProps`
- `FrameCtx`

Execution order and results are deterministic (assuming deterministic shaders).

---

## Failure Conditions

Compilation fails if:

- Ports are unconnected where required
- Cycles exist
- Invalid port names are referenced

---

## Design Principle

Topology is static.  
Configuration is dynamic.

This separation enables high-performance per-frame execution.

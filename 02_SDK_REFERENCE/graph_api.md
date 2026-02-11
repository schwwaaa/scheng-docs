# Graph API

This document describes the ShadeCore graph construction API.

The graph defines **topology only**. It does not encode behavior, timing, or control.

---

## Graph Purpose

A `Graph` describes how data flows between nodes.

It defines:
- Node instances
- Named input/output ports
- Connections between nodes

It does not define:
- Parameter values
- Shader logic
- Execution timing

---

## Node Identity

Nodes are identified by opaque `NodeId` values.

- IDs are assigned when nodes are added
- IDs are stable for the lifetime of the graph
- IDs are used as keys into `NodeProps`

---

## Ports

Each node kind defines a fixed set of ports.

- Ports are identified by string names
- Connections must match compatible ports
- Validation occurs at compile time

---

## Connections

Connections are directional:

```
connect_named(src, "out", dst, "in")
```

Rules:
- One output may feed multiple inputs
- Inputs accept a single upstream connection
- Cycles are not allowed

---

## Compilation

Calling `compile()`:

- Validates all connections
- Resolves execution order
- Produces an immutable execution plan

After compilation, the graph structure must not change.

---

## Design Guarantees

- Graph compilation is deterministic
- Invalid graphs fail fast
- Execution order is explicit and repeatable

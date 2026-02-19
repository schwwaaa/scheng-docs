---
layout: default
title: OSC Contract
parent: SDK
nav_order: 9
permalink: /sdk/osc-contract/
---

# OSC Contract

The OSC Contract defines how external control values enter the instrument layer. The engine and runtime remain protocol-agnostic. Control systems update `NodeProps`. The runtime consumes those values during frame evaluation.

---

## Control Flow

Control integration occurs entirely outside core crates.

Typical flow:

1. Receive OSC message externally
2. Parse address and value
3. Map address to a `NodeId` and parameter
4. Update `NodeProps`

The runtime reads updated values on the next frame.

---

## Guarantees

- No protocol handling inside core engine crates
- No transport dependency in `scheng-runtime`
- Latest value wins
- No implicit interpolation
- No internal buffering beyond the current `NodeProps` state

All temporal behavior must be implemented at the instrument layer.

---

## Responsibility Boundary

Application layer responsibilities:

- Socket lifecycle
- Message parsing
- Address validation
- Value mapping
- Range clamping
- Thread synchronization

Engine responsibilities:

- Read `NodeProps`
- Apply values during deterministic frame evaluation

The engine does not:

- Parse OSC
- Maintain transport state
- Perform smoothing or interpolation

---

## Recommended Practices

- Clamp values before writing to `NodeProps`
- Validate `NodeId` and parameter existence
- Avoid blocking in control handlers
- Synchronize updates if control input is multi-threaded
- Log parameter updates when debugging

Control systems should remain stateless beyond `NodeProps`.

---
layout: default
title: OSC Contract
parent: SDK Reference
nav_order: 5
permalink: /sdk/osc-contract/
---

# OSC Contract

The OSC Contract defines how control values enter the instrument layer.

The runtime remains transport-agnostic.

---

## Control Flow

1. Receive OSC externally
2. Parse address/value
3. Map to parameters
4. Update `NodeProps`

---

## Guarantees

- No protocol dependency in core engine
- Latest value wins
- No implicit interpolation

---

## Responsibility Boundary

Application layer handles:

- Socket lifecycle
- Message parsing
- Validation
- Value mapping

Runtime handles:

- Reading updated parameters

---

## Recommended Practices

- Clamp values explicitly
- Log parameter updates
- Avoid blocking in control handlers

---
layout: default
title: Known Constraints and Non-Goals
parent: SDK
nav_order: 10
permalink: /sdk/known-constraints/
---

# Known Constraints and Non-Goals

This document records intentional architectural constraints.

These define what the engine does not do and where responsibility boundaries exist.

---

## Time and Transport

This section defines how temporal control is handled in the system. The engine evaluates frames. It does not manage playback state or time progression.

**Constraint:**  
The engine does not own time.

**Implications:**

- No play/pause inside core crates
- No internal frame clock
- No transport logic in runtime

All transport semantics belong to the instrument layer.

**Subject to change*

---

## Control Protocols

This section defines how external control signals enter the system. The engine processes values. It does not implement protocol handling.

**Constraint:**  
The engine is protocol-agnostic.

**Implications:**

- No OSC parsing in core crates
- No MIDI handling in runtime
- No network socket management

Control values enter the engine via `NodeProps` only.

---

## Device Management

This section defines the boundary between the engine and hardware. The engine consumes GPU resources but does not manage devices.

**Constraint:**  
Device interaction is external.

**Implications:**

- No webcam enumeration in runtime
- No OS-specific capture APIs in engine
- No platform-specific code in core crates

The engine consumes textures.  
It does not manage hardware.

---

## Node Surface Area

This section defines how node types evolve. The system favors extensibility through configuration rather than proliferation of node categories.

**Constraint:**  
Node kinds must remain minimal.

**Implications:**

- Prefer shader extensibility over introducing new node types
- Avoid duplicating node categories
- Preserve stable execution semantics

New behavior should default to shader or instrument-level extension.

---

## Performance Constraints

This section defines execution-level performance expectations. The runtime must remain predictable and allocation-stable under per-frame load. Performance issues should be addressed at the instrument level before modifying engine internals.

The engine avoids:

- Per-frame allocation in hot paths
- Shader recompilation per frame
- Hidden resource churn

---

## Contributor Guidance

This section clarifies ownership boundaries when proposing changes. If ownership is unclear, default to instrument-level implementation.

When proposing changes, determine ownership:

1. Topology (`scheng-graph`)
2. Configuration (`NodeProps`)
3. Runtime (`scheng-runtime`)
4. Instrument layer

---

## Core Principle

This section summarizes the architectural intent. Behavior belongs at the edges.

The engine remains:

- Small
- Deterministic
- Execution-focused

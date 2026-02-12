---
layout: default
title: Known Constraints and Non-Goals
parent: SDK Reference
nav_order: 3
permalink: /sdk/known-constraints/
---

# Known Constraints and Non-Goals

This document records intentional architectural constraints.

These are not limitations by accident — they are design decisions.

---

# Time and Transport

Constraint:
The engine does not own time.

Implication:
- No play/pause inside core crates
- No internal frame clock
- No transport logic in runtime

All transport semantics belong to the instrument layer.

---

# Control Protocols

Constraint:
The engine is protocol-agnostic.

Implication:
- No OSC parsing in core crates
- No MIDI handling in runtime
- No network socket management

Control values enter via NodeProps only.

---

# Device Management

Constraint:
Devices are external concerns.

Implication:
- No webcam enumeration in runtime
- No OS-specific capture APIs in engine
- No platform-specific code in core crates

Engine consumes textures — it does not manage hardware.

---

# Node Proliferation

Constraint:
Node kinds must remain minimal.

Implication:
- Prefer shader extensibility over new node types
- Avoid duplicating node categories
- Maintain stable execution semantics

---

# Performance Constraints

- Avoid per-frame allocation in hot paths
- Avoid shader recompilation per frame
- Avoid hidden resource churn

Instrument-level design should be optimized before engine changes.

---

# Contributor Guidance

When proposing changes:

Ask:

1. Does this belong in topology?
2. Does this belong in configuration?
3. Does this belong in runtime?
4. Or does this belong in the instrument layer?

If the answer is unclear, default to instrument-level implementation.

---

# Core Principle

The engine remains small, deterministic, and execution-focused.

Behavior belongs at the edges.

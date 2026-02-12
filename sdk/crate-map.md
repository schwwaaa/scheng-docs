---
layout: default
title: Crate Map
parent: SDK Reference
nav_order: 1
permalink: /sdk/crate-map/
---

# Crate Map

This document defines the architectural boundaries of the ShadeCore / Schengine workspace.

It describes where responsibilities live and, equally important, where they do not.

This is not an API reference. It is a system ownership map.

---

# Architectural Principles

The workspace is organized around strict separation of concerns:

- Topology (graph)
- Configuration (NodeProps)
- Execution (runtime)
- GPU integration (runtime-glow)
- Input/Output integration (edge crates)

No crate should violate these boundaries.

---

# Core Crates

## shadecore-core

Purpose:
Defines foundational types and traits shared across the engine.

Responsibilities:
- Shared identifiers (NodeId, etc.)
- Core trait definitions
- Minimal dependency surface

Non-Responsibilities:
- No GL bindings
- No windowing
- No device access
- No execution logic

This crate must remain small and dependency-light.

---

## shadecore-graph

Purpose:
Defines graph topology and compilation.

Responsibilities:
- Node instantiation
- Port definitions
- Connection wiring
- Validation
- Compilation into execution plans

Non-Responsibilities:
- No shader compilation
- No GPU resources
- No runtime execution
- No parameter mutation

Graph is static topology only.

---

## shadecore-runtime

Purpose:
Defines abstract execution traits.

Responsibilities:
- Execution interfaces
- Plan execution contracts
- Runtime behavior definitions (abstract)

Non-Responsibilities:
- No concrete GL integration
- No device management

This crate defines what it means to execute a plan, without specifying how.

---

## shadecore-runtime-glow

Purpose:
Concrete OpenGL runtime implementation using `glow`.

Responsibilities:
- Shader compilation
- Program caching
- Texture management
- Framebuffer allocation
- Per-frame execution

Non-Responsibilities:
- No topology mutation
- No control protocol parsing
- No time ownership

This crate is the GPU execution engine.

---

# Input / Output Crates

## shadecore-input-video

Purpose:
Video decoding and texture production.

Responsibilities:
- External decode integration
- JSON configuration loading
- Texture delivery to graph

Non-Responsibilities:
- No transport control
- No engine time management

---

# Integration Boundary

Platform-specific integrations (Syphon, Spout, capture, etc.) must:

- Live outside core crates
- Treat runtime as a pure execution engine
- Avoid injecting platform logic into engine internals

---

# Stability Expectations

Core crates should change slowly.

Runtime implementations may evolve, but must not break:

- Graph compilation guarantees
- NodeProps contract
- FrameCtx semantics

This crate map is a contract between contributors.

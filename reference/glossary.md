---
layout: default
title: Glossary
parent: Reference
nav_order: 2
permalink: /reference/glossary/
---

# Glossary

This glossary defines the canonical terminology used throughout the ShadeCore SDK documentation.

The definitions here are authoritative.  
If a term is used in SDK, Internals, or Patterns documentation, it should align with this glossary.

---

# Core Architectural Terms

## Instrument

A user-defined application built on top of the ShadeCore engine.

An instrument is responsible for:

- Constructing the Graph
- Supplying NodeProps
- Supplying FrameCtx
- Handling control input (OSC, MIDI, etc.)
- Managing external textures or devices

The engine does not define instruments. Instruments define themselves around the engine.

---

## Engine

The collective core crates that define:

- Graph topology
- Runtime execution
- GPU integration

The engine is deterministic and execution-focused.  
It does not own time, control protocols, or device management.

---

## Graph

The static topology describing how nodes connect.

The graph defines:

- Nodes
- Ports
- Directed connections
- Acyclic data flow

The graph does not define behavior. It defines structure.

---

## Execution Plan

An immutable, compiled representation of a Graph.

Produced by calling `compile()`.

The execution plan defines:

- Deterministic node order
- Validated connections
- Runtime-ready topology

The plan cannot be modified once compiled.

---

## Node

A single unit of computation inside the graph.

A node may:

- Produce textures
- Transform textures
- Combine textures
- Write final outputs

Nodes do not own time or control state.

---

## NodeId

An opaque identifier assigned by the Graph.

Used as the key into:

- NodeProps
- Shader bindings
- Parameter maps

NodeId is stable for the lifetime of a graph instance.

---

## NodeProps

The complete configuration surface for nodes.

NodeProps supplies:

- Shader sources
- Mixer parameters
- Video configuration
- Texture bindings
- Output naming

NodeProps is owned by the instrument and read by the runtime.

---

## FrameCtx

The immutable per-frame execution context.

Contains:

- width
- height
- time
- frame

The engine does not generate or mutate FrameCtx.

---

## Runtime

The component that executes a compiled plan for one frame.

The runtime:

- Reads NodeProps
- Reads FrameCtx
- Binds resources
- Dispatches draw calls
- Produces outputs

The runtime is not a scheduler.

---

## Runtime Implementation

A concrete execution backend.

Example:
- `shadecore-runtime-glow`

Responsible for:

- Shader compilation
- GL resource management
- Texture allocation
- Framebuffer management

---

# GPU & Rendering Terms

## ShaderPass

A processing node that executes a GLSL program over input textures.

Used for:

- Effects
- Transformations
- Post-processing

---

## Mixer

A node that combines multiple input textures into one output.

Mixing semantics are defined entirely by the shader.

The engine does not impose RGB or channel semantics.

---

## Uniform

A shader parameter supplied by the runtime.

Standard uniforms include:

- `uResolution`
- `uTime`
- `uFrame`

Custom uniforms must be supplied via NodeProps.

---

## Sampler Binding

The mapping between a graph input texture and a shader sampler uniform.

Sampler ordering is deterministic per node kind.

---

## Framebuffer (FBO)

A GPU render target used for intermediate or final outputs.

Managed internally by the runtime implementation.

---

# Control & Integration Terms

## Control Layer

The instrument-level system that:

- Receives external messages (OSC, MIDI, etc.)
- Maps values to parameters
- Updates NodeProps

The control layer is outside the engine.

---

## OSC Contract

The pattern by which OSC values are translated into parameter updates.

The engine does not parse OSC directly.

---

## Texture Input

An externally created texture injected into the graph.

Ownership model:

- Instrument owns the texture handle
- Runtime consumes it during execution

---

## Transport

The time-control model implemented by the instrument.

Examples:

- Play
- Pause
- Scrub
- Loop

Transport logic updates FrameCtx.time.

---

# Determinism & Execution Terms

## Deterministic Execution

For identical:

- Graph
- NodeProps
- FrameCtx

The runtime produces identical execution order and resource bindings.

Shader determinism depends on shader code.

---

## Lazy Compilation

Shader programs are compiled only on first use.

Compiled programs are cached for reuse across frames.

---

## Hot Path

The per-frame execution path.

Must avoid:

- Allocation
- Shader recompilation
- Blocking I/O

---

# Architectural Boundaries

## Topology vs Configuration

Topology:
- Defined by Graph
- Static after compile

Configuration:
- Defined by NodeProps
- Mutable between frames

---

## Engine vs Instrument

Engine:
- Deterministic executor
- No protocol ownership
- No device ownership
- No time ownership

Instrument:
- Owns time
- Owns control
- Owns external textures
- Owns graph construction

---

# Non-Goals

The engine is not:

- A media player
- A scheduler
- A device manager
- A protocol parser

These responsibilities belong to the instrument layer.

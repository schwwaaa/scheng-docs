---
layout: default
title: Overview
parent: SDK
nav_order: 0
permalink: /sdk/
---

# SDK Overview

The scheng SDK defines the public surface for building video instruments on top of the scheng engine. It provides the contracts, types, and execution interfaces required to construct graph-based, GPU-accelerated visual systems. This document describes the role, structure, and boundaries of the SDK.

## High Level
At a high level, the SDK acts as the declarative contract layer between instrument code and engine fulfillment.

*The SDK defines ...*

- How nodes are declared
- How parameters are structured
- How frame context is passed
- How shaders are registered and invoked
- How execution is triggered per frame

*The SDK exists to ...*

- Define stable interfaces between graph, runtime, and shader layers
- Enforce explicit execution contracts
- Enable instrument development without modifying core engine internals
- Provide predictable, deterministic runtime behavior

*The SDK does not include ...*

- Internal runtime scheduling details
- GPU driver abstractions
- Private engine state
- Experimental or unstable internal utilities

---

## Scope

### Graph Construction APIs
The Graph Construction APIs provide the primitives for building directed execution graphs that define how data flows between nodes. These APIs allow developers to create nodes, connect outputs to inputs, define execution order, and describe the overall topology of a visual or computational pipeline.

They form the structural backbone of the SDK — enabling deterministic, inspectable, and reusable graph definitions.

Responsibilities:

- Node instantiation
- Texture routing
- Graph topology validation


### Node Contract Definitions

Node contracts define the formal interface of a node: its inputs, outputs, expected data types, and execution behavior. A contract ensures that nodes can be validated, composed, and executed safely within the graph without ambiguity.

These definitions create strict boundaries between graph logic and runtime implementation, ensuring consistency across custom and built-in nodes.
Responsibilities:

- Define input/output texture expectations
- Expose configurable parameters
- Execute per-frame logic safely

### Runtime Execution Interfaces

Defines how frames are planned and dispatched. Runtime execution interfaces provide the abstraction layer responsible for evaluating the graph. They define how nodes are scheduled, how frames or buffers are processed, and how outputs are resolved. The runtime does not define shader behavior. It coordinates execution.

This layer separates graph definition from execution backend (e.g., CPU, GPU, Glow, headless runtime), allowing multiple runtimes to operate against the same graph model.

Responsibilities:

- Accept graph definitions
- Prepare execution plans
- Invoke node execution in correct order
- Maintain frame context

### Shader Form Contracts

Defines the interface between node logic and GPU shader forms. Shader form contracts describe the expected structure and binding behavior of shader-based nodes. They define how uniforms, textures, and parameters are exposed and mapped to the runtime. 

This ensures that shader modules behave predictably across runtimes and that parameter surfaces are formally declared rather than implicitly assumed.

Responsibilities:

- Uniform bindings
- Texture inputs
- Output target definitions
- Explicit parameter mapping


### NodeProps and Parameter Surfaces

NodeProps represent the configurable parameter surface of a node. Defines parameter surfaces for nodes. They provide a structured way to expose values such as floats, toggles, vectors, or other control inputs that influence node behavior.

Parameter surfaces are designed to be introspectable and controllable, making them compatible with UI systems, automation, or external control signals. NodeProps are the boundary between control inputs and execution logic.

Responsibilities:

- Store runtime-modifiable values
- Provide default configuration
- Maintain deterministic parameter state

### Control Propagation Structures (e.g., OSC Integration Points)

Control propagation structures define how external control signals move through the system. This includes integration points for OSC, MIDI, or other real-time control mechanisms.

These structures ensure that parameter updates, automation, and live performance inputs can be injected into the graph safely and deterministically without breaking execution guarantees.

---

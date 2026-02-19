---
layout: default
title: Architecture
nav_order: 2
permalink: /architecture/
---

# Architecture

*scheng* enables developers to construct programmable video instruments. Rather than being a fixed VJ application, *scheng* is an engine and SDK for building custom video instruments with predictable execution behavior. It provides a graph-based execution model, explicit shader and node contracts, and a modular SDK surface for building deterministic, extensible visual systems.

---

# Architecture at a Glance

*scheng* is organized as a multi-crate workspace that separates responsibilities into clearly defined layers. Each layer exists to reduce ambiguity, preserve determinism, and allow independent evolution without breaking runtime guarantees.

<img src="/assets/img/scheng-architecture.png"
     alt="Logo"
     style="display:block;margin:0 auto;"
     width=240px>

## Layers 

Each layer is intentionally isolated to enforce clarity, extensibility, and real-time safety.

1. **Graph Layer**  --> *Declarative signal topology and node relationships.*
2. **Runtime Layer**  --> *Frame planning and deterministic execution.*
3. **Shader Contracts**  --> *Explicit interfaces between nodes and GPU forms.*
4. **Control Inputs**  --> *OSC, keyboard, transport, and external state.*
5. **Output Targets**  --> *Render targets and external integrations.*

---


## 1. Graph Layer  
**Declarative signal topology and node relationships.**

The Graph Layer defines *what connects to what*.

It is purely structural — no GPU work happens here.  
Developers declare nodes, their inputs and outputs, and how signals flow between them. The graph is a topology description of a visual instrument.

Key properties:

- No frame timing logic  
- No rendering logic  
- No control handling  
- Only topology and node relationships  

This separation ensures that signal structure remains stable and inspectable before execution.

---

## 2. Runtime Layer  
**Frame planning and deterministic execution.**

The Runtime Layer defines *when and how the graph executes*.

For every frame:

- A plan is generated  
- Node execution order is resolved  
- Resources are prepared  
- Execution proceeds deterministically  

The runtime guarantees:

- Stable ordering  
- No implicit side effects  
- Real-time safety boundaries  
- Frame-by-frame predictability  

The graph defines structure.  
The runtime defines execution.

---

## 3. Shader Contracts  
**Explicit interfaces between nodes and GPU forms.**

Shader Contracts define *what a node promises*.

Every node exposes:

- Required inputs  
- Produced outputs  
- Parameter interfaces  
- Expected texture formats  

This prevents implicit coupling between nodes and makes GPU execution explicit and testable.

Contracts are not implementation — they are agreements.

They allow:

- Node composability  
- Validation before execution  
- Safe extensibility  
- Clear SDK boundaries  

---

## 4. Control Inputs  
**OSC, keyboard, transport, and external state.**

Control is external to rendering.

It modifies node parameters without altering graph structure.

Examples:

- OSC messages  
- Transport controls  
- Keyboard events  
- External automation  

Control never executes shaders directly.  
It updates parameters.  
The runtime then applies those changes on the next frame.

This separation prevents race conditions and keeps execution deterministic.

---

## 5. Output Targets  
**Render targets and external integrations.**

Outputs define *where frames go*.

They consume the final graph results and route them to:

- Windows  
- Textures  
- Syphon / Spout  
- Encoders (future)  
- Network streams (future)  

Outputs do not modify graph state.  
They are sinks.

---

## Layer Isolation Philosophy

Each layer is intentionally isolated to enforce:

- Predictable execution  
- Clear debugging boundaries  
- Modular crate evolution  
- Real-time performance discipline  
- Long-term SDK stability  

No layer implicitly reaches into another.  
All interaction is explicit.

---
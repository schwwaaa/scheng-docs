---
layout: default
title: Overview
nav_order: 2
permalink: /overview/
---

# Overview


scheng enables developers to construct programmable video instruments using:

- Directed signal graphs
- GPU shader execution
- Real-time parameter control
- Structured runtime planning

Rather than being a fixed VJ application, scheng is an engine and SDK for building custom instruments with predictable execution behavior. It provides a graph-based execution model, explicit shader and node contracts, and a modular SDK surface for building deterministic, extensible visual systems.

---

# Architecture at a Glance

scheng is organized as a multi-crate workspace separating core responsibilities:

1. **Graph Layer**  
   Declarative signal topology and node relationships.

2. **Runtime Layer**  
   Frame planning and deterministic execution.

3. **Shader Contracts**  
   Explicit interfaces between nodes and GPU forms.

4. **Control Inputs**  
   OSC, keyboard, transport, and external state.

5. **Output Targets**  
   Render targets and external integrations.

Each layer is intentionally isolated to enforce clarity, extensibility, and real-time safety.

---

# Core Concepts

**Graph**  
A declarative structure defining signal flow between nodes.

**Node**  
A unit of execution implementing a defined contract. Nodes consume and produce textures.

**Instrument**  
An executable built on the scheng SDK that wires graph, runtime, and controls.

**Runtime**  
The system responsible for frame scheduling and dispatch.

**Transport**  
Time and playback control propagation through the graph.

**Control Surface**  
External systems that modify node parameters in real time.

---

# Design Principles

- Explicit execution contracts  
- Clear separation of concerns  
- Deterministic frame behavior  
- Extensibility without runtime modification  
- GPU-first architecture  

---

# What scheng Is Not

scheng is not:

- A GUI-based patch editor
- A turnkey VJ application
- A video editing suite
- A high-level scripting framework

It is a programmable engine for constructing video instruments.

---

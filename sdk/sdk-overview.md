---
layout: default
title: SDK Overview
parent: SDK Reference
nav_order: 1
permalink: /sdk/
---

# SDK Overview

The scheng SDK defines the public surface for building video instruments on top of the scheng engine.

It provides the contracts, types, and execution interfaces required to construct graph-based, GPU-accelerated visual systems.

This document describes the role, structure, and boundaries of the SDK.

---

## Purpose

The SDK exists to:

- Define stable interfaces between graph, runtime, and shader layers
- Enforce explicit execution contracts
- Enable instrument development without modifying core engine internals
- Provide predictable, deterministic runtime behavior

The SDK is the supported extension surface of scheng.

---

## Scope

The SDK includes:

- Graph construction APIs
- Node contract definitions
- Runtime execution interfaces
- Shader form contracts
- NodeProps and parameter surfaces
- Control propagation structures (e.g., OSC integration points)

The SDK does not include:

- Internal runtime scheduling details
- GPU driver abstractions
- Private engine state
- Experimental or unstable internal utilities

For engine implementation details, see **Internals**.

---

## Architecture Model

At a high level, the SDK sits between instrument code and engine execution.

Instrument Code  
→ SDK Contracts  
→ Runtime Execution  
→ GPU Shader Execution  

The SDK defines:

- How nodes are declared
- How parameters are structured
- How frame context is passed
- How shaders are registered and invoked
- How execution is triggered per frame

---

## Core Components

### Graph API

Defines how nodes and edges are declared and connected.

Responsibilities:

- Node instantiation
- Texture routing
- Graph topology validation

---

### Node Contracts

Each node must implement a defined execution contract.

Responsibilities:

- Define input/output texture expectations
- Expose configurable parameters
- Execute per-frame logic safely

---

### NodeProps

Defines parameter surfaces for nodes.

Responsibilities:

- Store runtime-modifiable values
- Provide default configuration
- Maintain deterministic parameter state

NodeProps are the boundary between control inputs and execution logic.

---

### Runtime Execution Interface

Defines how frames are planned and dispatched.

Responsibilities:

- Accept graph definitions
- Prepare execution plans
- Invoke node execution in correct order
- Maintain frame context

The runtime does not define shader behavior. It coordinates execution.

---

### Shader Contract

Defines the interface between node logic and GPU shader forms.

Responsibilities:

- Uniform bindings
- Texture inputs
- Output target definitions
- Explicit parameter mapping

Shader contracts must remain consistent to preserve execution guarantees.

---

## Stability Guarantees

Public SDK APIs are considered stable unless explicitly documented otherwise.

Changes that affect:

- Node contract signatures
- Frame context structures
- Shader uniform expectations

are considered breaking and must be versioned appropriately.

---

## Extension Model

To extend scheng using the SDK:

- Create new nodes implementing the required contract
- Register new shader forms
- Extend NodeProps for parameter exposure
- Wire graph topology explicitly

The SDK allows feature growth without modifying runtime internals.

---

## Design Principles

- Explicit over implicit
- Deterministic frame execution
- Clear boundary between graph and runtime
- GPU-first architecture
- Minimal hidden state

---

## When to Use the SDK Reference

Use the SDK Reference when:

- Implementing new nodes
- Registering shader forms
- Modifying parameter surfaces
- Investigating execution flow
- Ensuring contract correctness

For conceptual explanations and workflows, see **Guides**.

For internal engine mechanics, see **Internals**.

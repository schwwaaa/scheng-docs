---
layout: default
title: Pipeline
parent: SDK
nav_order: 1
permalink: /sdk/pipeline
---

# Pipeline

The pipeline describes how responsibility flows from instrument declaration to GPU fulfillment. This is not a call stack and not an internal implementation trace. It is a layered responsibility model aligned with crate boundaries. Each layer corresponds directly to architectural boundaries in the workspace.

---

## Overview

- **Instrument Code** → *Instrument code describes structure, parameters, and execution intent.*
- **SDK Contract Layer** → *The SDK formalizes those declarations into validated contracts.*  
- **Deterministic Runtime Evaluation** → *The runtime evaluates those contracts deterministically.*  
- **Backend GPU Execution** → *The backend executes the resulting shader work on the GPU.*

### Key Principle

No layer reaches backward across boundaries.

The pipeline enforces strict separation:

- Graph defines topology.
- Runtime defines execution semantics.
- Backend defines GPU fulfillment.

This alignment ensures:

- Deterministic behavior
- Backend independence
- Clear ownership across crates
- Stable extension points


---

## Pipeline Layers

### Instrument Code
This is user space. This layer declares structure only.

Instrument code uses:

- `scheng-graph` to declare topology
- `NodeProps` to configure parameter surfaces
- Shader contracts to describe GPU expectations

At this stage:

- No GPU resources exist
- No frame execution occurs
- No scheduling decisions are made

---

### SDK Contract Layer
No execution happens here. Only structural correctness is enforced.

This corresponds to:

- `scheng-graph` validation
- Contract enforcement in core types

Responsibilities:

- Validate graph topology
- Enforce typed port connections
- Ensure NodeProps integrity
- Guarantee structural determinism

---

### Deterministic Runtime Evaluation

This layer defines *how plans are evaluated*, but not how GPU work is performed. Runtime is abstract.

This corresponds to:

- `scheng-runtime`

Responsibilities:

- Accept compiled graph plans
- Determine execution ordering
- Maintain frame context
- Invoke node execution contracts

---

### Backend GPU Execution

This layer fulfills the runtime contract using a concrete backend implementation.

This corresponds to:

- `scheng-runtime-glow`

Responsibilities:

- Compile shaders
- Bind uniforms and textures
- Allocate framebuffers
- Dispatch GPU work
- Produce output surfaces

---


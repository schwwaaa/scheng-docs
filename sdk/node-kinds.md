---
layout: default
title: Node Kinds
parent: SDK Reference
nav_order: 3
permalink: /sdk/node-kinds/
---

# Node Kinds

Node kinds define execution roles within the graph.

Each kind establishes:

- Port configuration
- Expected inputs
- Expected outputs
- Runtime behavior category

---

## Source Nodes

Produce textures without upstream dependencies.

Examples:

- Video decode nodes
- Webcam input
- Procedural generators

Guarantee: At least one output texture per frame (unless external failure).

---

## Processing Nodes

Transform input textures to a new output.

Examples:

- ShaderPass
- Color transforms
- Post-processing effects

Guarantee: Pure transformation given deterministic shader and inputs.

---

## Mixer Nodes

Combine multiple textures into a single output.

Characteristics:

- Fixed input count per node type
- Semantics defined by shader
- Parameters supplied via `NodeProps`

---

## Output Nodes

Terminal nodes that write results to:

- Framebuffers
- Named outputs
- Readback buffers

They do not produce further graph outputs.

---

## Extensibility Guidance

Prefer:

- New shaders
- New configuration parameters

Avoid:

- Introducing new node kinds unless execution semantics require it

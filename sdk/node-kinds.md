---
layout: default
title: Node Kinds
parent: SDK
nav_order: 4
permalink: /sdk/node-kinds/
---

# Node Kinds

Node kinds define execution roles within a compiled graph. They classify how a node participates in topology and evaluation. Behavior is still determined by shader code and `NodeProps`.

- Node kinds do not alter execution order.  
- Ordering is determined by graph topology.

---

## Source Nodes

Source nodes produce textures without upstream dependencies.They have no required input ports.

Examples:

- Video decode nodes
- Capture inputs
- Procedural generators

Characteristics:

- At least one output port
- No required input connections
- Output produced once per frame (unless upstream resource failure)

---

## Processing Nodes

Processing nodes transform one or more input textures into a new output texture.

Examples:

- Shader passes
- Color transforms
- Post-processing stages

Characteristics:

- Require input connections
- Produce at least one output
- Deterministic given identical inputs, `NodeProps`, and shaders

---

## Mixer Nodes

Mixer nodes combine multiple input textures into a single output.

Characteristics:

- Fixed input count per node type
- Combination semantics defined by shader
- Parameters supplied via `NodeProps`
- Deterministic given identical inputs and configuration

---

## Output Nodes

Output nodes terminate the graph.

They write results to:

- Framebuffers
- Named outputs
- Readback buffers

Characteristics:

- May consume inputs
- Do not produce downstream outputs
- Represent terminal graph boundaries

---

## Design Constraints

Node kinds describe structural roles only. These responsibilities belong to `scheng-graph`, `scheng-runtime`, and backend implementations.

They do not:

- Control execution timing
- Modify topology during evaluation
- Manage GPU resources
- Introduce scheduling behavior

---

## Extensibility Guidance

Avoid introducing new node kinds unless execution semantics require a distinct structural role. Prefer extending behavior through:

- New shaders
- Additional `NodeProps` configuration

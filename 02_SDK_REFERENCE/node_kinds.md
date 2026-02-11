# Node Kinds

This document lists the conceptual categories of node kinds in ShadeCore.

Each node kind defines:
- Port layout
- Execution behavior
- Runtime expectations

---

## Source Nodes

Source nodes generate data.

Examples:
- Video decode sources
- Webcam inputs
- External texture inputs

Source nodes do not consume upstream inputs.

---

## Processing Nodes

Processing nodes transform data.

Examples:
- ShaderPass
- Mixer nodes (e.g. MatrixMix4)

They typically:
- Consume one or more textures
- Produce a single output texture

---

## Mixer Nodes

Mixer nodes combine multiple inputs.

- Number of inputs is fixed per node kind
- Semantics are shader-defined
- Weights and parameters are externally supplied

MatrixMix4 is the canonical mixer.

---

## Output Nodes

Output nodes terminate execution.

Examples:
- PixelsOut

They typically:
- Do not produce outputs
- Write to framebuffers or readback buffers

---

## Extensibility

Adding new node kinds requires engine changes.

Most new behavior should be implemented via:
- New shaders
- Existing node kinds

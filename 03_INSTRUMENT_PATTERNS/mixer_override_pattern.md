# Mixer Override Pattern

This document describes how to override mixer behavior using shaders,
with `MatrixMix4` as the canonical example.

The goal is to change **how forms are combined** without adding new node kinds.

---

## Why Override Mixers?

Default mixers often perform simple linear blends.

In expressive video instruments, you may want:

- Luma keys
- Chroma keys
- Nonlinear blends
- Edge-based mixes
- Feedback-style combinations

All of these can be implemented as custom GLSL in a mixer node.

---

## Core Idea

1. Use an existing mixer node (e.g. `MatrixMix4`)
2. Provide a custom fragment shader via `NodeProps.shader_sources`
3. Drive parameters (e.g. `uWeights`) from `MatrixMixParams`
4. Treat inputs as **forms**, not RGB channels

The engine still manages inputs, outputs, and execution order.

---

## Inputs and Uniforms

A typical `MatrixMix4` override shader receives:

- Up to 4 input textures (forms)
- A weight vector (e.g. `uWeights`)

Example conceptual mapping:

- Input 0: webcam
- Inputs 1–3: GLSL layers
- `uWeights.x`: key low threshold
- `uWeights.y`: key high threshold
- `uWeights.z`: camera gain
- `uWeights.w`: background gain

The exact meaning of each weight is up to the shader.

---

## Wiring Pattern

At the graph level:

- Route your sources into the mixer inputs
- Use `MatrixMix4` as the combining node
- Route mixer output to `PixelsOut` or another pass

At the NodeProps level:

- Assign a custom fragment shader to the mixer node
- Provide `MatrixMixParams` with weights
- Optionally expose weights via OSC

---

## Example Behaviors

Some common behaviors implemented via mixer overrides:

- **Luma key:** use brightness from one input to decide blend
- **Soft wipe:** use a spatial function (e.g. gradient) as mix factor
- **Glitch blend:** sample inputs with offsets or noise before mixing
- **Form switching:** hard switch between inputs based on thresholds

All these behaviors are defined in GLSL only.

---

## Advantages

- No new node kinds required
- Fully controlled by shaders
- Works with existing execution and resource models
- Encourages treating inputs as independent worlds

---

## Design Principles

- Mixer nodes define **topology**, shaders define **semantics**
- Parameters are generic (weights), meaning is instrument-specific
- Overrides are preferred to engine-level special cases

# Shader Contract

This document defines the **contract between the ShadeCore runtime and GLSL shaders**.

Shaders are treated as opaque GPU programs. The engine does not inspect shader logic,
but it guarantees a stable execution environment and uniform interface.

---

## Shader Types

ShadeCore supports shaders in the following contexts:

- Fullscreen shader passes (`ShaderPass`)
- Mixer shaders (e.g. `MatrixMix4` override shaders)

Each shader type has a defined set of inputs and uniforms.

---

## Required Structure

Fragment shaders must:

- Compile under GLSL 330 core (or compatible target)
- Write a final color to the output variable
- Use engine-provided samplers and uniforms consistently

Vertex shaders are typically provided by the engine (fullscreen triangle),
but may be overridden explicitly.

---

## Standard Uniforms

The runtime guarantees the following uniforms when applicable:

- `uniform vec2 uResolution;`
- `uniform float uTime;`
- `uniform int uFrame;`

These values are sourced from `FrameCtx`.

---

## Sampler Bindings

Shader passes receive input textures via named samplers.

Common conventions:

- `sampler2D uTex0`
- `sampler2D uTex1`
- ...

The exact mapping depends on the node kind and graph wiring.

The engine guarantees stable binding order for a given node type.

---

## Mixer Shaders

Mixer nodes (e.g. `MatrixMix4`) additionally receive:

- `uniform float uWeights[4];`

The meaning of these weights is **entirely shader-defined**.

The engine does not impose RGB semantics or linear mixing rules.

---

## Custom Uniforms

Shaders may declare additional uniforms.

Values must be supplied by the instrument via `NodeProps`.
The engine does not provide defaults for custom uniforms.

---

## Compilation and Lifetime

- Shaders are compiled lazily
- Compilation errors are surfaced at runtime
- Programs are cached per node

Shader recompilation occurs only when source changes.

---

## Design Principles

- Shaders define behavior
- The engine defines execution order
- Mixing semantics are intentional and explicit

Shaders are first-class instruments, not implementation details.

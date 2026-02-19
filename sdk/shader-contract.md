---
layout: default
title: Shader Contract
parent: SDK
nav_order: 6
permalink: /sdk/shader-contract/
---

# Shader Contract

The Shader Contract defines the interface between `scheng-runtime` and GLSL programs. Shaders provide visual behavior. The runtime provides execution context and resource binding.

**Subject to change*

---

## Engine Guarantees

During execution, the runtime guarantees:

- A valid and current GL context
- Bound input textures in deterministic order
- Standard uniforms:
  - `uResolution`
  - `uTime`
  - `uFrame`
- Correct framebuffer binding for output

Sampler ordering is stable and corresponds to graph port ordering.

The runtime handles:

- Program compilation
- Uniform binding
- Texture binding
- Draw dispatch

---

## Shader Responsibilities

Shaders must:

- Compile under the supported GLSL version
- Declare required samplers and uniforms
- Write a valid output color
- Respect provided sampler bindings

Shaders must not assume:

- Hidden engine state
- Implicit uniforms
- Dynamic topology changes

Behavior must be fully determined by:

- Input textures
- Provided uniforms
- `NodeProps` configuration

---

## Custom Uniforms

Custom uniforms are supplied via `NodeProps`.

The runtime reads values from `NodeProps` and sets them before dispatch.

If a shader declares a uniform that is not provided via `NodeProps`, the result is undefined.

Structural changes to uniforms require recompilation.

---

## Compilation Model

Shaders are:

- Compiled lazily on first use
- Cached per node instance
- Recompiled only when shader source changes

Compilation failures are returned to the caller immediately.

---

## Execution Boundary

The runtime controls:

- Execution order
- Resource lifetime
- Frame boundaries

Shaders control:

- Pixel-level visual behavior

The engine does not interpret shader semantics.

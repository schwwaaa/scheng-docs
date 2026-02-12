---
layout: default
title: Shader Contract
parent: SDK Reference
nav_order: 8
permalink: /sdk/shader-contract/
---

# Shader Contract

Defines the interface between runtime and GLSL programs.

---

## Engine Guarantees

The runtime guarantees:

- Valid GL context
- Bound textures
- Standard uniforms:
  - `uResolution`
  - `uTime`
  - `uFrame`
- Deterministic sampler ordering

---

## Shader Responsibilities

Shaders must:

- Compile under supported GLSL version
- Write valid output color
- Respect provided sampler bindings

---

## Custom Uniforms

Must be supplied via `NodeProps`.

Missing values may produce undefined results.

---

## Compilation Model

- Compiled lazily
- Cached per node
- Recompiled only when source changes

---

## Design Boundary

Engine controls execution.  
Shaders control visual behavior.

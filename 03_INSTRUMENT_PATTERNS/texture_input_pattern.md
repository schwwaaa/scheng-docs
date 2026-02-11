# Texture Input Pattern

This document describes the pattern for using external textures
(such as webcams) in a ShadeCore instrument via texture input nodes.

---

## Goals

- Treat external sources as **forms** like any other
- Keep GL texture ownership external to the engine
- Use a stable node interface for sampling

---

## Core Components

A texture input pattern includes:

- An external producer (e.g. webcam capture)
- A GL texture managed by the instrument
- A texture input node in the graph
- Shader passes sampling that texture

The engine only sees a handle and a node ID.

---

## Graph Wiring

At the graph level:

1. Add a texture input node
2. Connect it to one or more shader passes

The node exposes a texture to downstream nodes.
It does not create or manage the texture itself.

---

## NodeProps Configuration

The instrument writes the external texture handle into `NodeProps`:

- Map node ID to a texture handle
- Update as needed when the source changes

The runtime uses this handle when executing the graph.

---

## Shader Usage

Downstream shaders sample the input texture like any other sampler:

- Declare a `sampler2D` uniform
- Sample using UV coordinates

The semantics of the texture (webcam, feedback, etc.) are instrument-defined.

---

## Lifetime and Ownership

The instrument owns:

- Texture creation
- Texture updates (uploading new frames)
- Destruction when shutting down

The engine assumes the texture is valid for the duration of execution.

---

## Design Principles

- External textures are first-class forms
- Graph and runtime do not manage capture devices
- Texture input nodes are a bridge between host and engine

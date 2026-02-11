# Adding External Inputs

This guide explains how to integrate external sources such as webcams or video files.

---

## Ownership Model

External inputs are owned by the instrument.

The engine only samples textures.

---

## Integration Steps

1. Acquire or create a GL texture
2. Update texture contents externally
3. Bind texture to a texture input node
4. Sample downstream in shaders

---

## Lifetime Rules

- Texture must remain valid while in use
- Updates must occur before execution
- Destruction occurs on shutdown

---

## Design Principle

The engine does not manage devices.

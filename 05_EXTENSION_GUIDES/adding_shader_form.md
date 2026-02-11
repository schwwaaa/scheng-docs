# Adding a New Shader Form

This guide explains how to add new GLSL shader forms to an instrument.

---

## Where Shaders Live

Shaders may live:
- Inline (for small experiments)
- As external `.glsl` files (preferred for reuse)

---

## Shader Expectations

Shaders should:
- Compile under GLSL 330 core
- Treat inputs as abstract forms
- Avoid hard-coded color semantics

---

## Wiring a Shader

1. Create a ShaderPass node
2. Assign shader source via `NodeProps.shader_sources`
3. Connect inputs explicitly

---

## Parameterization

Use uniforms or mixer weights to expose control.

Do not:
- Assume linear blending
- Assume RGB meaning
- Bake constants unnecessarily

---

## Design Principle

Shaders are instruments, not effects.

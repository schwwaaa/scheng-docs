# NodeProps Reference

`NodeProps` is the primary configuration surface of the ShadeCore SDK.

All per-node configuration is supplied externally via `NodeProps`. The engine reads from
this structure during execution and does not mutate it.

This design ensures:
- Deterministic execution
- Clear separation between engine and instrument logic
- No hidden state inside nodes

---

## Role of NodeProps

`NodeProps` maps **node IDs** to configuration values.

It is used to:
- Attach shaders to shader-capable nodes
- Provide parameters to mixers
- Configure input sources (video files, webcams, textures)
- Name outputs

Nodes do not own configuration internally.

---

## Shader Sources

Shader-capable nodes (e.g. `ShaderPass`, `MatrixMix4`) read shader source code from:

- `shader_sources: HashMap<NodeId, ShaderSource>`

Each `ShaderSource` contains:
- Vertex shader source
- Fragment shader source
- Optional origin/debug string

Shaders are compiled lazily by the runtime.

---

## Mixer Parameters

Mixers such as `MatrixMix4` read parameters from:

- `matrix_params: HashMap<NodeId, MatrixMixParams>`

These parameters are typically mapped to GLSL uniforms such as `uWeights`.

The engine does not interpret mixing semantics; it simply forwards values.

---

## Video Decode Configuration

Video decode nodes read configuration from:

- `video_decode_json: HashMap<NodeId, PathBuf>`

The JSON file defines:
- Video path
- Decode behavior
- Looping and timing options

This keeps decoder configuration out of code.

---

## External Texture Inputs

Texture input nodes read from:

- `texture_inputs: HashMap<NodeId, TextureHandle>`

This is commonly used for:
- Webcam textures
- External GL textures
- Interop with other systems

Ownership remains external to the engine.

---

## Output Naming

Outputs can be explicitly named via:

- `output_names: HashMap<NodeId, String>`

This is required when:
- A graph has multiple outputs
- Outputs need to be addressed deterministically

Certain names may be reserved by the runtime.

---

## Design Guarantees

- NodeProps is read-only during execution
- Missing entries fall back to node defaults
- All configuration is explicit
- No side effects occur when updating NodeProps between frames

---

## Summary

If you want to change behavior without modifying engine code,
you should do it through `NodeProps`.

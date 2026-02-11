# Matrix Mix 4 + Webcam (OSC)

**Example crate:** `shadecore-example-graph-matrix-mix4-webcam`

---

## What This Example Is

A four-input form mixer controlled entirely via OSC.

Inputs typically include:
- Webcam
- Three GLSL shader forms

---

## Signal Flow

```
Webcam -> ShaderPass ->
Shader A ------------->
Shader B -------------> MatrixMix4 -> PixelsOut
Shader C ------------->
```

---

## Controls (OSC)

- `/param/w0`
- `/param/w1`
- `/param/w2`
- `/param/w3`

Each controls the contribution of one form.

---

## Key Pattern Demonstrated

- Mixer nodes as semantic combiners
- OSC-driven parameter updates
- Treating all inputs as equal forms

---

## Run Command

```
cargo run -p shadecore-example-graph-matrix-mix4-webcam -- --bind 127.0.0.1:9000
```

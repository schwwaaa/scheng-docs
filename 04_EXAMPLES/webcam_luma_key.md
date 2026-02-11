# Webcam Luma Key Instrument

**Example crate:** `shadecore-example-graph-webcam-luma-key`

---

## What This Example Is

A webcam luma-key instrument built on a custom MatrixMix4 shader.

The webcam is keyed *through* three GLSL shader forms.

---

## Signal Flow

```
Webcam -> Passthrough ->
Shader A ------------->
Shader B -------------> MatrixMix4 (luma key) -> PixelsOut
Shader C ------------->
```

---

## Controls (OSC)

- `/param/key_low`
- `/param/key_high`
- `/param/cam_gain`
- `/param/bg_gain`

Aliases:
- `w0` → key_low
- `w1` → key_high
- `w2` → cam_gain
- `w3` → bg_gain

---

## Key Pattern Demonstrated

- Mixer override via custom GLSL
- Nonlinear mixing semantics
- OSC as a live performance surface

---

## Run Command

```
cargo run -p shadecore-example-graph-webcam-luma-key -- --bind 127.0.0.1:9001
```

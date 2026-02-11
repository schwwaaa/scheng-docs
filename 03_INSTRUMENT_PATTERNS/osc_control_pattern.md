# OSC Control Pattern

This document describes the standard pattern for integrating OSC into a ShadeCore instrument.

OSC is treated as an **external control surface** that feeds values into:
- Local instrument state
- `NodeProps` configuration
- Shader parameters (via uniforms or mixer weights)

The engine itself remains unaware of OSC.

---

## Goals

- Keep OSC handling **localized** to the instrument
- Avoid protocol-specific code in engine crates
- Make control mappings easy to log, debug, and change

---

## Basic Structure

A typical OSC-enabled instrument:

1. Parses a `--bind` address from CLI (e.g. `127.0.0.1:9000`)
2. Binds an OSC receiver to that address
3. On each frame:
   - Polls for incoming messages
   - Applies updates to local parameters
   - Writes updated values into `NodeProps`

---

## Address Convention

Examples use a simple, flat addressing scheme:

- All control messages are sent to `/param/<name>`
- Values are `f32`

Examples:

- `/param/w0 0.5`
- `/param/key_low 0.2`
- `/param/cam_gain 1.5`

The receiver strips `/param/` and yields `(name, value)`.

This keeps the mapping string-based and decoupled from node IDs.

---

## Mapping Strategy

Inside the instrument, map OSC names to parameters explicitly:

- Use a match statement or small lookup table
- Normalize / clamp values as needed
- Immediately update the corresponding NodeProps or local state

Example pattern (pseudo-Rust):

```text
for (name, value) in osc.poll() {
    match name.as_str() {
        "w0" | "key_low" => params.key_low = value,
        "w1" | "key_high" => params.key_high = value,
        "w2" | "cam_gain" => params.cam_gain = value,
        "w3" | "bg_gain"  => params.bg_gain  = value,
        _ => {}
    }
}
```

Then write the final values into `NodeProps` (e.g. mixer weights).

---

## Logging

Instruments should log OSC activity:

- When a message is received
- When a parameter is updated

Example log style:

```text
[osc] received key_low = 0.200000
[osc] applied -> weights = [0.200, 0.800, 1.500, 1.000]
```

This is essential for live debugging and performance use.

---

## Multiple Instruments

If running multiple instruments at once:

- Use different `--bind` ports
- Keep parameter naming consistent where possible
- Reuse the same OSC controller layout

This lets you “point” a single controller at different instruments by switching targets.

---

## Design Principles

- OSC is optional and external
- All control mappings live in the instrument layer
- Parameters are name-based, not node-based
- Logging is required for practical live use

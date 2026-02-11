# OSC Contract

This document defines the **OSC control contract** used by ShadeCore examples.

OSC support is intentionally lightweight and external to the engine.

---

## Role of OSC

OSC is used to:

- Inject control values into instruments
- Modify parameters in real time
- Drive shaders and mixers dynamically

The engine itself does not parse OSC messages.

---

## Receiver Model

OSC receivers are instantiated at the example or application level.

Typical flow:

1. Bind a UDP socket
2. Poll for messages each frame
3. Map messages to parameters
4. Write values into `NodeProps` or local state

---

## Addressing Convention

Examples use the following convention:

- OSC messages are sent to `/param/<name>`
- Values are single floats

Examples:

- `/param/w0 0.5`
- `/param/key_low 0.25`
- `/param/cam_gain 1.2`

The receiver strips `/param/` and yields `(name, value)`.

---

## Typing

- All parameters are treated as `f32`
- No implicit scaling or clamping is performed by the engine
- Instruments are responsible for validation

---

## Update Semantics

- OSC messages may arrive at any time
- Polling is explicit
- The most recent value wins

No buffering or interpolation is performed by default.

---

## Logging and Observability

Examples are expected to log:

- Receipt of OSC messages
- Parameter updates

This is critical for live debugging and performance use.

---

## Design Rationale

Keeping OSC external ensures:

- No protocol lock-in
- No hidden engine state
- Maximum flexibility for instruments

OSC is a control surface, not a dependency.

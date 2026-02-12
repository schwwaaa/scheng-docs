---
layout: default
title: Frame Context (`FrameCtx`)
parent: SDK Reference
nav_order: 1
permalink: /sdk/frame-ctx/
---

# Frame Context (`FrameCtx`)

`FrameCtx` defines the complete, immutable execution context for a single frame of graph execution.

It is the only global, per-frame input provided to the runtime.

---

## Role in the System

`FrameCtx` establishes:

- Render dimensions
- Temporal position
- Frame identity

The runtime does not maintain internal time, frame counters, or implicit scheduling.  
All temporal semantics originate from `FrameCtx`.

---

## Data Structure

```rust
pub struct FrameCtx {
    pub width: u32,
    pub height: u32,
    pub time: f32,
    pub frame: u64,
}
```

---

## Field Semantics

### width / height

Defines the target render resolution for the frame.

Effects:

- Sets viewport dimensions
- Propagates to shader uniform `uResolution`
- Determines render target allocation size

Changing resolution between frames is allowed but may cause internal resource reallocation.

---

### time

Floating-point time value in seconds.

The runtime:

- Forwards this value to shaders (`uTime`)
- Does not interpret or constrain it
- Does not enforce monotonicity

Valid patterns:

- Monotonic wall-clock progression
- Manual scrubbing
- Looping time domains
- Discontinuous jumps

---

### frame

Unsigned frame index.

The runtime:

- Forwards this value to shaders (`uFrame`)
- Does not enforce increment rules
- Does not derive timing from it

Used for deterministic frame identity and debugging.

---

## Guarantees

- Immutable during execution
- Stable for the duration of a frame
- No hidden time accumulation in the runtime
- No implicit scheduler dependency

---

## Failure Modes

Improper usage may result in:

- Shader logic misbehavior (if time assumptions are violated)
- Resource churn (if resolution fluctuates excessively)
- Non-deterministic results (if frame/time values are inconsistent)

---

## Design Rationale

Separating frame context from runtime state ensures:

- Deterministic execution
- Replayability
- Offline rendering capability
- Explicit time control

The engine never owns time.

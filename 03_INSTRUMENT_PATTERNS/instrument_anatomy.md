# Instrument Anatomy

This document describes the standard structure of a ShadeCore instrument.

An instrument is an application-level composition built from:
- A graph
- Node configuration
- Control inputs (OSC, keyboard, etc.)
- A render loop

---

## High-Level Structure

A typical instrument follows this sequence:

1. Create a window and GL context
2. Build a graph of nodes
3. Compile the graph into a plan
4. Populate NodeProps
5. Enter the event/render loop
6. Update parameters
7. Execute the plan

---

## Graph Construction

The graph defines signal flow only.

It should:
- Be minimal
- Avoid conditional structure
- Represent processing topology, not behavior

Behavior belongs in shaders and control logic.

---

## Configuration via NodeProps

All runtime configuration happens via `NodeProps`.

Examples:
- Assign shaders
- Set mixer weights
- Provide video config paths
- Inject webcam textures

NodeProps may be updated every frame.

---

## Control Layer

Instruments typically include a control layer:

- OSC receivers
- Keyboard input
- MIDI or other external sources

Controls modify:
- NodeProps values
- Time passed into FrameCtx

The engine remains unaware of control protocols.

---

## Time and Transport

Time is external.

An instrument may:
- Use wall-clock time
- Pause or scrub
- Loop
- Override time entirely

This is achieved by controlling `FrameCtx.time`.

---

## Rendering

Rendering occurs by calling:

```
execute_plan_outputs(...)
```

The engine:
- Executes all nodes
- Produces output textures

The instrument:
- Clears buffers
- Presents results
- Handles window events

---

## Observability

Instruments should log:
- Control input reception
- Parameter updates
- Mode changes

This is critical for live systems.

---

## Summary

An instrument is not a node or a shader.

It is the **composition of graph, control, and execution**.

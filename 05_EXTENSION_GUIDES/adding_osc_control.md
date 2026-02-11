# Adding OSC Control to an Instrument

This guide explains how to add OSC control to any ShadeCore example.

---

## Step 1: Add CLI Binding

Parse a `--bind` argument to specify the OSC address.

---

## Step 2: Create Receiver

Instantiate an OSC receiver at startup.

---

## Step 3: Poll Per Frame

On each frame:
- Poll for messages
- Apply updates immediately
- Log received values

Receiving OSC alone does nothing.

---

## Step 4: Apply to Parameters

Map OSC names to:
- Mixer weights
- Shader uniforms
- Transport state

---

## Design Principle

OSC is a control surface, not a dependency.

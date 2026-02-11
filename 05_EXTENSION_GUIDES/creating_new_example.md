# Creating a New Example

This guide explains how to create a new ShadeCore example correctly.

Examples are **reference instruments**, not demos or tests.

---

## Step 1: Choose a Base Example

Always start by copying an existing example that is closest to your goal.

Good bases:
- `video_scrub_keyboard_transport`
- `graph_matrix_mix4_webcam`
- `graph_webcam_luma_key`

Avoid starting from minimal examples unless debugging.

---

## Step 2: Duplicate the Example

1. Copy the example directory
2. Rename the crate and folder
3. Update:
   - `Cargo.toml` name
   - Binary name
   - Window title

Do not modify engine crates.

---

## Step 3: Modify the Graph

You may:
- Add or remove nodes
- Rewire connections
- Swap shader sources

You may NOT:
- Change node kinds
- Change runtime behavior
- Invent new engine APIs

---

## Step 4: Expose Controls

Every interactive example should expose control via:
- Keyboard and/or
- OSC

OSC is preferred for continuous control.

---

## Step 5: Log Everything

Examples must log:
- Control input
- Parameter changes
- Runtime errors

If you cannot see it in logs, it does not exist.

---

## Design Principle

Examples are meant to be:
- Copied
- Modified
- Broken
- Rebuilt

They are learning artifacts, not abstractions.

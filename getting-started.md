---
layout: default
nav_order: 3
permalink: /getting-started/
title: Getting Started
---

# Getting Started

This page walks you through setting up a local environment and running
your first scheng instrument.

------------------------------------------------------------------------

## Audience

Use this page if you want to:

-   Build scheng from source
-   Run the included example instruments
-   Confirm your environment is ready before diving into Guides and
    Patterns

------------------------------------------------------------------------

## Prerequisites

Required:

-   Rust toolchain (stable)
-   Cargo (installed with Rust)
-   Git
-   A GPU with modern OpenGL support
-   Up-to-date graphics drivers

Verify Rust and Cargo:

``` bash
rustc --version
cargo --version
```

------------------------------------------------------------------------

## Clone the Repository

``` bash
git clone <repo-url>
cd scheng
```

Replace `<repo-url>` with the actual Git repository URL.

------------------------------------------------------------------------

## Build the Workspace

``` bash
cargo build --workspace
```

A successful build confirms:

-   All dependencies resolve
-   The workspace compiles cleanly
-   Core crates build without errors

If this step fails, resolve build errors before proceeding.

------------------------------------------------------------------------

## Run an Example Instrument

Example:

``` bash
cargo run -p shadecore-example-minimal
```

On success:

-   A window opens
-   Frames render
-   No runtime panics occur

------------------------------------------------------------------------

## Validate Runtime Behavior

While running:

-   Confirm stable frame updates
-   Test any mapped controls (keyboard, OSC, etc.)
-   Check terminal output for warnings or shader errors

------------------------------------------------------------------------

## Troubleshooting

### Build Errors

``` bash
cargo clean
cargo build --workspace
```

Ensure you are using the stable Rust toolchain.

### Blank or Black Window

-   Check for shader compile/link errors in the terminal
-   Verify GPU compatibility
-   Confirm required assets are present

------------------------------------------------------------------------

## Next Steps

After a successful run:

1.  Review the Project Overview (home page)
2.  Explore Examples
3.  Read Guides for structured workflows

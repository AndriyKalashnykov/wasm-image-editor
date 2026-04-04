# CLAUDE.md

## Project Overview

WebAssembly image editor written in Rust. Applies real-time image filters (grayscale, sepia, color channels, opacity) via WASM in the browser, with JavaScript equivalents for comparison.

## Tech Stack

- **Language:** Rust (2021 edition), compiled to `wasm32-unknown-unknown`
- **Build target:** WebAssembly (`cdylib` crate type)
- **Frontend:** Vanilla HTML/CSS/JS (no framework)
- **Build tools:** Cargo, Make, cargo-server (local dev server)

## Build & Run

```bash
# Prerequisites
cargo binstall cargo-watch cargo-server

# Release build + serve
make build

# Debug build (no server)
make build-debug

# Watch mode (rebuild on change)
make watch

# Lint
make lint
```

## Project Structure

```
src/lib.rs          # All WASM filter functions (grayscale, sepia, red, blue, green, opacity)
index.html          # Single-page app entry point
resources/app.js    # JS glue code and JS-based filters
resources/app.css   # Styles
resources/editor.wasm  # Compiled WASM binary (checked in)
.cargo/config       # Default build target: wasm32-unknown-unknown
```

## Key Conventions

- The `.cargo/config` sets the default build target to `wasm32-unknown-unknown`, so plain `cargo build` produces WASM output.
- The compiled `.wasm` file is committed to `resources/` for zero-build deployment.
- Filter functions are `#[no_mangle]` exports called directly from JS via the WebAssembly API.
- No npm/node dependencies; the frontend is pure vanilla JS.
- Code formatting enforced via `cargo fmt --check` (the `make lint` target).

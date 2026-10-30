---
name: ch32fun
description: Work with WCH RISC-V firmware via ch32fun and minichlink.
version: 0.1.0
author: Hermes Agent
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes.tags:
    - Embedded
    - RISC-V
    - WCH
    - minichlink
  hermes.related_skills: []
---

# ch32fun

Knowledge base for the `cnlohr/ch32fun` repo: minimal RISC-V firmware stack, headers, examples, and `minichlink` tooling for WCH MCUs. Distilled from the README and repo structure. Does not replace the repo README or official docs.

## When to Use
- Questions about ch32fun repo layout, chip support, or build flow.
- Need for exact build, flash, or debug commands with `minichlink`/`make`/PlatformIO.
- Quick-reference on SWIO pin constraints or printf-debug throughput.

## Repo Structure
- `ch32fun/` — single-header/source core for compiling apps.
- `minichlink/` — cross-platform programmer/debugger using libusb.
- `examples/`, `examples_ch5xx/`, `examples_h41x/`, `examples_l103/`, `examples_usb/`, `examples_v10x/`, `examples_v20x/`, `examples_v30x/`, `examples_x00x/`, `examples_x035/` — per-family examples.
- `build_scripts/` — make/clangd helpers.
- `extralibs/` — optional extra libraries.
- `misc/` — bundled `libgcc.a` and utilities.
- `platformio.ini` — PlatformIO environments.

## Reference Files
Load on demand with `skill_view(name='ch32fun', file_path='...')`:
- `references/chip-support.md` — chip families and known-good list.
- `references/programming-debug.md` — minichlink, SWIO pins, printf, gdbserver.
- `references/build-flash.md` — make, PlatformIO, build artifacts, clangd.
- `references/constraints.md` — no-HAL philosophy, no malloc/free, design rules.
- `references/third-party.md` — related repos and libraries.

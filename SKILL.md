---
name: ch32fun
description: Work with WCH RISC-V firmware via ch32fun, minichlink, and bare-metal cooperative scheduling patterns.
version: 0.1.0
author: Aishee Mitra (aishee-mitra) & Sayan Chakrabarti (sayanriju)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [embedded, risc-v, wch, ch32v003, minichlink, ch32fun, cooperative-scheduler]
    related_skills: []
---

# ch32fun

A comprehensive skill and reference guide for working with the `cnlohr/ch32fun` repository: a minimal, bare-metal RISC-V firmware stack, headers, examples, and `minichlink` tooling for WCH microcontrollers (including CH32V003, CH32X035, CH32V10x-V30x, CH5xx).

## When to Use

- Developing bare-metal C/C++ firmware using `ch32fun` on WCH RISC-V MCUs.
- Configuring and building projects via `make` or PlatformIO.
- Flashing and debugging firmware with `minichlink` or GDB.
- Designing periodic tasks or cooperative scheduling patterns without RTOS/dynamic allocation overhead.
- Looking up chip capabilities, SWIO pin assignments, or architectural constraints (no-HAL, no malloc).

## Repo Structure

- `ch32fun/` — single-header/source core for compiling applications.
- `minichlink/` — cross-platform programmer/debugger using libusb.
- `examples/` — family and peripheral example projects.
- `build_scripts/` — make and clangd compilation helpers.
- `extralibs/` — optional community libraries.
- `misc/` — bundled utilities and `libgcc.a`.
- `platformio.ini` — PlatformIO environments and configurations.

## Reference Files

Detailed references available on demand:

- `references/coop-scheduler.md` — Cooperative non-preemptive periodic task scheduler using SysTick (`PeriodicJob`), millis counter, and zero-allocation callbacks.
- `references/build-flash.md` — Make/PlatformIO build setup, artifact sizing, and clangd / compile_commands config.
- `references/chip-support.md` — Chip family matrix (CH32V003 primary target, V103-V307, X035, CH5xx).
- `references/programming-debug.md` — `minichlink` usage, SWIO hardware pins (PD1), terminal fast printf (~36 KB/s), and gdbserver.
- `references/constraints.md` — Bare-metal philosophy (no HAL, direct TRM register access, minimal ~500B footprint, alternatives to malloc/free).
- `references/third-party.md` — Community libraries, hardware drivers, and official processor/debug manuals.

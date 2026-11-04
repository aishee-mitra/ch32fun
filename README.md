# ch32fun-skill

[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-skills.sh-blue)](https://skills.sh)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

An open [Agent Skills](https://agentskills.io/) and [skills.sh](https://skills.sh) compatible repository providing comprehensive knowledge and patterns for developing bare-metal RISC-V firmware with [`cnlohr/ch32fun`](https://github.com/cnlohr/ch32fun) and WCH microcontrollers (CH32V003, CH32X035, CH32V10x-V30x, CH5xx).

## Included Skills

- **`ch32fun`**: Bare-metal WCH RISC-V development guide covering:
  - Repository structure and build systems (`make`, PlatformIO, `clangd`).
  - Flashing and debugging with `minichlink`, SWIO protocol, and fast printf debugging.
  - Zero-heap cooperative periodic task scheduling (`PeriodicJob` via SysTick).
  - Chip support matrix and architecture constraints (no-HAL, direct register manipulation, memory footprint limits).

## Installation

### Using `skills.sh` / `npx skills`

Install globally or into your agent's workspace:

```bash
# Global install (Claude Code, Cursor, Windsurf, Codex, etc.)
npx skills add aishee-mitra/ch32fun -g

# Or install for a specific agent/project
npx skills add aishee-mitra/ch32fun
```

### Using Hermes Agent

Install directly via Hermes Skills Hub:

```bash
# Direct GitHub install
hermes skills install aishee-mitra/ch32fun/skills/ch32fun

# Or add this repository as a tap
hermes skills tap add aishee-mitra/ch32fun
hermes skills install ch32fun
```

Inside a Hermes chat session:
```text
/skills install aishee-mitra/ch32fun/skills/ch32fun
```

## Structure

```text
.
├── skills.sh.json              # skills.sh catalog category grouping
├── README.md
├── LICENSE
└── skills/
    └── ch32fun/
        ├── SKILL.md            # Main entry point & frontmatter
        └── references/
            ├── coop-scheduler.md   # SysTick periodic cooperative job scheduler pattern
            ├── build-flash.md      # Toolchains, make, platformio, clangd
            ├── chip-support.md     # Chip family matrix & specs
            ├── constraints.md      # Architectural principles & memory constraints
            ├── programming-debug.md# minichlink, fast SWIO printf, gdb
            └── third-party.md      # Ecosystem libraries and hardware drivers
```

## License

[MIT](LICENSE)

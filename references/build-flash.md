# Build, Flash, and Editor Setup

Source: `cnlohr/ch32fun` README.

## Make Build
```bash
cd examples/blink
make
```

## Build Artifacts
- `.bin` — used by `minichlink` for flashing.
- `.hex` — compatible with the official WCH flash tool.
- `text` = code, `data` = constants/init values, `bss` = uninitialized.
- `dec` = sum of the above; flash footprint in bytes.

## PlatformIO
- New project wizard: Framework = `ch32v003fun`.
- PlatformIO Core CLI:
  ```sh
  pio init -b genericCH32V003F4P6 -O "framework = ch32v003fun"
  ```
- VS Code: open repo in VSCode, use project environment switcher to select one example folder.
- To add examples directly, expand environments in `platformio.ini` per README-linked instructions.

## clangd
- If `ch32fun.h` is unresolved, run `make clangd` or `bear -- make build` to generate `compile_commands.json`.
- `build_scripts/build_all_clangd.sh` does this across examples.

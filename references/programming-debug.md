# Programming and Debugging

Source: `cnlohr/ch32fun` README.

## minichlink
- Cross-platform programmer/debugger using WCH CH-Link via libusb.
- Supported programmers:
  - STM32F042 Programmer (`NHC-Link042`)
  - ESP32S2 Programmer (`esp32s2-funprog`)
  - Official WCH Link-E
  - Ardulink (Arduino-based)
  - rv003usb SWDIO programmer
  - Built-in USB ISP bootloader on USB-equipped WCH chips

## Fast printf Debug
- Call `SetupDebugPrintf();` then `printf()` over SWIO.
- Typical throughput: ~36 KB/s.
- Example: `./minichlink -T | pv > /dev/null`

## GDB / gdbserver
- `minichlink` includes gdbserver; compatible with `gdb-multiarch` and VS Code.

## Pin Constraints
- **SWIO**: `PD1` — REQUIRED. Do not reassign.
- **NRST**: `PD7` — optional; defaults to GPIO in some configs.
- **UART TX**: `PD5` — optional; SWIO preferred for printf debug.

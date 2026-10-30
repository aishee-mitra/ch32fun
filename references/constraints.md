# Design Constraints and Philosophy

Source: `cnlohr/ch32fun` README.

## No HAL
- The project explicitly avoids WCH's HAL/EVT. Use the Technical Reference Manual (TRM) directly.

## Minimal Footprint
- blinky can be ~500 bytes with ch32fun.
- Faster boot, simpler control flow than HAL-based setups.

## malloc/free
- Not provided in `ch32fun`.
- Recommended alternatives: static allocation, fixed-size lists, tinyalloc, libmemory, mameMalloc.
- Chips range from 2 KB RAM to 128 KB RAM; the right allocator depends on target.

## Coding Style
- Follow repo conventions; discussion happens in the project's Discord.
- External contributions receive greater scrutiny.
- Contributions must be the submitter's own work; tool-generated code is not accepted.
- Accepted contributions expect basic support from the submitter.

# Chip Support Matrix

Source: `cnlohr/ch32fun` README.

## Primary Target
- **CH32V003** — 10-cent RISC-V EC core, 48 MHz, 16 KB flash, 2 KB RAM. Packages: SOP-8, QFN-20, SOIC.

## Supported Families
- CH32X035
- CH32V002, V004, V005, V006, V007
- CH32V103, V203, V208, V303, V305, V307
- CH570, CH572, CH571, CH573, CH581, CH582, CH583, CH584, CH585, CH591, CH592
- Some obscure chips reported working: CH32M030, CH641, CH643, CH645

## Notes
- GDB is mostly tested on the V003, but works to a limited degree on others.
- For RISC-V QingKeV2 details, see the Processor Manual and Debug Manual links in the repo README footnotes.

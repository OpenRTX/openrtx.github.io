# Yaesu FT2D
 
## Device models
- Yaesu FT2D[RE]

The suffix "R" denotes the USA version, "E" denotes the european version.

## Specifications
* Display: 160x160 1.7'' touch screen grayscale LCD
* Frequency range
    * VHF: 144.0000-146.0000
    * UHF: 430.0000-440.0000

## Hardware configuration

The FT2D features three different CPUs, named MAIN, SUB and DSP:

|  CPU | Part Name        | Manufacturer      | ISA   |
|:----:|------------------|-------------------|-------|
| MAIN | R5F61668RN50BGV  | Renesas           | H8SX  |
|  SUB | R5F5630BCDLK     | Renesas           | RX630 |
|  DSP | TMS320VC5509AZHH | Texas Instruments | C55x  |


H8SX is supported by a GCC 4.7 fork by Renesas, no Ghidra module exists.
RX600 is supported by a GCC 8.3.0 fork by Renesas, a [Ghidra proc module](https://github.com/ballon-rouge/rx-proc-ghidra) is available.

## Memory mapping

TODO

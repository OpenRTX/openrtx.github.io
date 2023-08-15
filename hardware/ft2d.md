# Yaesu FT2D
 
## Device models
- Yaesu FT2D[RE]

The suffix "R" denotes the USA version, "E" denotes the european version.

## Specifications
* Display: 160x160 1.7'' touch screen greyscale LCD
* Frequency range
    * VHF: 144.0000-146.0000
    * UHF: 430.0000-440.0000

## Hardware configuration

The FT2D features three different CPUs, named MAIN, SUB and DSP:

| CPU    | Part Name          | Manufacturer        | ISA     | Flash   | RAM    |
| :----: | ------------------ | ------------------- | ------- | ------- | ------ |
| MAIN   | R5F61668RN50BGV    | Renesas             | H8SX    | 1MB     | 56KB   |
| SUB    | R5F5630BCDLK       | Renesas             | RX630   | 1MB     | 96KB   |
| DSP    | TMS320VC5509AZHH   | Texas Instruments   | C55x    | 64KB    | 256KB  |


H8SX is supported by a GCC 4.7 fork by Renesas, no Ghidra module exists.
RX600 is supported by a GCC 8.3.0 fork by Renesas, a [Ghidra processor module](https://github.com/ballon-rouge/rx-proc-ghidra) is available.

The two Renesas CPU communicate through a serial port, connected to the RXD6 and TXD6 ports of both processors.
The RX630 (SUB) is responsible of managing the screen and the GPS module, while the H8SX (MAIN) is responsible for all
the main radio features.

The OpenRTX firmware will run on the MAIN CPU, with full multi-threading support, the SUB CPU will run a single OpenRTX-compatible
firmware, which will fetch UI information from the serial port, plot it on the screen, read the data from the GPS module and send it back.
All of this will be done in a single task, so that no RTOS is required on the SUB CPU.

## Memory mapping

### SUB

|           Name          | Start Address | Length   |
|:-----------------------:|---------------|----------|
|           RAM           | 0x0           | 0x20000  |
| Program ROM (read only) | 0xFFF00000    | 0xfffff |

## M17 Path

Q2082 DAC, which is the main one used for Rx/Tx is connected to the MAIN CPU.

## Disassembly

Full disassembly pictures and videos can be found [on the Recessim Wiki](https://wiki.recessim.com/view/Yaesu_FT2DR).

## OpenRTX Porting

There is a freeRTOS port [available for H8S](https://www.freertos.org/porth8s.html),
which probably can be used as a reference for the H8SX.

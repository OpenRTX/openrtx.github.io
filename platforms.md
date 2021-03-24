# Supported Platforms

The OpenRTX code, especially the one dealing with hardware, is organised following a hierarchical structure composed of **device family**, **platform** and **model**:
* **Device family**: group of devices which have similar features.
* **Platform**: group of devices of the same family, employing the same low-level drivers.
* **Model**: specific device of a given platform, with its own dedicated settings.

## Device tree
* **MDx family**
    * **MD-3x0 Platform**, compilation target `md3x0`
        * Tytera MD380
        * Tytera MD380G
        * Tytera MD390
        * Tytera MD390G
        * Retevis RT3

    * **MD-UV3x0 Platform**
        * Tytera MD-UV380,    target `mduv380`
        * Tytera MD-UV380G,   target `mduv380g`
        * Tytera MD-UV390,    target `mduv390`
        * Tytera MD-UV390G,   target `mduv390g`
        * Baofeng DM-1701,    temporary target `mduv380`
        * Retevis RT3s,       target `mduv380`
        * Retevis RT3s (GPS), target `mduv380g`

    * **MD-9600 Platform**
        * Tytera MD-9600,       target `md9600`
        * Tytera MD-9600 (GPS), target `md9600g`

* **GDx family**
    * **GDx Platform**
        * Radioddity GD-77, target `gd77`
        * Baofeng DM-1801, target `md1801`

## Supported Features Matrix

### Legend
- 🔴 Not supported
- 🟡 In development
- 🟢 Supported

### Basic Functions

| Radio model                    | Boot  | Display | Keyboard | CPS   | RTC   | Persistence | GPS   |
| ---                            | :---: | :---:   | :---:    | :---: | :---: | :---:       | :---: |
| Tytera MD-380 / MD-390         | 🟢    | 🟢       | 🟢       | 🟢    | 🟢     | 🟡          | 🟢     |
| Tytera MD-UV380 / Retevis RT3s | 🟢    | 🟢       | 🟢       | 🟢    | 🟢     | 🟡          | 🟢     |
| Radioddity GD-77 / DM-1801     | 🟢    | 🟢       | 🟢       | 🟢    | N/A    | 🟡          | N/A   |
| Tytera MD-9600                 | 🟢    | 🔴       | 🔴       | 🔴    | 🔴     | 🔴          | 🔴     |

### Modes

| Radio model                    | FM RX | FM TX | M17 RX | M17 TX | APRS RX | APRS TX | DMR RX | DMR TX | DMR SMS |
| ---                            | :---: | :---: | :---:  | :---:  | :---:   | :---:   | :---:  | :---:  | :---:   |
| Tytera MD-380 / MD-390         | 🟢    | 🟢     | 🟡     | 🟡     | 🔴      | 🔴      | 🔴     | 🔴     | 🔴      |
| Tytera MD-UV380 / Retevis RT3s | 🟢    | 🟢     | 🔴     | 🔴     | 🔴      | 🔴      | 🔴     | 🔴     | 🔴      |
| Radioddity GD-77 / DM-1801     | 🟢    | 🟢     | 🔴     | 🔴     | 🔴      | 🔴      | 🔴     | 🔴     | 🔴      |
| Tytera MD-9600                 | 🔴    | 🔴     | 🔴     | 🔴     | 🔴      | 🔴      | 🔴     | 🔴     | 🔴      |

## Table of hardware

This table provides a glance on the underlying hardware of each of the supported platforms, for a detailed description see the [hardware page](hardware.md).

Platform|MCU|DMR baseband|RF chip|Display controller|Non volatile memory|GPS|
---------|:---:|:---:|:---:|:---:|:---:|:---:|
 MD-3x0  | STM32F405VG | HR-C5000 | SKY73210 | HX8302A | 25Q128FV SPI flash | JS-M710 |
MD-UV3x0 | STM32F405VG | HR-C6000 | AT1846S  | HX8302A | 25Qx SPI flash     | JS-H210 |
   GDx   | MK22FN512   | HR-C6000 | AT1846S  | UC1701  | 25Q80BV SPI flash +<br>AT24C512 I2C EEPROM | - |
 MD-9600 | STM32F405VG | HR-C6000 | SKY73210 | ST7567  | 25Q128FV SPI flash | JS-M710 |

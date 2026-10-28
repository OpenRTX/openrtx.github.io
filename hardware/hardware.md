# Hardware documentation

This section collects all the information about the hardware and the OEM firmware of the supported devices, collected either through the various documents available on the internet or by reverse engineering.

### Table of hardware

This table provides a glance on the underlying hardware of each of the supported platforms, for a detailed description see the individual pages linked below.

| Platform          |      MCU       | Baseband | RF chip  | Display controller |             Non volatile memory              |     GPS     |
|-------------------|:--------------:|:--------:|:--------:|:------------------:|:--------------------------------------------:|:-----------:|
| MD-3x0            |  STM32F405VG   | HR-C5000 | SKY72310 |      HX8302A       |              25Q128FV SPI flash              |   JS-M710   |
| MD-UV3x0          |  STM32F405VG   | HR-C6000 | AT1846S  |      HX8302A       |              25Q128FV SPI flash              |   JS-H210   |
| GDx               |   MK22FN512    | HR-C6000 | AT1846S  |       UC1701       | 25Q80BV  SPI flash +<br>AT24C512 I2C EEPROM  |      -      |
| HD1               |   MK22FN512    | HR-C6000 | AT1846S  |                    | 25Q80BV  SPI flash +<br>AT24C512 I2C EEPROM  |  ST-26-U7L  |
| DM-1x01           |       ?        |     ?    |    ?     |         ?          |                 ?                            |     ?       |
| MD-9600           |  STM32F405VG   | HR-C6000 | SKY72310 |       ST7567       |              25Q128FV SPI flash              |   JS-M710   |
| T-TWR Plus        |    ESP32-S3    |  SA868S  | AT1846S  |       SH1106       |               optional microSD               |    L76K     |
| CS7000-M17        |  STM32F405VG   | HR-C6000 | SKY72310 |      ST7735R       |              25Q128FV SPI flash              | MC-1010-2RE |
| CS7000-M17 Plus   |  STM32H743VI   | HR-C6000 | SKY72310 |      ST7735R       |              25Q128FV SPI flash              | MC-1010-2RE |
| C62               |    CSK6011B    |    -     |  BK4819  |      ST7735R       |             PY25Q40HB SPI flash              |      -      |
| AT-D890UV         |  STM32U575VI   | SCT3288? | WX4818N? |                    |          TC58CVG2S0HRAIJ SPI Flash           |   CC1167Q   |
| AT-D878UV         |  ?             |        ? |        ? |                    |                                   ?          |   ?         |
| RT-4D             |  AT32F423CCT7  | HF6853   | BK4819   |       ST7567       |             PY25Q128HA SPI flash             |      -      |
| FT-2D             |       ?        |      ?   |    ?     |         ?          |                     ?                        |      ?      |

### Device documentation

* [Tytera MD-380(G) and MD-390(G)](hardware/md380.md)
* [Tytera MD-UV380(G), MD-UV390, and Retevis RT3S](hardware/mduv380.md)
* [Tytera MD-9600 (G)](hardware/md9600.md)
* [Radioddity GD-77 and Baofeng DM-1801](hardware/gd77.md)
* [Yaesu FT2D](hardware/ft2d.md)
* [Ailunce HD1](hardware/hd1.md)
* [Anytone AT-D878UV](hardware/uv878.md)
* [LILYGO T-TWR Plus](hardware/ttwrplus.md)
* [Radtel RT-4D](hardware/rt4d.md)
* [Connect Systems CS7000-M17 and CS7000-M17 Plus](hardware/cs7000.md)
* [Retevis C62](hardware/c62.md)

### Devices without doc pages
* Anytone AT-D890UV
* Module 17
* Talkpod A36plus



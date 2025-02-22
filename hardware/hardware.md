# Hardware documentation

This section collects all the information about the hardware and the OEM firmware of the supported devices, collected either through the various documents available on the internet or by reverse engineering.

### Table of hardware

This table provides a glance on the underlying hardware of each of the supported platforms, for a detailed description see the individual pages linked below.

| Platform   |     MCU     |   Baseband   |  RF chip | Display controller |             Non volatile memory             |   GPS     |
|------------|:-----------:|:------------:|:--------:|:------------------:|:-------------------------------------------:|:---------:|
| MD-3x0     | STM32F405VG |   HR-C5000   | SKY72310 |       HX8302A      |              25Q128FV SPI flash             | JS-M710   |
| MD-UV3x0   | STM32F405VG |   HR-C6000   |  AT1846S |       HX8302A      |              25Q128FV SPI flash             | JS-H210   |
| GDx        |  MK22FN512  |   HR-C6000   |  AT1846S |       UC1701       | 25Q80BV  SPI flash +<br>AT24C512 I2C EEPROM |    -      |
| HD1        |  MK22FN512  |   HR-C6000   |  AT1846S |                    | 25Q80BV  SPI flash +<br>AT24C512 I2C EEPROM | ST-26-U7L |
| MD-9600    | STM32F405VG |   HR-C6000   | SKY72310 |       ST7567       |              25Q128FV SPI flash             | JS-M710   |
| T-TWR Plus | ESP32-S3    |   SA868S     |  AT1846S |       SH1106       |              optional microSD               | L76K      |

### Device documentation

* [MD-380(G) and MD-390(G)](hardware/md380.md)
* [MD-UV380(G)](hardware/mduv380.md)
* [MD-9600 (G)](hardware/md9600.md)
* [GD-77 and DM-1801](hardware/gd77.md)
* [Yaesu FT2D](hardware/ft2d.md)
* [Ailunce HD1](hardware/hd1.md)
* [Anytone UV878](hardware/uv878.md)
* [LILYGO T-TWR Plus](hardware/ttwrplus.md)
* [Radtel RT-4D](hardware/rt4d.md)

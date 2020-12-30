# Hardware Documentation

For OpenRTX we use the following definition for device **family**, **platform** and **model**:
* Device **family**: Devices which have similar features
* Device **platform**: Devices who employ the same drivers
* Device **model**: Devices which employ the same drivers and settings

## Hardware components matrix
Platform|MCU|DMR baseband|RF chip|Display controller|Non volatile memory|GPS|
---     |:---:|:---:|:---:|:---:|:---:|:---:|
[MD3x0](md3x0.md)     | STM32F405VG | HR-C5000 | SKY73210 | HX8302A | 25Q128FV SPI flash | |
[MD-UV3x0](mduv3x0.md)| STM32F405VG | HR-C6000 | AT1846S  | HX8302A | 25Qx SPI flash     | |
[GD77](gd77.md)       | MK22FN512   | HR-C6000 | AT1846S  | UC1701  | 25Q80BV SPI flash +<br> AT24C512 I2C EEPROM | |
[MD-9600](md9600.md)  | STM32F405VG | HR-C6000 | AT1846S  |         |                    | |

## Device models
### MDx family
#### MD3x0 Platform
* Tytera MD380, target `md380`
* Retevis RT3, target `md380`
* Tytera MD380G, target `md380g`
* Tytera MD390, target `md390`
* Tytera MD390G, target `md390g`

#### MD-UV3x0 Platform
* Tytera MD-UV380, target `mduv380`
* Retevis RT3s, target `mduv380`
* Baofeng DM-1701, temporary target `mduv380`
* Tytera MD-UV380G, target `mduv380g`
* Retevis RT3s (GPS), target `mduv380g`
* Tytera MD-UV390, target `mduv390`
* Tytera MD-UV390G, target `mduv390g`

#### MD-9600 Platform
* Tytera MD-9600, target `md9600`
* Tytera MD-9600 (GPS), target `md9600g`

### GDx family

#### GD77 Platform
* Radioddity GD77, target `gd77`
* Baofeng DM-1801, target `gd77`


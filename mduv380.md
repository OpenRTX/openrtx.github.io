# MD-UV380

## Specifications
### Frequency range
* VHF: 136.000-174.000
* UHF: 400.000-480.000

## Model variants
There are two variants of this radio available for purchase
- With GPS
- Without GPS

These two variants apart from having or not the GPS chip included,
have a difference in the SPI flash memory layout.

The start address of the CPS sections varies as following:

No GPS version | GPS version | Section
  ---   | ---    | ---
0x40000 | 0x110000 | Channel
0x70000 | 0x140000 | Contacts

## Hardware components
### Clock tree configuration
To be found

## Memory mapping

### SPI Flash layout
Start  | End    | Section
  ---  | ---    | ---
0x2001 | 0x2005 | CPS timestamp
0x2040 | 0x2049 | Splash text line 1
0x2050 | 0x2059 | Splash text line 2
0x2180 | 0x59b9 | Saved SMS messages

#### CPS timestamp
0x00|0x01|0x2|0x3|0x4|0x5|0x6|0x7|0x8|0x9|0xA|0xB|0xC|0xD|0xE|0xF| 
---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
FF|year(first two digits)|year(last two digits)|month|date|hour|minute|seconds|00|01|01|02|FF|FF|FF|FF

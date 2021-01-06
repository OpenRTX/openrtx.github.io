# User Guide

> This firmware is highly experimental and is in alpha stage. \
Not all functionalities you expect from your device are implemented yet. \
However contributions and testing are welcome and accepted.

## Where to get OpenRTX
* You can find pre-built versions on OpenRTX on our [GitHub releases page](https://github.com/OpenRTX/OpenRTX/releases)
* You can compile OpenRTX from source following [these instructions](https://github.com/OpenRTX/OpenRTX/wiki/How-to-compile)

## How to flash OpenRTX to your radio
To flash the OpenRTX firmware to your radio, follow these steps:

* Backup your codeplug using your favourite codeplug editor
Flashing OpenRTX won't erase your codeplug or other settings, 
it will just replace your firmware, but it's better to have a backup

* Put your radio in DFU mode (bootloader mode)
To do this you have to turn off the radio and turn it back on while pressing a combination of keys that
depends on your specific model:
    * For MD3x0 radios you have to press the PTT button and the side button above it
    * For GDx radios you have to press the two side buttons below it
The DFU mode will be indicated by the screen being off and a blinking or steady LED on your radio

* Flash OpenRTX
    * From Linux you can use [radio_tool](https://github.com/v0l/radio_tool)
    * From Windows you can use the software that comes with firmware upgrades for your radio

* Enjoy

* To bring the radio back to the factory or other firmware just follow again this procedure
but flashing another firmware

## How to update your codeplug from OpenRTX
Radios supported by OpenRTX use the DFU protocol to read and write the codeplug from a computer.

To update the codeplug on a radio running OpenRTX you can put the radio in DFU mode and use
your favourite codeplug editor to read or write the codeplug.
To put the radio in DFU mode look at the flashing instructions on this page.

Here are some codeplug editors for Linux:
- [dmrconfig](https://github.com/OpenRTX/dmrconfig)
- [editcp](https://github.com/DaleFarnsworth-DMR/editcp)
- [qdmr](https://github.com/hmatuschek/qdmr) 

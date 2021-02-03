# User Guide

> NOTICE: this firmware is highly experimental and in an alpha stage, this means \
that not all functionalities you expect from your device are yet implemented. \
However, contributions and testing are welcome and accepted.

## Where to get OpenRTX
* You can find pre-built OpenRTX binary images on our [GitHub releases page](https://github.com/OpenRTX/OpenRTX/releases)
* Or you can compile OpenRTX from the sources by following [these instructions](https://github.com/OpenRTX/OpenRTX/wiki/How-to-compile)

## How to flash OpenRTX to your radio
To flash the OpenRTX firmware on your radio, follow these steps:

* _Backup your codeplug using your favourite codeplug editor._ Flashing OpenRTX will just replace your firmware without erasing your codeplug or other settings, however is safer to have a backup.

* _Put your radio in DFU (bootloader) mode_. To do this you have to turn off the radio and turn it back on while pressing a combination of keys that depends on your specific model, the DFU mode will be indicated by the screen being off and a blinking or steady LED:
    * For MD3x0 radios you have to press the PTT button and the side button above it.
    * For GDx radios you have to press the two side buttons below it.
    

* _Flash OpenRTX_
    * On Linux you can use [radio_tool](https://github.com/v0l/radio_tool).
    * On Windows you can use the radio's manufacturer firmware upgrade tool.

* _Enjoy_

To restore the original firmware, just flash it on the radio like you did for OpenRTX or the upgrades to the original firmware.

## How to update your codeplug from OpenRTX
At the moment the only means to update the codeplug on a radio flashed with OpenRTX is using the manufacturer's DFU protocol: to update the codeplug, then, you have to put the radio in DFU mode and use your favourite codeplug editor to read or write the codeplug.

To put the radio in DFU mode look at the instructions for firmware flashing in the above section, while for writing the codeplug you can use either the radio manufacturer's tool or use one of the following editors (currently only for Linux):

- [dmrconfig](https://github.com/OpenRTX/dmrconfig)
- [editcp](https://github.com/DaleFarnsworth-DMR/editcp)
- [qdmr](https://github.com/hmatuschek/qdmr) 

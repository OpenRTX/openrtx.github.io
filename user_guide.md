# User Guide

> NOTICE: this firmware is highly experimental and in an alpha stage, this means \
that not all functionalities you expect from your device are yet implemented. \
However, contributions and testing are welcome and accepted.

## Where to get OpenRTX
### Stable builds
**Stable builds** are recommended for general use, they are well tested and include only functionality considered complete.
* You can find pre-built stable OpenRTX binary images on our [GitHub releases page](https://github.com/OpenRTX/OpenRTX/releases)

### Unstable builds
**Unstable builds** are not fully tested, use them only if you want to develop or try a feature not yet released.
* You can find nightly OpenRTX builds [here](https://openrtx.schinken-radio.de/nightly/)
(thanks to Phil DF5PMF for building and hosting them)
* Or you can compile OpenRTX from the sources by following [these instructions](compiling.md)

## How to flash OpenRTX to your radio
To flash the OpenRTX firmware on your radio, follow these steps:

* _**Backup your codeplug using your favourite codeplug editor.**_ Flashing OpenRTX will just replace your firmware without erasing your codeplug or other settings, however is safer to have a backup.

* _**Put your radio in DFU (bootloader) mode**_. To do this you have to turn off the radio and turn it back on while pressing a combination of keys that depends on your specific model, the DFU mode will be indicated by the screen being off and a blinking or steady LED:
    * For MD3x0 radios you have to press the PTT button and the side button above it.
    * For GDx radios you have to press the two side buttons below it.
    

* _**Flash OpenRTX**_
    * On Linux you can use [radio_tool](https://github.com/v0l/radio_tool).
    * On Windows you can use the radio's manufacturer firmware upgrade tool.

* _**Enjoy**_

To restore the original firmware, just flash it on the radio like you did for OpenRTX or the upgrades to the original firmware.

## How to update your codeplug from OpenRTX
At the moment the only means to update the codeplug on a radio flashed with OpenRTX is using the manufacturer's DFU protocol: to update the codeplug, then, you have to put the radio in DFU mode and use your favourite codeplug editor to read or write the codeplug.

To put the radio in DFU mode look at the instructions for firmware flashing in the above section, while for writing the codeplug you can use either the radio manufacturer's tool or use one of the following editors (currently only for Linux):

- [dmrconfig](https://github.com/OpenRTX/dmrconfig)
- [editcp](https://github.com/DaleFarnsworth-DMR/editcp)
- [qdmr](https://github.com/hmatuschek/qdmr) 


## Operating your OpenRTX radio
### MD380/UV380 (RT3/RT3s)

### Left
* Keeping the 'M' button pressed, brings up the 'macro menu'. Within the 'macro menu' you can select the following functions which are under the following keys:
    * 1 CTCSS frequency
    * 2 CTCSS encode/decode/off
    * 3 Transmit power ( low / high )
    * 4 Frequency raster ( 12.5 / 20 / 25 Khz )
    * 5 Modulation mode ( DMR / FM / M17 )
    * 6 Lock
    * 7 B+
    * 8 B-
    * 9 Save settings ( not implemented yet )

### Right
* Programming/accessory input/output

### Front
* By presssing the buttons, the following functions can be selected:
    * 0 n/a
    * Green book
        * Zone
        * Channel
        * Contacts
        * GPS
        * Settings
        * Info
        * About

    * Red book
        * VFO / Memory selection
    * Up arrow
        * step up in frequency (VFO mode) or up configured channel (memory mode)
    * Down arrow
        * step down in frequency (VFO mode) or up configured channel (memory mode)
    * */lock
    * #/up arrow

### Top
* The knobs on the top provide:
    * Channel/Frequency selection
    * Power/Volume


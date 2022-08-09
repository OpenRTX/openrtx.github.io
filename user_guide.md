# Users guide

> NOTICE: this firmware is still under significant development, this means that not all functionalities you expect from your device are yet implemented. However, contributions and testing are welcome and accepted.

## Getting OpenRTX

**Stable builds** are recommended for general use, they are well tested and include only functionality considered complete.
Pre-built stable OpenRTX binary images are available on our [GitHub releases page](https://github.com/OpenRTX/OpenRTX/releases)

**Nightly builds** are pre-build images obtained every day by compiling the latest commits on the master branch. By their nature, they are not fully tested and may contain bugs or regressions: use them only if you want to develop or try a feature not yet released.\
OpenRTX nightly builds are available at these places:
* at Phil DF5PMF's [page](https://openrtx.schinken-radio.de/nightly/) (thanks for building and hosting them!);
* on OpenRTX [page](https://files.openrtx.org/nightly/).

Finally, you can compile OpenRTX from the sources by following [these instructions](compiling.md).

## Flashing OpenRTX to your radio
To flash the OpenRTX firmware on your radio, follow these steps:

* **Backup your codeplug using your favourite codeplug editor.** Flashing OpenRTX will just replace your firmware without erasing your codeplug or other settings, however is safer to have a backup.

* **Put the radio in firmware upgrade mode**. To do this you have to turn off the radio and turn it back on while pressing a combination of keys that depends on your specific model, the firmware upgrade mode will be indicated by the screen being off and a blinking or steady LED:
    * For MD3x0 radios you have to press the PTT button and the side button above it.
    * For GDx radios you have to press the two side buttons below it.


* **Flash OpenRTX**
    * On Linux you can use [radio_tool](https://github.com/v0l/radio_tool).
    * On Windows you can use the radio's manufacturer firmware upgrade tool.

* **Enjoy**

To restore the original firmware, just flash it on the radio like you did for OpenRTX or the upgrades to the original firmware.

## Updating the codeplug
Currently the only means to update the codeplug on a radio flashed with OpenRTX is using the manufacturer codeplug update mechanism. If the radio supports codeplug update when in firmware upgrade mode, power it on in this mode and update the codeplug. If this is not the case, to update the codeplug you have to flash back the original firmware and then read/write the codeplug while the radio is running it. Once you finished, you can re-flash an OpenRTX image.

To put the radio in bootloader mode look at the instructions for firmware flashing in the above section, while for writing the codeplug you can use either the radio manufacturer's tool or use one of the following editors (currently only for Linux):

- [dmrconfig](https://github.com/OpenRTX/dmrconfig)
- [editcp](https://github.com/DaleFarnsworth-DMR/editcp)
- [qdmr](https://github.com/hmatuschek/qdmr)



## Operating your OpenRTX radio
#### Left
* Keeping the 'M' button pressed, brings up the 'macro menu'. Within the 'macro menu' you can select the following functions which are under the following keys:
    * 1 CTCSS frequency
    * 2 CTCSS encode/decode/off
    * 3 Transmit power ( low / high )
    * 4 IF filter bandwidth ( 12.5 / 20 / 25 Khz )
    * 5 Modulation mode ( DMR / FM / _M17_ )
    * 6 Lock
    * 7 B+ Display brightness increase
    * 8 B- Display brightness decrease
    * 9 Save settings ( not implemented yet )

#### Right
* Programming/accessory input/output

#### Front
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
    * */lock n/a
    * #/up arrow n/a

#### Top
* Power/Volume
* Channel/Frequency knob, on Tytera MD380/UV380 (Retevis RT3/RT3s): works as up/down arrow keys. Rotation clockwise acts as one press of the up key per step, counterclockwise as one press of the down key per step.

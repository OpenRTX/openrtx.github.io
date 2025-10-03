# Users guide

> NOTICE: this firmware is still under significant development, this means that not all functionalities you expect from your device are yet implemented. However, contributions and testing are welcome and accepted.

## Getting OpenRTX

**Stable builds** are recommended for general use, they are well tested and include only functionality considered complete.
Pre-built stable OpenRTX binary images are available on our [GitHub releases page](https://github.com/OpenRTX/OpenRTX/releases)

**Nightly builds** are pre-built images obtained every day by compiling the latest commits on the master branch. By their nature, they are not fully tested and may contain bugs or regressions: use them only if you want to develop or try a feature not yet released.\
OpenRTX nightly builds are available at these places:
* at Phil DF5PMF's [page](https://openrtx.schinken-radio.de/nightly/) (thanks for building and hosting them!);
* on OpenRTX [page](https://files.openrtx.org/nightly/).

Finally, you can compile OpenRTX from the sources by following [these instructions](compiling.md).

## Flashing OpenRTX to your radio
To flash the OpenRTX firmware on your radio, follow these steps:

* **Backup your codeplug using your favourite codeplug editor.** Flashing OpenRTX will just replace your firmware without erasing your codeplug or other settings, however it is safer to have a backup.

* **Put the radio in firmware upgrade mode**. To do this you have to turn off the radio and turn it back on while pressing a combination of keys that depends on your specific model, the firmware upgrade mode will be indicated by the screen being off and a blinking or steady LED:
    * For MD3x0 radios you have to press the PTT button and the side button above it.
    * For GDx radios you have to press the two side buttons below it.
    * For MD9600 you have to press the red button and the P1 button on the radio.


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

* FM Mode

    |   |   |   |   |
    |---|---|---|---|
    | **Green Book** | **Up Arrow** <br> Squelch step up | **Down Arrow** <br> Squelch step down | **Red Book** |
    | **1** <br> T- CTCSS <br> Frequency Step down | **2** <br> T+ CTCSS <br> Frequency Step up | **3** <br> CTCSS Mode <br> (Disable / Encode /<br>Decode / Encode+Decode) | **\* \ Lock** |
    | **4** <br> IF Filter bandwidth <br> (12.5 / 20 / 25 KHz) | **5** <br> Modulation Mode <br> (DMR/FM/M17) | **6** <br> Transmitter Power <br> (high / low or 1W / 5W) | **0 \ \_** |
    | **7** <br> B+ <br> Increase Display <br> Brightness| **8** <br> B- <br> Decrease Display <br> Brightness  | **9** <br> Lock | **# \ Shift arrow** |

* M17 Mode

    |   |   |   |   |
    |---|---|---|---|
    | **Green Book** | **Up Arrow** <br> Squelch step up | **Down Arrow** <br> Squelch step down | **Red Book** |
    | **1** | **2** | **3** | **\* \ Lock** |
    | **4** | **5** <br> Modulation Mode <br> (DMR/FM/M17) | **6** <br> Transmitter Power <br> (high/low or 1W/5W) | **0 \ \_** |
    | **7** <br> B+ <br> Increase Display <br> Brightness| **8** <br> B- <br> Decrease Display <br> Brightness  | **9** <br> Lock | **# \ Shift arrow** |

* Note: Brightness control is not available on MD-UV380. See [hwconfig.h](https://github.com/OpenRTX/OpenRTX/blob/dbe7ff470004e57b12c85243d4ca7a7664cf4f77/platform/targets/MD-UV3x0/hwconfig.h#L155) for additional details.


#### Right
* Programming/accessory input/output

#### Front
* By pressing the buttons, the following functions can be selected:


|   |   |   |   |
|---|---|---|---|
| **Green Book** <br> Menu/Settings <br> (Enter/Accept) | **Up Arrow** <br> Freq step up | **Down Arrow** <br> Freq step down | **Red Book** <br> VFO / Memory <br> (Cancel)|
| **1** | **2** | **3** | **\* \ Lock** |
| **4** | **5** | **6** | **0 \ \_** |
| **7** | **8** | **9** | **# \ Shift arrow** <br> *M17 mode:* <br> - Set destination |

* Green Book Menu / Settings
    * Banks
    * Channels
    * Contacts
    * GPS
    * Settings
    * Info
    * About

#### Top
* Power/Volume
* Channel/Frequency knob
    * on Tytera MD380/UV380 (Retevis RT3/RT3s): works as up/down arrow keys. Rotation clockwise acts as one press of the up key per step, counterclockwise as one press of the down key per step.

#### Front

* FM Mode
**#** is used to send a 1750 Hz repeater tone ("tone burst") while pressing PTT.

* M17 Mode
**#** is used to change the destination call. Tap **#**, then use the keypad to enter the call, with ultimately the **Green Book** or **OK** button being used to confirm entry. Note that when entering a callsign, **\*** may be used to add a "/" or "-" char, and **0** may be used to add a space.

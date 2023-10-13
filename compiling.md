# Building OpenRTX from sources

- [Building OpenRTX from sources](#building-openrtx-from-sources)
  - [Linux toolchain setup](#linux-toolchain-setup)
      - [Installing the basic tools](#installing-the-basic-tools)
      - [Additional requirements only for linux emulator](#additional-requirements-only-for-linux-emulator)
      - [Tools required for firmware images](#tools-required-for-firmware-images)
  - [Windows toolchain setup](#windows-toolchain-setup)
  - [Getting the source code](#getting-the-source-code)
  - [Compiling for radios](#compiling-for-radios)
  - [Compiling for Linux](#compiling-for-linux)
      - [Compiling with address sanitizer](#compiling-with-address-sanitizer)
      - [Compiling on Alpine Linux / PostmarketOS](#compiling-on-alpine-linux--postmarketos)
      - [Compiling a Zephyr-based target](#compiling-a-zephyr-based-target)
  - [Flashing the firmware to a radio](#flashing-the-firmware-to-a-radio)
      - [Tytera and Retevis radios](#tytera-and-retevis-radios)
      - [GD-77 and DM-1801](#gd-77-and-dm-1801)
  - [Running on Linux](#running-on-linux)

## Linux toolchain setup

The basic tools required to compile OpenRTX from the sources are _git_ and the _meson_ build system. If building only the emulator version, the compiler shipped with your distribution is sufficient. On the other hand, if you're building the firmware for one of the radio targets, you'll require also the GCC toolchain for the miosix kernel. In this latter case, also _cmake_ and _libusb_ are required for compiling the external tools for flashing the radio.

#### Installing the basic tools

To install the basic tools required to compile both the linux emulator and the firmware images, you can use the package manager provided with your linux distribution. E.g. on Debian/Ubuntu and derived distributions you can use:

```
sudo apt update && sudo apt install git pkg-config build-essential
```

Then, install `meson` and `ninja` using pip:

```
sudo apt install python-pip3
pip3 install --user meson ninja
```

Alternatively, you can use the package manager of your distribution. For example:

```
sudo apt install meson
```

#### Additional requirements only for linux emulator

When compiling the linux emulator version, the following additional packages are required:
* SDL2 development package
* Codec2 development package
* `readline` package

The package names depend on the package manager you use. On Debian/Ubuntu and derived distributions the command is:

```
sudo apt install libsdl2-dev codec2-dev readline
```

#### Tools required for firmware images

To build the firmware images ready to be flashed on the radios, the miosix kernel GCC toolchain is required, as well as some additional tools used to encrypt and flash the binary files obtained at the end of the compilation process.

**WARNING: since the latest release, the GCC toolchain for miosix kernel is compatible only with x64 systems!**

To install the toolchain, download the installer and run it: the installer will ask for your root password to copy the compiler to the `/opt/arm-miosix-eabi` directory, and put symlinks to `/usr/bin`.

```
wget https://miosix.org/toolchain/MiosixToolchainInstaller.run
sh MiosixToolchainInstaller.run
```
The toolchain also provides an uninstall script, which can be found in the installation directory.

The tool used to encrypt and flash the binary executables is called [radio_tool](https://github.com/v0l/radio_tool). The compilation script will automatically detect if _radio\_tool_ is already installed in the system and, if this is not the case, it will download the sources and compile automatically a local copy of the program. For the compilation process to succeed _cmake_ and _libusb_ must be present in the system: to install them, use the system package manager. On Debian/Ubuntu and derived distributions the command is:

```
sudo apt install cmake libusb-1.0-0 libusb-1.0-0-dev
```

Finally, if you are targeting the *Module 17* platform, _dfu-util_ is required to flash the binary image on the microcontroller's flash memory. This tool can be installed using the system package manager:


```
sudo apt install dfu-util
```

Alternatively, it can be compiled from source with the following commands:

```
sudo apt install autoconf
git clone git://git.code.sf.net/p/dfu-util/dfu-util
./autogen.sh
./configure
make
make install
```

#### Compiling a Zephyr-based target

You need to install the following packages (their names might vary according to your Linux distribution):

- `zephyr-sdk`
- `python-west`
- `python-pyelftools`
- `python-cbor2`
- `python-intelhex`
- `python-requests`

[Dependency intructions for Fedora and other distros:](https://docs.zephyrproject.org/latest/develop/getting_started/installation_linux.html)

How to clone OpenRTX and Zephyr with a single command:
```
mkdir openrtx-build && cd $_
west init -m https://github.com/OpenRTX/OpenRTX
west update
source zephyr/zephyr-env.sh # You need to execute this for every new shell
```

From OpenRTX root, compile with:

```
meson setup build
meson compile -C build openrtx_ttwrplus
```

Flash with:

```
west flash
```

Check the USB serial console with:

```
west espressif monitor
```

You can debug a Zephyr target by installing `openocd-esp32` and placing
[the following udev rules](https://github.com/espressif/openocd-esp32/blob/master/contrib/60-openocd.rules) in the /etc/udev/rules.d folder.

You can start a GDB shell using:

```
west debug --openocd `which openocd-esp32openocd`
```

## Flash MCUBOOT to the ESP32
```
cd $ZEPHYR_PATH/zephyr
source zephyr-env.sh
west build -p always -b esp32s3_devkitm --sysbuild samples/hello_world
west flash
```

---

## Windows toolchain setup

On Windows is possible only to compile the binary images for the radios, the linux emulator will not work. To set up the toolchain follow these steps:

- Install _git_ from [here](https://git-scm.com/download/win)
- Install _perl_ from [here](https://strawberryperl.com/)
- Install _python_ from [here](https://www.python.org/downloads/)
- Install the Miosix Toolchain from [here](https://miosix.org/wiki/index.php?title=Miosix_Toolchain)

Install _meson_ and _ninja_ using _pip_, run these commands in a PowerShell:

```
pip3 install --user meson ninja
```

Finally, install radio _radio\_tool_: download the windows archive from [here](https://github.com/v0l/radio_tool) and extract it in a folder of your choice. Add _radio\_tool_ to your _PATH_ environment variable by running the following command in a PowerShell:

```
 $env:PATH += ";path-to-radio_tool"
```

To verify that radio_tool is in PATH, this should not return an error

```
 get-command radio_tool
```

---

## Getting the source code

To obtain the OpenRTX source code, clone the github repository with the following command:

```
git clone --recursive https://github.com/OpenRTX/OpenRTX
```

This command also ensures that all the submodules providing the dependencies for the main codebase are correctly fetched from their sources. Alternatively, you can use the following sequence of commands:

```
git clone https://github.com/OpenRTX/OpenRTX
cd OpenRTX
git submodule init
git submodule update
```

---

## Compiling for radios

To build the firmware binary issue the following commands:

```
meson setup --cross-file cross_arm.txt build_arm
meson compile -C build_arm openrtx_TARGET
```

Where `TARGET` has to be replaced with the correct build target, depending on your radio model:

- TYT MD-380, MD-390, Retevis RT3, Retevis RT8 → `md3x0`
- TYT MD-UV380, MD-UV390, Retevis RT3s → `mduv3x0`
- TYT MD-9600 → `md9600`
- Radioddity GD-77 → `gd77`
- Baofeng DM1801 → `dm1801`
- Module17 → `mod17`

**NOTE: if you are using a version of Meson older than v0.55.0, the above command will fail. To compile, use the following command:**

```
meson setup --cross-file cross_arm.txt build_arm
ninja -C build_arm openrtx_MODEL -jN
```

Where N is the number of cores that you want to allocate to the build process.

---

## Compiling for Linux

The software can be compiled with:

```
meson setup build_linux
meson compile -C build_linux openrtx_linux
```

**NOTE: if you are using a version of Meson older than v0.55.0, the above command will fail. To compile, use the following command:**

```
meson setup build_linux
ninja -C build_linux openrtx_linux -jN
```

Where N is the number of cores that you want to allocate to the build process.

#### Compiling with address sanitizer
During development it may be helpful to turn on the address sanitizer: the `asan` tool can help you spot buffer overflow by printing extra info after crashes. Keep in mind that `asan` produces a slower build of OpenRTX and thus should be used only during development and testing.

To compile with `asan` delete the current _build_linux_ directory and issue the following command:
```
meson setup build_linux -Dasan=true
```
Then follow the same compilation procedure listed above for a plain build.

#### Compiling on Alpine Linux / PostmarketOS
We successfully compiled OpenRTX on a Pine64 PinePhone running PostmarketOS (based on Alpine Linux). To install the build dependencies on PostmarketOS run the following command:
```
sudo apk add git meson build-base sdl2-dev
```
---

## Flashing the firmware to a radio

#### Tytera and Retevis radios
To flash either a locally compiled firmware or a pre-build image to a Tytera or Retevis radio, the _radio\_tool_ program is used. To use it from your non-root Linux user, you need to provide the correct permissions for user-space USB access:

```
sudo cp 99-openrtx.rules /etc/udev/rules.d
sudo udevadm control --reload-rules
```
then disconnect and reconnect the radio if you already have it connected to the computer.

If the compilation terminated without errors, to flash the obtained binary image you first have to connect the radio to the computer using the USB programming cable and put it in firmware upgrade mode. Then, to flash the firmware, issue the following command:

```
meson compile -C build_arm openrtx_MODEL_flash
```

Once the flashing process terminated without errors, you can power cycle your radio and enjoy the OpenRTX firmware!

You can also use _radio\_tool_ to flash a pre-built binary, for example the ones available on the repository's [releases page](https://github.com/OpenRTX/OpenRTX/releases). To do so, issue the following command:

```
radio_tool -d 0 -f -i new_firmware.bin
```

Substituting _new\_firmware_ with the name of the binary image you want to flash.

#### GD-77 and DM-1801

Currently, the Radioddity GD77 and Baofeng DM1801 devices are not supported by _radio\_tool_, thus you need to have the OpenGD77 wrapping and flashing tools in your _PATH_.
To have so, you need to add the following paths from the OpenGD77 repository in your _PATH_ environment variable:

- `/OpenGD77/tools/Python/FirmwareLoader`
- `/OpenGD77/firmware/tools`

Then, you can simply use the following command to compile and flash OpenRTX to your radio:

```
meson compile -C build_arm openrtx_MODEL_flash
```

#### LILYGO T-TWR Plus

This radio is based on ESP32 and currently is not yet integrated into `radio_tool`, therefore `west` will be used for flashing, please refer to [Compiling a Zephyr-based target](#compiling-a-zephyr-based-target) for instructions on how to install the required tools.

Once you have flashed MCUBOOT on your radio and obtained an OpenRTX image, you can flash it with `west flash`.

---

## Running on Linux

To run OpenRTX on Linux you simply have to execute the binary `build_linux/openrtx_linux`, which was compiled with the instructions above.

# Building OpenRTX from sources


1. [Installing the basic tools](#installing-the-basic-tools)
    - [Linux toolchain setup](#linux-toolchain-setup)
    - [Windows toolchain setup](#windows-toolchain-setup)
    - [MacOS toolchain setup](#macos-toolchain-setup)
2. [Obtaining the source code](#obtaining-the-source-code)
3. [Compiling the firmware](#compiling-the-firmware)
    - [Compiling for a radio](#compiling-for-a-radio)
    - [Compiling the emulator](#compiling-the-emulator)
4. [Flashing the firmware to a radio](#flashing-the-firmware-to-a-radio)
    - [Tytera and Retevis radios](#tytera-and-retevis-radios)
    - [GD-77 and DM-1801](#gd-77-and-dm-1801)
    - [Other radios](#other-radios)

[Appendix: compiling for a Zephyr-based target](#Compiling-for-a-Zephyr--based-target)
    
## 1. Installing the basic tools

As a general rule, the basic tools required to compile OpenRTX from the sources are: `git`, `python`, the `meson` build system and the GCC toolchain for the miosix kernel.
This rule has two exceptions:
1. the linux emulator, which doesn't the miosix GCC toolchain since the compiler shipped with your distribution is sufficient;
2. the T-TWR Plus, which requires the Zephyr RTOS build system.

#### Linux toolchain setup

All the required tools, except for the miosix toolchain, can be installed using the package manager provided by your Linux distribution.

E.g. on Debian/Ubuntu and derived distributions you can use:

```
sudo apt update && sudo apt install git pkg-config build-essential meson
```

Alternatively to the package manager, `meson` and `ninja` can be installed also using pip:

```
sudo apt install python-pip3
pip3 install --user meson ninja
```

The miosix kernel GCC toolchain is required, comes with its own installation program: download the installer and run it. The installer will ask for your root password to copy the compiler to the `/opt/arm-miosix-eabi` directory, and put symlinks to `/usr/bin`. 

```
wget https://miosix.org/toolchain/MiosixToolchainInstaller.run
sh MiosixToolchainInstaller.run
```
**WARNING: since the latest release, the GCC toolchain for miosix kernel is compatible only with x64 systems!**

The toolchain also provides an uninstall script, which can be found in the installation directory.

If you're targeting a TYT or Retevis radio, (i.e. the MD-380, MD-UV380, RT3, ...) you also need a tool used to encrypt and flash the binary executables called [radio_tool](https://github.com/v0l/radio_tool). The compilation script will automatically detect if _radio\_tool_ is already installed in the system and, if this is not the case, it will download the sources and compile automatically a local copy of the program. For the compilation process to succeed _cmake_ and _libusb_ must be present in the system: to install them, use the system package manager. On Debian/Ubuntu and derived distributions the command is:

```
sudo apt install cmake libusb-1.0-0 libusb-1.0-0-dev
```

To use `radio_tool` from your non-root Linux user, you need to provide the correct permissions for user-space USB access:
```
sudo cp 99-openrtx.rules /etc/udev/rules.d
sudo udevadm control --reload-rules
```

To flash the firmware on the *Module 17* and *CS7000-M17* (non Plus) platforms, `dfu-util` is required: this tool can be installed using the system package manager.

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

---

#### Windows toolchain setup

On Windows is possible only to compile the binary images for the radios, the Linux emulator will not work. To set up the toolchain follow these steps:

- Install `git` from [here](https://git-scm.com/download/win)
- Install `perl` from [here](https://strawberryperl.com/)
- Install `python` from [here](https://www.python.org/downloads/)
- Install the Miosix Toolchain from [here](https://miosix.org/wiki/index.php?title=Miosix_Toolchain)

Install `meson` and `ninja` using pip by running these commands in a PowerShell:

```
pip3 install --user meson ninja
```

If you're targeting a TYT or Retevis radio, (i.e. the MD-380, MD-UV380, RT3, ...) you also need to install [radio_tool](https://github.com/v0l/radio_tool), which is needed to encrypt and flash the binary executables. To install it, download the windows archive from [this link](https://github.com/v0l/radio_tool) and extract it in a folder of your choice. Add _radio\_tool_ to your _PATH_ environment variable by running the following command in a PowerShell:

```
 $env:PATH += ";path-to-radio_tool"
```

To verify that radio_tool is in PATH, this should not return an error

```
 get-command radio_tool
```

To flash the firmware on the *Module 17* and *CS7000-M17* (non Plus) platforms, `dfu-util` is required: the installer is available at [this link](https://dfu-util.sourceforge.net/releases/).

---

#### MacOS toolchain setup

The MacOS toolchain setup is similar to that for Linux. Currently, the Zephyr-based tagets can't be built on MacOS. 

To install the basic tools required to compile both the Linux emulator and the firmware images, you can use [Homebrew](https://brew.sh/). Most developers will already have it installed, otherwise follow the instructions at the link. Git and the Xcode tools are automatically installed during the Homebrew installation process, while `meson` needs to be installed separately with the following command:

```
brew install pkg-config meson
```

There is no miosix package in Homebrew: to install it you need to download and run the MacOS installer for your Mac's architecture (Intel or ARM) from https://miosix.org/wiki/index.php?title=Miosix_Toolchain#Latest_Stable_version_of_the_Miosix_Toolchain.

If you're targeting a TYT or Retevis radio, (i.e. the MD-380, MD-UV380, RT3, ...) you also need to install [radio_tool](https://github.com/v0l/radio_tool), which is needed to encrypt and flash the binary executables. The compilation script will automatically detect if `radio_tool` is already installed in the system and, if this is not the case, it will download the sources and compile automatically a local copy of the program. For the compilation process to succeed `cmake` and `libusb` must be present in the system. 

```
brew install cmake libusb
```

To flash the firmware on the *Module 17* and *CS7000-M17* (non Plus) platforms, `dfu-util` is required: this tool can be installed using homebrew.

```
brew install dfu-util
```

**Additional notes:**

The gd77 and dm1801 targets use `bin2sgl`. In order to run it on Apple Silicon Macs, you may need to install Rosetta 2 with the following command:

```
softwareupdate --install-rosetta --agree-to-license
```

---

## 2. Obtaining the source code

To obtain the OpenRTX source code, clone the github repository with the following command:

```
git clone https://github.com/OpenRTX/OpenRTX
```

The sources can also be downloaded as a zip file from the main page of the repository.

---

## 3. Compiling the firmware

#### Compiling for a radio

To build the firmware binary issue the following commands:

```
meson setup --cross-file CROSS_FILE build_arm
meson compile -C build_arm openrtx_TARGET
```

Where `CROSS_FILE` and `TARGET` have to be properly selected basing on the radio model:

| Radio model                                  | `CROSS_FILE`  | `TARGET`  |
| ---------------------------------------------|:-------------:|:--------: |
| TYT MD-380, MD-390, Retevis RT3, Retevis RT8 | cross_arm.txt | `md3x0`   |
| TYT MD-UV380, MD-UV390, Retevis RT3s         | cross_arm.txt | `mduv3x0` |
| TYT MD-9600                                  | cross_arm.txt | `md9600`  |
| Radioddity GD-77                             | cross_arm.txt | `gd77`    |
| Baofeng DM1801                               | cross_arm.txt | `dm1801`  |
| Module17                                     | cross_arm.txt | `mod17`   |
| Connect Systems CS7000-M17                   | cross_arm.txt | `cs7000`  |
| Connect Systems CS7000-M17 Plus              | cross_cm7.txt | `cs7000p` |


**NOTE: if you are using a version of Meson older than v0.55.0, the above command will fail. To compile, use the following command:**

```
meson setup --cross-file cross_arm.txt build_arm
ninja -C build_arm openrtx_MODEL -jN
```

Where N is the number of cores that you want to allocate to the build process.

---

#### Compiling the emulator

The emulator version of OpenRTX can be compiled for both Linux and MacOS and requires the following additional packages:
* SDL2 development package
* Codec2 development package
* `readline` package

The package names depend on the package manager you use. On Debian/Ubuntu and derived distributions the command is:

```
sudo apt install libsdl2-dev libcodec2-dev libreadline-dev
```

while on MacOS it is:

```
brew install sdl2 codec2 readline
```

Once the required packages are installed, on Linux the emulator program can be compiled using the following command:

```
meson setup build_linux
meson compile -C build_linux openrtx_linux
```

On MacOS, the build command is similar to the one for Linux, but with an additional argument at the beginning:

```
MACOSX_DEPLOYMENT_TARGET=11 meson setup build_darwin
MACOSX_DEPLOYMENT_TARGET=11 meson compile -C build_darwin openrtx_linux
```

On MacOS you may also need to define some or all of the following environment variables (assuming Homebrew is configured to install into `/opt/homebrew`):

```
CFLAGS=-I/opt/homebrew/include
CPATH=/opt/homebrew/include
CPPFLAGS=-I/opt/homebrew/include
DYLD_FALLBACK_LIBRARY_PATH=/usr/local/lib:/opt/homebrew/lib:/usr/lib
HOMEBREW_CELLAR=/opt/homebrew/Cellar
HOMEBREW_PREFIX=/opt/homebrew
HOMEBREW_REPOSITORY=/opt/homebrew
LDFLAGS=-L/opt/homebrew/lib
LIBRARY_PATH=/opt/homebrew/lib
```

We successfully compiled OpenRTX also on a Pine64 PinePhone running PostmarketOS (based on Alpine Linux). To install the build dependencies on PostmarketOS run the following command:
```
sudo apk add git meson build-base sdl2-dev
```

Finally, to run the OpenRTX emulator program you simply have to execute the binary `build_linux/openrtx_linux`.

**Compiling with address sanitizer**
During development it may be helpful to turn on the address sanitizer: the `asan` tool can help you spot buffer overflow by printing extra info after crashes. Keep in mind that `asan` produces a slower build of OpenRTX and thus should be used only during development and testing.

To compile with `asan` delete the current _build_linux_ directory and issue the following command:
```
meson setup build_linux -Dasan=true
```
Then follow the same compilation procedure listed above for a plain build.

---

## 4. Flashing the firmware to a radio

#### Tytera and Retevis radios
To flash the newly compiled binary image connect the radio to the computer using the USB programming cable, put it in firmware upgrade mode and issue the following command:

```
meson compile -C build_arm openrtx_MODEL_flash
```

where `MODEL` is the same radio model used to compile the firmware, as listed in the table at point 2.

Once the flashing process terminated without errors, you can power cycle your radio and enjoy the OpenRTX firmware!

`radio_tool` can be used also to flash a pre-built binary image, for example the ones available on the repository's [releases page](https://github.com/OpenRTX/OpenRTX/releases). To do so, issue the following command:

```
radio_tool -d 0 -f -i new_firmware.bin
```

Substituting `new_firmware.bin` with the name of the binary image you want to flash.

#### GD-77 and DM-1801

Currently, the Radioddity GD77 and Baofeng DM1801 devices are not supported by `radio_tool`. However, the required tools used to flash the firmware are already included in the main OpenRTX repository. To flash the firmware, you just need to connect the radio to the computer, put it in firmware update mode and issue the following command:
```
meson compile -C build_arm openrtx_MODEL_flash
```

where `MODEL` is the same radio model used to compile the firmware, as listed in the table at point 2.

#### Other radios

The *Module17* and the *CS7000-M17* use the `dfu-util` tool to manage the firmware updates. To write a new firmware, put the target in firmware update mode, connect it to the computer and issue the following command:

```
meson compile -C build_arm openrtx_MODEL_flash
```

where `MODEL` is the same radio model used to compile the firmware, as listed in the table at point 2.

To write a new firmware on *CS7000-M17 Plus*, you have to use the Connect Systems' firmware update tool to write a "wrapped" binary image. This "wrapped" binary image can be obtained directly from the OpenRTX build system
by issuing the command:

```
meson compile -C build_arm openrtx_cs7000p_wrap
```

the resulting file will be located in the `build_arm` folder inside the main OpenRTX folder and will be named `openrtx_cs7000p_wrap.bin`

---

## Compiling for a Zephyr-based target

To setup the Zephyr RTOS build environment you need to install the following packages (their names might vary according to your Linux distribution):

- `zephyr-sdk`
- `python-west`
- `python-pyelftools`
- `python-cbor2`
- `python-intelhex`
- `python-requests`

[Dependency instructions for Fedora and other distros:](https://docs.zephyrproject.org/latest/develop/getting_started/installation_linux.html)

Once the build environment is ready, you can then move on to cloning the OpenRTX and Zephyr sources. With the following command you can clone both the repositories at once:
```
mkdir openrtx-build && cd $_
west init -m https://github.com/OpenRTX/OpenRTX
west update
source zephyr/zephyr-env.sh # You need to execute this for every new shell
```

To compile the firmware, run the following command from the OpenRTX root folder:

```
rm -rf build; meson setup build; meson compile -C build openrtx_ttwrplus_uf2
```

To flash the firmware, use:

```
rm -rf build; meson setup build; meson compile -C build openrtx_ttwrplus_flash
```

You can also check the USB serial console with:

```
west espressif monitor
```

You can debug a Zephyr target by installing `openocd-esp32` and placing
[the following udev rules](https://github.com/espressif/openocd-esp32/blob/master/contrib/60-openocd.rules) in the /etc/udev/rules.d folder.

You can start a GDB shell using:

```
west debug --openocd `which openocd-esp32openocd`
```

**Note:** you may need to flash MCUBOOT on the ESP32 before being able to write any firmware image. To do this, follow these commands:
```
cd $ZEPHYR_PATH/zephyr
source zephyr-env.sh
west build -p always -b esp32s3_devkitm --sysbuild samples/hello_world
west flash
```

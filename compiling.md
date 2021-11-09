# Compilation instructions

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

The sources, then, can be compiled either for one of the supported radios or to be executed on a linux machine. In this latter case, however, OpenRTX comes without radio support.

* [Compiling for a radio](#Compiling-for-radios)
* [Flashing a compiled firmware to a radio](#Flashing-a-compiled-firmware-to-your-radio)
* [Compiling in emulator mode on a linux machine](#Compiling-for-Linux)


## Compiling for radios

#### Toolchain installation

The tools required to compile the sources and obtain a flashable binary image are _meson_ build system, the GCC toolchain for the miosix kernel and `cmake` for compiling the external tools for flashing the radio.

Install `cmake` using the package manager provided with your linux distribution, e.g. on Debian/Ubuntu and derived distributions you can use:
```
sudo apt update && sudo apt install cmake
```

**WARNING: since the latest release, the GCC toolchain for miosix kernel is compatible only with x64 systems!**

Then, to install the toolchain, download the installer and run it: the installer will ask for your root password to copy the compiler to the `/opt/arm-miosix-eabi` directory, and put symlinks to `/usr/bin`.

```
wget https://miosix.org/toolchain/MiosixToolchainInstaller.run
sh MiosixToolchainInstaller.run
```

The toolchain also provides an uninstall script, which can be found in the installation directory.

Finally, an updated _meson_ tool can be installed through `pip3` package installer.

```
pip3 install meson --user
```

To flash the compiled binary on the radio, we will use `radio_tool`, but we are bundling that with the OpenRTX toolchain,
so there is no need to install it by hand.
If you have `radio_tool` installed in your system, make sure that it's version is greater or equal than v0.1.1.

Once you have set up the toolchain, you can build the firmware binary using the following commands:

```
meson setup --cross-file cross_arm.txt build_arm
meson compile -C build_arm openrtx_TARGET
```

Where `TARGET` has to be replaced with the correct build target, depending on your radio model:

- TYT MD-380, TYT MD-390, Retevis RT3, Retevis RT8 → `md3x0`
- TYT MD-UV380, Retevis RT3s → `mduv3x0`
- Radioddity GD-77 → `gd77`
- Baofeng DM1801 → `dm1801`

If you are using a version of Meson older than v0.55.0, the above command will fail. To compile, use:

```
meson setup --cross-file cross_arm.txt build_arm
ninja -C build_arm openrtx_MODEL -jN
```

Where N is the number of cores that you want to allocate to the build process.

The firmware comes also with different testsuites: the instructions to compile a binary image starting from those, are available on a [dedicated page](tests.md).

## Flashing a compiled firmware to your radio

#### Tytera and Retevis radios
You can flash either the locally compiled firmware or a pre-build image to your radio by using the [radio_tool](https://github.com/v0l/radio_tool) software.

First of all, compile and install `radio_tool` using the following commands:
```
git clone https://github.com/v0l/radio_tool
cd radio_tool
mkdir build && cd build
cmake ..
make -j4
sudo make install
```
For more information on `radio_tool` visit its repository at: https://github.com/v0l/radio_tool

To use `radio_tool` from your non-root Linux user, you need to provide the correct permissions for user-space USB access:

```
sudo cp 99-openrtx.rules /etc/udev/rules.d
sudo udevadm control --reload-rules
```

then disconnect and reconnect the radio if you already have it connected to the computer.

If the compilation terminated without errors, to flash the obtained binary image you first have to connect the radio to the computer using the USB programming cable and put it in DFU (or recovery) mode. Then, to flash the firmware, issue the following command:

```
meson compile -C build_arm openrtx_MODEL_flash
```

Once the flashing process terminated without errors, you can power cycle your radio and enjoy the new breath of freedom!

You can also use `radio_tool` to flash a pre-built binary, for example the ones available on the repository's [releases page](https://github.com/OpenRTX/OpenRTX/releases). To do so, issue the following command:

```
radio_tool -d 0 -f -i new_firmware.bin
```

Substituting `new_firmware` with the name of the binary image you want to flash.

#### GD-77 and DM-1801

Currently, the Radioddity GD77 and Baofeng DM1801 devices are not supported by `radio_tool`, thus you need to have the OpenGD77 wrapping and flashing tools in your `PATH`.
To have so, you need to add the following paths from the OpenGD77 repository in your `PATH` environment variable:

- `/OpenGD77/tools/Python/FirmwareLoader`
- `/OpenGD77/firmware/tools`

Then, you can simply use the following command to compile and flash OpenRTX to your radio:

```
meson compile -C build_arm openrtx_MODEL_flash
```

## Compiling for Linux
To compile the Linux OpenRTX version you need the following packages: GCC, _meson_ and _libSDL_.
Their installation depends on which package manager you have: for example, on an Ubuntu machine the command is:

```
sudo apt install meson gcc pkg-config libsdl2-dev
```

while on Fedora it becomes:

```
sudo dnf install meson SDL2-devel
```

### Compiling on Alpine Linux / PostmarketOS
We successfully compiled OpenRTX on a Pine64 PinePhone running PostmarketOS (based on Alpine Linux). \
The same instructions should work on another device running PostmarketOS or Alpine Linux.
You can install the build dependencies on PostmarketOS with:
```
sudo apk add git meson build-base sdl2-dev
```

The software can be compiled with:

```
meson setup build_linux
meson compile -C build_linux openrtx_linux
```

If you are using a version of Meson older than v0.55.0, the above command will fail. To compile, use:

```
meson setup build_linux
ninja -C build_linux openrtx_linux -jN
```

Where N is the number of cores that you want to allocate to the build process.

#### Compiling with address sanitizer
During development it may be helpful to turn on the address sanitizer: the *asan* tool can help you spot buffer overflow by printing extra info after crashes.

Keep in mind that asan produces a slower build of OpenRTX and thus should be used only during development and testing.

To compile with asan delete the current `build_linux` directory and issue the following command:
```
meson setup build_linux -Dasan=true
```
Then follow the same compilation procedure for a plain build.

## Running on Linux

To run OpenRTX on Linux you simply have to execute the binary `build_linux/openrtx_linux`, which was compiled with the instructions above.

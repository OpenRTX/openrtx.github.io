# Compilation instructions

The first thing to do if you want to compile the OpenRTX firmware, is to obtain the source files by cloning the github repository. This can be done with the following command:

```
git clone https://github.com/OpenRTX/OpenRTX
```

Then, the toolchain software and the libraries required for the compilation process depend on whether you want to obtain a binary image to be flashed on a DMR radio or for the execution on an x86/x64 Linux machine.

## Compiling for DMR radios

To build the firmware binary you need to have the ARM GCC toolchain and the _meson_ tool installed on your computer.__
Their installation depends on which package manager you have: for example, on an Ubuntu machine the command is:

```
sudo apt install meson gcc-arm-none-eabi 
```

while on Fedora it becomes:

```
sudo dnf install meson arm-none-eabi-gcc-cs
```

An important aspect to keep in mind is that with some package managers installing the ARM GCC package does not automatically installs the _newlib_ library, providing a lightweight implementation of the standard C library. A symptom revealing that the newlib is not installed is having the compiler generating error messages relative to a not found `stdio.h` or similar headers.

In this case, you have to install newlib separately. For example, on Ubuntu, with:
```
sudo apt install libnewlib-arm-none-eabi
```

Once you have set up the toolchain, you can build the firmware binary using the following commands:

```
meson setup --cross-file cross_arm.txt build_arm
meson compile -C build_arm openrtx_TARGET
```

Where `TARGET` has to be replaced with the correct build target, depending on your radio model:

- TYT MD-380, TYT MD-390, Retevis RT3, Retevis RT8 → `md3x0`
- TYT MD-UV380, Retevis RT3s → `mduv380`
- Radioddity GD-77 → `gd77`
- Baofeng DM1801 → `dm1801`

If you are using a version of Meson older than v0.55.0, the above command will fail. To compile, use:

```
meson setup --cross-file cross_arm.txt build_arm
ninja -C build_arm openrtx_MODEL -jN
```
Where N is the number of cores that you want to allocate to the build process.

#### Compiling the platform test suites

OpenRTX comes with a set of test suites which can be used for testing and debugging the radio hardware and the relative low-level drivers. The sources of the test suites are located in the `tests/platform` folder inside the the repository's root directory. The majority of the test routines interact with user through the USB virtual serial port, which is automatically enabled on system boot.  
To compile a test suite, configure the toolchain with the following command:

```
meson configure -Dtest=FILENAME build_arm
```

Where `FILENAME` is the name of the test suite source file **without** the ".c" extension. Once the test suite source has been configured, you can obtain the binary image to be flashed on the radio by following the compilation procedure listed above.  
To restore everything to a clean situation, delete the `build_arm` folder.

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

To use `radio_tool` from your non-root Linux user, you need to provide the correct permissions for user-space USB access: to do so copy the [99-openrtx.rules](https://github.com/OpenRTX/OpenRTX/blob/master/99-openrtx.rules) udev rule file in `/etc/udev/rules.d/`, then disconnect and reconnect the radio if you already have it connected to the computer.

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

To run OpenRTX on Linux you need to change a system configuration parameter to allow the correct execution of the uC/OS-III RTOS.  
To do so, add the following line to `/etc/security/limits.conf` replacing `user` with your user name:
```
user - rtprio unlimited
```
Then, reboot your computer.

Now you can execute the binary `build_linux/openrtx_linux` you compiled with the instructions above.

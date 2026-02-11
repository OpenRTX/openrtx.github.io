When working on OpenRTX we often find in need to test some features or to execute some standalone programs without running the main OpenRTX code.
To do so we use the so called **platform tests** that can be used for example to dump the calibration data of a radio.

This page shows how to compile and run platform tests on your radio.

## Requirements
To run platform tests you need a working compiler toolchain installed, to do this follow the first section of the [compilation instructions](https://openrtx.org/#/compiling).

## Instructions
* Choose the platform test you want to run among those available in [OpenRTX/tests/platform](https://github.com/OpenRTX/OpenRTX/tree/master/tests/platform).
You can even use one of the available tests as a starting point to write your own!
* Delete any previous build folder
```
rm -rf build_*
```
* Setup the new build folder for flashing on your radio
```
meson setup --cross-file cross_cm4.txt build_cm4
```
* Specify the test you want to compile, omitting the `.c` extension.
For example to execute the `OpenRTX/tests/platform/mytestname.c` type:
```
meson configure -Dtest=mytestname build_cm4
```
* Compile and flash the test you selected.
Note that you have to choose the target corresponding to your radio model as explained in the [compilation instructions](https://openrtx.org/#/compiling)
```
meson compile -C build_cm4 openrtx_mytarget_flash
```
* Done!

## Switch from test to OpenRTX
To go back to compiling standard OpenRTX instead of a platform test you can type this command
```
meson configure -Dtest= build_cm4
```
Or simply delete your build folder with
```
rm -rf build_*
```

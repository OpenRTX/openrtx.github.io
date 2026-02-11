# Testing on Linux

While it poses limited to no utility for users, the Linux build may be very helpful for developers. 

## User interface testing

The `openrtx_linux` and `openrtx_linux_mod17` targets may be used to validate changes to the UI without having to have a physical device present.

## Testing Software-Decoded Baseband

The Linux build can demodulate a pre-recorded baseband file, which is useful for testing the receive path end-to-end without RF hardware.

### Sample baseband files

The `tests/unit/assets` directory contains the following baseband recordings:

| File                       | Use                                     |
|----------------------------|-----------------------------------------|
| `M17_test_baseband.raw`    | Runtime demodulation; src callsign only |
| `M17_test_baseband_dc.raw` | Runtime demodulation; src callsign only |


### Demodulating a raw baseband file with OpenRTX Linux

The Linux build uses a file source driver that reads, on loop, a raw 16-bit, little-endian, mono, 24 kHz file from `/tmp/baseband.raw`. Simply place the sample file in this location and launch the application in order for it to demodulate. Note that since the linux build has no audio driver, it is not expected that received audio can be heard.

```bash
# Copy the sample to the path the file-source driver reads from:
cp tests/unit/assets/M17_test_baseband.raw /tmp/baseband.raw
# Build
meson setup build_linux
meson compile -C build_linux openrtx_linux
# Launch
./build_linux/openrtx_linux
```

In this example, a receive screen with callsign OPNRTX is expected.

### Going Further

In theory, you could create a named pipe at `/tmp/baseband.raw` and write to it useing GNU Radio. Something like a source, with resampling to 24kHz if required, then float to short, then a file sink should work.

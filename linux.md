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

**Note:** These test samples have a sample rate of 48kHz, and must be decimated to 24kHz before they can be demodulated with OpenRTX on Linux. See [below](#decimation) for details.

### Demodulating a raw baseband file with OpenRTX Linux

The Linux build uses a file source driver that reads, on loop, a raw 16-bit, little-endian, mono, 24 kHz file from `/tmp/baseband.raw`. 

#### Decimation

The OpenRTX M17 modulator works at a sample rate of 48 kHz, but the demodulator works at a sample rate of 24 kHz. Because the sample baseband files have a sample rate of 48 kHz, they must be decimated to 24 kHz first in order to be demodulated. This can be done easily using the `sox` audio utility, available in most packagers. To decimate a sample baseband file, run:

```bash
# Convert the sample baseband recording from 48 kHz to 24 kHz
sox -r 48000 -e signed-integer -b 16 -c 1 tests/unit/assets/M17_test_baseband.raw \
    -r 24000 -e signed-integer -b 16 -c 1 M17_test_baseband_24k.raw \
    rate -v
```

Simply move the decimated file to `/tmp/baseband.raw` and launch the application in order for it to demodulate. Alternatively, you can set the output file path in `sox` to `/tmp/baseband.raw` directly and skip the copy step. 

```bash
# Copy the sample to the path the file-source driver reads from:
cp M17_test_baseband_24k.raw /tmp/baseband.raw

# Build
meson setup build_linux
meson compile -C build_linux openrtx_linux

# Launch
./build_linux/openrtx_linux
```

In this example, a receive screen with callsign OPNRTX is expected:

![Example demodulation](../_media/linux_emulator_baseband_demod.png)

**Note:** Since the Linux build has no audio driver, it is not expected that received audio can be heard.

### Going Further

In theory, you could create a named pipe at `/tmp/baseband.raw` and write to it using GNU Radio. Something like a source, with resampling to 24kHz if required, then float to short, then a file sink should work.

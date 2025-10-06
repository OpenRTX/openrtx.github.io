# Announcing 0.4.2 Release

By Ryan Turner (K0RET) • 2025-10-05

Today, OpenRTX maintainer Silvano Seva (IU2KWO) released OpenRTX version 0.4.2. This is primarily a maintenance release focusing on GPS, but there are other changes that you may notice too. First and foremost, download the release on [GitHub](https://github.com/OpenRTX/OpenRTX/releases/tag/v0.4.2), and follow [these steps](https://openrtx.org/#/user_guide?id=flashing-openrtx-to-your-radio) for how to flash it.

This release contains [63 commits between 8 contributors](https://github.com/OpenRTX/OpenRTX/compare/v0.4.1...v0.4.2) and, we think you'll love it! Next, let's dissect the release a bit and talk about all of its goodness.

## DC block filter

OpenRTX has, out of necessity, a DC filter on the mic input in processed modes so that the encoded signals are clean. Previously though, this filter resulted in noise being passed forward (and created!) when there were very low levels. This was heard as noisy artifacts during quiet times when transmitting M17.

Silvano addressed this by incorporating an [improved DSP algorithm](https://dspguru.com/dsp/tricks/fixed-point-dc-blocking-filter-with-noise-shaping/) that introduces noise shaping. In our testing, this significantly improved the amount of artifacts generated when using the M17 mode in typical uses. Note that this filter is applied on the transmitter's side, so it will affect how other people hear you -- not how you hear other people.

## Improvements to GPS

There were two main improvements to GPS this release. First, GPS drivers were refactored so that more capabilities can be leveraged. This was done in a way that limited maintenance overhead to ease supporting the existing radio platforms and make it easy to continue to add new ones. While these changes are "under the covers", they represent a substantial improvement in GPS handling, and along the way even a bug was found (and fixed).

With this done, plus some platform bugs squashed (thanks JKI757!), now GPS on the CS7000-M17 Plus radio is enabled!

A minor tweak too that you may notice if you don't have GPS on your radio: we now indicate that by showing "No GPS" on the screen rather than "GPS Off" or "No fix".

## Slight tweaks for RTC

Besides showing you where you are, the only GPS feature present today is to set the clock. The behavior for this however was a bit unintuitive, and we've addressed that now. When you enable setting time from GPS now, the setting stays on, and the time is periodically synced with the GPS time.

## UX Tweaks: Battery Percentage, FM settings, and voice prompts

The ability to show battery percentage rather than an icon was introduced (thanks Tarandeep Romana [VA1FOX]!), and this is available under the settings menu. Additionally, a new FM settings menu has been introduced so that options previously only available in the meta menu can be modified in the conventional way as well (thanks Imostlylurk!). Finally, some small tweaks to the displayed strings and the accessibility voice prompts were made, improving the overall coverage of the voice prompts.

## Developer experience and tech debt improvements

### A whole new style

OpenRTX has had a formally-adopted coding style for a long time, but it was not adopted throughout the project, and it had some eccentricities compared to more mainstream C and C++ projects. OpenRTX has formally adopted the Linux Kernel code format (with a few documented tweaks). An effort has begun to update all of the existing code to conform to the style, and we encourage you to help! Learn more in the related [GitHub issue](https://github.com/OpenRTX/OpenRTX/issues/346).

### Improved dev environment

Improvements were made to the [devcontainer](https://containers.dev/) so that it can successfully flash all supported platforms. Devcontainer is now a recommended way to setup your local dev environment. Native setup is still supported.

As part of this effort, the `bin2sgl` tool was ported to python (our preferred language for scripts) to make it more portable. This was our first documented AI-assisted contribution.

Thank you Peter Buchegger (OE5BPA), for your work on DX and tools! Also, thank you Grzegorz Kaczmarek (SP6HFE) for ensuring the DM1701 is easy to build for on VSCode.

### Improved platform tests

Silvano added platform tests for verifying a display, dumping the contents of the non‑volatile memory (NVM), and tools for printing the calibration data from MD3x0, MDUV3x0, GD77, and DM1801 platforms. Updates were made to other tests as part of routine maintenance as well.

Platform tests let OpenRTX contributors better support existing radios as well as expand to supporting new ones. Thank you Silvano for investing in this important part of the project!

### Hacktoberfest preparation and kick-off

Last but not least, improvements to the developer experience were made to facilitate OpenRTX's first Hacktoberfest participation. We've improved CICD to build each platform, ensuring that feedback about build problems is automatic. We've added a contributing guide. And we've seen our first Hacktoberfest contributions land!

If you're interested in contributing to OpenRTX, now is a great time to do so. And as always, drop in the chat and let us know how OpenRTX is working for you.

Cheers!

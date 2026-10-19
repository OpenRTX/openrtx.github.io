# Development status

OpenRTX is currently in active development. There will be bugs as we prioritize growth. Many features are still being built, so expect functionality to change regularly.

## Upcoming Milestones

- Firmware release v0.5.0 with persistence of settings and radio state on all the devices
- Backup and restore of external flash memory
- Codeplug support for FM and M17 operating modes

### Future

- Tytera MD-9600 support
- AX.25 and TNC support
- DMR RX, TX, and SMS support
- On-device APRS

## Current Support

**Last updated: 2026 October 14**

| Support Level                                      | Radio Family |
| -------------------------------------------------- | ------------ |
| Mainline code, [official builds][0]                | MD-3x0, MD-UV3x0, CS7000-M17 and plus, DM-1x01, GD-77, Module 17 |
| Mainline code, [nightly builds][1]                 | MD-9600, T-TWR Plus |
| Non-main repo, unofficial builds                   | [A36Plus][2] |
| WIP main repo branch, progress within last year    | [T-TWR Plus v2.1][3], [MD-UV3x0 Plus][4], [C62][5], [HD2][6] |
| WIP main repo branch, no progress within last year | FT-70D, RT-890, Micro17, MD-2017 |

[0]: https://github.com/OpenRTX/OpenRTX/releases
[1]: https://files.openrtx.org/nightly/
[2]: https://github.com/VR2TE/Talkpod-A36plus-Firmware/tree/main/A36plus%20MAX/OpenRTX
[3]: https://github.com/OpenRTX/OpenRTX/tree/ttwrplus21
[4]: https://github.com/OpenRTX/OpenRTX/tree/mduv3x0-plus
[5]: https://github.com/OpenRTX/OpenRTX/tree/Retevis-c62
[6]: https://github.com/OpenRTX/OpenRTX/tree/Ailunce-HD2

### Legend

- ❌ Not supported
- 🟡 In development
- ✅ Supported

### Basic Functions

| Radio model                    | Boot | Display | Keyboard | CPS | RTC | Persistence | GPS | Known Issues                                                                            |
| ------------------------------ | :--: | :-----: | :------: | :-: | :-: | :---------: | :-: | --------------------------------------------------------------------------------------- |
| Tytera MD-380 / MD-390         | ✅   | ✅      | ✅       | ✅  | ✅  | ✅          | ✅  | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:MD-3x0,ALL)       |
| Tytera MD-UV380 / Retevis RT3s | ✅   | ✅      | ✅       | ✅  | ✅  | ✅          | ✅  | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:MD-UV3x0,ALL)     |
| Radioddity GD-77               | ✅   | ✅      | ✅       | ✅  | N/A | ❌          | N/A | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:GD-77,ALL)        |
| Baofeng DM-1801                | ✅   | ✅      | ✅       | ✅  | N/A | ❌          | N/A | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:DM-1801,ALL)      |
| Tytera MD-9600                 | ✅   | ✅      | ✅       | ❌  | ✅  | ❌          | ❌  | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:MD-9600,ALL)      |
| Module 17                      | ✅   | ✅      | ✅       | ❌  | ✅  | ✅          | N/A | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:Module17,ALL)     |
| Lilygo T-TWR plus              | ✅   | ✅      | ✅       | ❌  | ✅  | ❌          | ✅  | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:T-TWR%20Plus,ALL) |
| Radtel RT-890                  | ✅   | 🟡      | ✅       | ❌  | N/A | ❌          | N/A | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:RT-890,ALL)       |
| Talkpod A36plus                | ✅   | ✅      | ✅       | ❌  | 🟡  | ✅          | N/A | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:A36plus,ALL)      |
| Retevis C62                    | ✅   | ✅      | ✅       | ❌  | ❌  | ❌          | N/A | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:retevis-c62,ALL)  |
| Connect Systems CS7000-M17     | ✅   | ✅      | ✅       | ❌  | N/A | ✅          | ✅  | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:CS7000,ALL)       |
| Connect Systems CS7000-M17 Plus| ✅   | ✅      | ✅       | ❌  | N/A | ✅          | ✅  | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:CS7000-Plus,ALL)  |

### Modes

| Radio model                                    |FM RX | FM TX | AM RX | M17 RX | M17 TX | APRS RX | APRS TX | DMR RX | DMR TX | DMR SMS |
| -----------------------------------------------|:---: | :---: | :---: | :----: | :----: | :-----: | :-----: | :----: | :----: | :-----: |
| Tytera MD-380 / MD-390 / Retevis RT3 **(VHF)** |✅    | ✅    | N/A   | ✅     | ❌     | ❌      | ❌      | ❌     | ❌     | ❌      |
| Tytera MD-380 / MD-390 / Retevis RT3 **(UHF)** |✅    | ✅    | N/A   | ✅     | ✅     | ❌      | ❌      | ❌     | ❌     | ❌      |
| Tytera MD-UV380 / Retevis RT3s                 |✅    | ✅    | N/A   | ✅     | ✅     | ❌      | ❌      | ❌     | ❌     | ❌      |
| Radioddity GD-77                               |✅    | ✅    | N/A   | N/A    | N/A    | ❌      | ❌      | ❌     | ❌     | ❌      |
| Baofeng DM-1801                                |✅    | ✅    | N/A   | N/A    | N/A    | ❌      | ❌      | ❌     | ❌     | ❌      |
| Tytera MD-9600                                 |❌    | ❌    | N/A   | ❌     | ❌     | ❌      | ❌      | ❌     | ❌     | ❌      |
| Module 17                                      |N/A   | N/A   | N/A   | ✅     | ✅     | N/A     | N/A     | N/A    | N/A    | N/A     |
| Lilygo T-TWR plus                              |✅    | ✅    | N/A   | ❌     | ❌     | ❌      | ❌      | N/A    | N/A    | N/A     |
| Radtel RT-890                                  |❌    | ❌    | N/A   | ❌     | ❌     | ❌      | ❌      | N/A    | N/A    | N/A     |
| Talkpod A36plus                                |✅    | ✅    | 🟡    | ❌     | ❌     | ❌      | ❌      | N/A    | N/A    | N/A     |
| Retevis C62                                    |✅    | ❌    | N/A   | ❌     | ❌     | ❌      | ❌      | ❌     | ❌     | ❌      |
| Connect Systems CS7000-M17                     |✅    | ✅    | N/A   | ✅     | ✅     | ❌      | ❌      | ❌     | ❌     | ❌      |
| Connect Systems CS7000-M17 Plus                |✅    | ✅    | N/A   | ✅     | ✅     | ❌      | ❌      | ❌     | ❌     | ❌      |


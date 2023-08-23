# Development status

OpenRTX is currently in active development. There will be bugs as we prioritize growth. Many features are still being built, so expect functionality to change regularly.

## Upcoming Milestones

### 2023-Q2

- Windows support for flashing firmware
- Improved audio support on Tytera MD-UV380 / Retevis RT3s

### Future

- Codeplug support for M17
- Tytera MD-9600 support
- AX.25 and TNC support
- DMR RX, TX, and SMS support
- On-device APRS

## Current Support

### Legend

- ❌ Not supported
- 🟡 In development
- ✅ Supported

### Basic Functions

| Radio model                    | Boot | Display | Keyboard | CPS | RTC | Persistence | GPS | Known Issues                                                                                                                     |
| ------------------------------ | :--: | :-----: | :------: | :-: | :-: | :---------: | :-: | -------------------------------------------------------------------------------------------------------------------------------- |
| Tytera MD-380 / MD-390         |  ✅  |   ✅    |    ✅    | ✅  | ✅  |     ✅      | ✅  | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:"platform::md3x0","platform::all")                         |
| Tytera MD-UV380 / Retevis RT3s |  ✅  |   ✅    |    ✅    | ✅  | ✅  |     ✅      | ✅  | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:%22platform::mduv3x0%22,%22platform::all%22)               |
| Radioddity GD-77               |  ✅  |   ✅    |    ✅    | ✅  | N/A |     ❌      | N/A | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:%22platform::gd77%22,%22platform::all%22)                  |
| Baofeng DM-1801                |  ✅  |   ✅    |    ✅    | ✅  | N/A |     ❌      | N/A | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is%3Aopen+label%3A%22platform%3A%3Adm1801%22%2C%22platform%3A%3Aall%22+) |
| Tytera MD-9600                 |  ✅  |   ✅    |    ✅    | ❌  | ✅  |     ❌      | ❌  | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is%3Aopen+label%3A"platform%3A%3Amd9600"%2C"platform%3A%3Aall")          |

### Modes

| Radio model                    | FM RX | FM TX | M17 RX | M17 TX | APRS RX | APRS TX | DMR RX | DMR TX | DMR SMS |
| ------------------------------ | :---: | :---: | :----: | :----: | :-----: | :-----: | :----: | :----: | :-----: |
| Tytera MD-380 / MD-390         |  ✅   |  ✅    |   ✅    |   ✅   |   ❌     |   ❌     |   ❌   |   ❌    |   ❌    |
| Tytera MD-UV380 / Retevis RT3s |  ✅   |  ✅    |   ✅    |   ✅   |   ❌     |   ❌     |   ❌   |   ❌    |   ❌    |
| Radioddity GD-77               |  ✅   |  ✅    |   ❌    |   ❌   |   ❌     |   ❌     |   ❌   |   ❌    |   ❌    |
| Baofeng DM-1801               |  ✅   |  ✅    |   ❌    |   ❌   |   ❌     |   ❌     |   ❌   |   ❌    |   ❌    |
| Tytera MD-9600                 |  ❌   |  ❌    |   ❌    |   ❌    |   ❌    |   ❌     |   ❌    |  ❌    |   ❌    |

_Tytera MD-9600 support is not yet complete, and as a result pre-made builds are not available._

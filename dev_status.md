# Development status

OpenRTX is currently in active development. There will be bugs as we prioritize growth. Many features are still being built, so expect functionality to change regularly.

## Upcoming Milestones

- Firmware release v0.4.0 with support for CS7000-M17 (Plus) and DM-1701
- Backup and restore of external flash memory
- Persistence of settings and radio state where missing
- Codeplug support for FM and M17 operating modes

### Future

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
| ------------------------------ | :--: | :-----: | :------: | :-: | :-: | :---------: | :-: | ------------------------------------------------------------------------------------ |
| Tytera MD-380 / MD-390         |  ✅  |   ✅     |    ✅     | ✅  | ✅   |     ✅      | ✅  | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:MD-3x0,ALL)     |
| Tytera MD-UV380 / Retevis RT3s |  ✅  |   ✅     |    ✅     | ✅  | ✅   |     ✅      | ✅  | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:MD-UV3x0,ALL)   |
| Radioddity GD-77               |  ✅  |   ✅     |    ✅     | ✅  | N/A |     ❌      | N/A | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:GD-77,ALL)      |
| Baofeng DM-1801                |  ✅  |   ✅     |    ✅     | ✅  | N/A |     ❌      | N/A | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:DM-1801,ALL)    |
| Tytera MD-9600                 |  ✅  |   ✅     |    ✅     | ❌  | ✅   |     ❌      | ❌  | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:MD-9600,ALL)    |
| Module 17                      |  ✅  |   ✅     |    ✅     | ❌  | ✅   |     ✅      | N/A | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:Module17,ALL)    |
| Lilygo T-TWR plus              |  ✅  |   ✅     |    ✅     | ❌  | ✅   |     ❌      | ✅  | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:T-TWR%20Plus,ALL)|
| Radtel RT-890                  |  ✅  |   🟡     |    ✅     | ❌  | N/A |     ❌      | N/A  | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:RT-890,ALL)    |
| Talkpod A36plus                |  ✅  |   ✅     |    ✅     | ❌  | 🟡   |     ✅     | N/A  | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:A36plus,ALL)    |
| Retevis C62                    |  ✅  |   ✅     |    ✅     | ❌  | ❌   |     ❌     | N/A  | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:retevis-c62,ALL)    |
| Connect Systems CS7000-M17     |  ✅  |   ✅     |    ✅     | ❌  | N/A  |     ✅     | ✅   | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:CS7000,ALL)            |
| Connect Systems CS7000-M17 Plus|  ✅  |   ✅     |    ✅     | ❌  | N/A  |     ✅     | ❌   | [on GitHub](https://github.com/OpenRTX/OpenRTX/issues?q=is:open+label:CS7000-Plus,ALL)|

### Modes

| Radio model                    | FM RX | FM TX | M17 RX | M17 TX | APRS RX | APRS TX | DMR RX | DMR TX | DMR SMS |
| ------------------------------ | :---: | :---: | :----: | :----: | :-----: | :-----: | :----: | :----: | :-----: |
| Tytera MD-380 / MD-390         |  ✅   |  ✅    |   ✅    |   ✅   |   ❌     |   ❌     |   ❌   |   ❌    |   ❌     |
| Tytera MD-UV380 / Retevis RT3s |  ✅   |  ✅    |   ✅    |   ✅   |   ❌     |   ❌     |   ❌   |   ❌    |   ❌     |
| Radioddity GD-77               |  ✅   |  ✅    |   N/A  |  N/A   |   ❌    |   ❌     |   ❌   |   ❌     |   ❌    |
| Baofeng DM-1801                |  ✅   |  ✅    |   N/A  |  N/A   |   ❌    |   ❌     |   ❌   |   ❌     |   ❌    |
| Tytera MD-9600                 |  ❌   |  ❌    |   ❌    |   ❌    |   ❌    |   ❌     |   ❌    |  ❌     |   ❌    |
| Module 17                      |  N/A |  N/A   |   ✅    |   ✅    |   N/A  |  N/A    |  N/A   |  N/A   |   N/A   |
| Lilygo T-TWR plus              |  ✅   |  ✅    |   ❌    |   ❌    |   ❌    |   ❌     |  N/A   |  N/A   |   N/A   |
| Radtel RT-890                  |  ❌   |  ❌    |   ❌    |   ❌    |   ❌    |   ❌     |   ❌    |  ❌     |   ❌    |
| Talkpod A36plus                |  ✅   |  ✅    |   ❌    |   ❌    |   ❌    |   ❌     |   ❌    |  ❌     |   ❌    |
| Retevis C62                    |  ✅   |  ❌    |   ❌    |   ❌    |   ❌    |   ❌     |   ❌    |  ❌     |   ❌    |
| Connect Systems CS7000-M17     |  ✅   |  ✅    |   ✅    |   ✅   |   ❌     |   ❌     |   ❌   |   ❌    |   ❌     |
| Connect Systems CS7000-M17 Plus|  ✅   |  ✅    |   ✅    |   ✅   |   ❌     |   ❌     |   ❌   |   ❌    |   ❌     |


### Notes:
* _Tytera MD-9600 support is not yet complete, and as a result pre-made builds are not available._
* _Similarly, Radtel RT-890 support is not yet complete either, and same as above, pre-made builds are not available._
* _Semi-regular pre-made builds for the Talkpod A36plus can be found [here.](https://github.com/VR2TE/Talkpod-A36plus-Firmware/tree/main/A36plus%20MAX/OpenRTX)_

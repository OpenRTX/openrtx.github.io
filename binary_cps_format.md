# OpenRTX Binary Codeplug Format

Hannes Matuschek DM3MAT  
Niccolò Izzo IU2KIN  
Silvano Seva IU2KWO  

## Copyright notice

Copyright (c) 2023 the people and organizations identified as authors. All rights reserved.

## Introduction

The OpenRTX Binary Codeplug Format (OBCF) is a binary data format for storing radio configurations in an easy-to-use and platform-agnostic way while providing support for modern amateur-radio use cases like M17. This format is utilized in radio codeplugs constructed using a customer programming software (CPS) and loaded directly to radio transceivers.

### Objectives

- Be open and free
- Support common amateur-radio needs for communicating both directly and utilizing infrastructure like repeaters or hotspots
- Support memory channel use cases for analog FM, DMR Tier II, and M17 modes
- Provide portability across various hardware platforms, both for end users and for firmware developers

## Version control

OBCF observed [Semantic Versioning v2.0.0](https://semver.org/spec/v2.0.0.html). This document is at **v0.1.0** of the standard presently. Errata not resulting in substantially different understandings, capabilities, or definitions may be treated as un-versioned revisions of this document without incrementing the patch versions. A detailed history of changes for this document may be viewed [on GitHub](https://github.com/OpenRTX/openrtx.github.io/commits/master/binary_cps_format.md).

## Specification

The OBCF format defines a binary file with the following structure:

```
 (MSB)                                 (LSB)
+------+--------+--------+------------+-----+
|header|contacts|channels|bank offsets|banks|
+------+--------+--------+------------+-----+
```

These 5 sections are further described in the sections below. The file extension is `.rtxc`.

### Header

The header contains metadata about the codeplug to ensure compatibility with interpreting software and identifying information about the codeplug to simplify identification.

#### Field definition

| Field Name     | Data Type | Description                                                                                                                                                     |
| -------------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| magic          | uint32_t  | Number used to identify the start of a codeplug; this is always "RTXC", i.e. `0x43585452`                                                                       |
| version_number | uint32_t  | Major, minor and bufgix version of the OBCF standard, constructed as the `(CPS_VERSION_MAJOR << 16) \| (CPS_VERSION_MINOR << 8) \| (CPS_VERSION_FIX)`. Refer to [Version control](#version-control). |
| author         | char[32]  | User-provided author of the codeplug                                                                                                                            |
| desc           | char[32]  | User-provided description of the codeplug                                                                                                                       |
| timestamp      | uint64_t  | Unix timestamp of when the codeplug was last edited                                                                                                             |
| ct_count       | uint16_t  | Number of stored contacts, used for memory offsets                                                                                                              |
| ch_count       | uint16_t  | Number of stored channels, used for memory offsets                                                                                                              |
| b_count        | uint16_t  | Number of stored banks                                                                                                                                          |

#### Layout

This structure is the beginning of the file. The fields are laid out in the following manner:

```
      00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F
0000 |<-magic------>|<-version->|<-author------------
0010  -----------------------------------------------
0020  ------------------------->|<-descr-------------
0030  -----------------------------------------------
0040  ------------------------->|<-timestamp-------->
0050 |<ct_c|<ch_c|<b_co|
```

### Contacts

#### Field definition

| Field name | Data Type                                                                                        | Description                                                              |
| ---------- | ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------ |
| name       | char[32]                                                                                         | Display name for the contact                                             |
| mode       | uint8_t                                                                                          | Mode that the contact is intended to be used for                         |
| info       | [m17Contact_t](#m17contact_t-type-description) or [dmrContact_t](#dmrcontact_t-type-description) | Either contains the m17 info or the dmr info, as described further below |

#### Mode lookup table

|                     Bits | Value    | Notes                                 |
| -----------------------: | -------- | ------------------------------------- |
|             `0b00000000` | None     | Indeterminate state for compatibility |
|             `0b00000001` | FM       | Only used for channels                |
|             `0b00000010` | DMR      |                                       |
|             `0b00000011` | M17      |                                       |
| `0b00000100..0b11111111` | Reserved |                                       |

#### DMR contact type lookup table

|   Bits | Value           |
| -----: | --------------- |
| `0b00` | Group contact   |
| `0b01` | Private contact |
| `0b10` | Broadcast call  |
| `0b11` | Reserved        |

#### m17Contact_t type description

| Field   | Data Type  | Description                                                                                                                                          |
| ------- | ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| address | uint8_t[6] | M17 address stored in 48 bits, following their [address encoding](https://spec.m17project.org/pdf/M17_spec.pdf#appendix.A)                           |

#### dmrContact_t type description

| Field            | Data Type | Description                                                                                          |
| ---------------- | --------- | ---------------------------------------------------------------------------------------------------- |
| id               | uint32_t  | DMR ID                                                                                               |
| contact_settings | uint8_t   | Bit 0:1 contact type. Bit 2 rx tone enable/disable. Bit 3:7 reserved                                 |

#### Layout

If there are contacts, this portion will be the second portion of the file. The memory locations listed below are relative to the end of the previous block, being either the header or a previous contact. The fields depend on the type of contact.

For M17 contacts, this section is laid out in the following manner:

```
      00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F
0000 |<-name-----------------------------------------
0010  ---------------------------------------------->|
0020 |<m|<-address------->|
```

For DMR contacts, this section is laid out in the following manner:

```
      00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F
0000 |<-name-----------------------------------------
0010  ---------------------------------------------->|
0020 |<m|<-id------>|<i|  |
```

### Channels

#### Field descriptions

| Field name      | Data Type                                                                                                                     | Description                                                                                                                                               |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| mode            | uint8_t                                                                                                                       | Operating mode; refer to [Mode lookup table](#mode-lookup-table)                                                                                          |
| traits          | uint8_t                                                                                                                       | First two bits are channel bandwidth (refer to [Bandwidth lookup table](#bandwidth-lookup-table)), then one bit indicating true if the channel is RX only |
| power           | uint32_t                                                                                                                      | transmit power, in mW
| rx_frequency    | uint32_t                                                                                                                      | RX frequency, in Hz                                                                                                                                        |
| tx_frequency    | uint32_t                                                                                                                      | TX frequency, in Hz                                                                                                                                        |
| name            | char[32]                                                                                                                      | display name for channel                                                                                                                                  |
| descr           | char[32]                                                                                                                      | Description of the channel                                                                                                                                |
| ch_location     | [geo_t](#geo_t-type-description)                                                                                              | transmitter location                                                                                                                                      |
| infoblock       | [fmInfo_t](#fminfo_t-type-description), [dmrInfo_t](#dmrinfo_t-type-description), or [m17Info_t](#m17info_t-type-description) | Information block for the channel                                                                                                                         |

#### Bandwidth lookup table

| Bits | Bandwidth (kHz) |
| ---- | --------------- |
| 0b00 | 12.5            |
| 0b01 | 20              |
| 0b10 | 25              |
| 0b11 | Reserved        |

#### geo_t type description

| Field       | Data Type | Description                                                                                                                                                        |
| ----------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| ch_lat_int  | int8_t    | Floor of latitude (⌊x⌋; e.g. given latitude of 44.493889, this value is 44)                                                                                        |
| ch_lat_dec  | uint16_t  | Fractional part of latitude, first rounded to the ten-thousandths, represented as a positive whole number (e.g. given latitude of 44.493889, this value is 4939)   |
| ch_lon_int  | int16_t   | Floor of longitude (⌊x⌋; e.g. given longitude of 11.342778, this value is 11)                                                                                      |
| ch_lon_dec  | uint16_t  | Fractional part of longitude, first rounded to the ten-thousandths, represented as a positive whole number (e.g. given longitude of 11.342778, this value is 3428) |
| ch_altitude | uint16_t  | Altitude of the center of the radiator of the transmitter, stored in meters MSL offset +500 (e.g. 0m would be stored as 500)                                       |

#### fmInfo_t type description

| Field  | Data Type | Description                                                                                                                                                              |
| -------|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| rxTone | uint8_t   | Bit 0:6 index of the CTCSS tone for receive squelch as in the [CTCSS Code Frequencies Lookup Table](#ctcss-code-frequencies-lookup-table). Bit 7 enable/disable receive tone squelch. (e.g. disabled 173.8 Hz tone value is `0b00011111`) |
| txTone | uint8_t   | Bit 0:6 index of the CTCSS tone to be transmitted as in the [CTCSS Code Frequencies Lookup Table](#ctcss-code-frequencies-lookup-table). Bit 7 enable/disable tone transmission.(e.g. enabled 107.2 tone value is `0b10001110)` |

#### dmrInfo_t type description

| Field         | Data Type | Description                                                                                                                                                            |
| ------------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| colorCode     | uint8_t   | RX and TX colour codes used, as defined by [ETSI TS 102 361-1 Table 9.18](https://www.etsi.org/deliver/etsi_ts/102300_102399/10236101/02.02.01_60/ts_10236101v020201p.pdf). <br> Bit 0:3 RX colour code. Bit 4:7 TX colour code. (e.g. RX colour code 0 and TX colour code 15 would be represented as `0b00001111`)                                                                   |
| dmr_timeslot  | uint8_t   | Timeslot being used, represented in integer form (e.g. timeslot 2 is `0b00000010`)                                                                                     |
| contact_index | uint16_t  | Index to retrieve contact from list for reverse lookups                                                                                                                |

#### m17Info_t type description

| Field         | Data Type | Description                                                                                                                                                                                                                                    |
| ------------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| can           | uint8_t   | RX and TX channel access numbers (CANs) used, as defined by [M17 Specification section 3.1.3](https://spec.m17project.org/), with the RXvalue represented in the first half (e.g. an RX CAN of 0 and TX CAN of 2 is represented as 0b00000010) |
| mode_encr     | uint8_t   | Channel operation mode and encryption mode, as defined by [M17 channel modes lookup table](#m17-channel-modes-lookup-table) and [M17 channel encryption lookup table](#m17-channel-encryption-lookup-table)                                    |
| gps_mode      | uint8_t   | Boolean whether GPS position should be embedded in transmit payload                                                                                                                                                                            |
| contact_index | uint16_t  | Index to retrieve contact from list for reverse lookups                                                                                                                                                                                        |

#### M17 channel modes lookup table

|             Bits | Value                  |
| ---------------: | ---------------------- |
|         `0b0000` | Reserved               |
|         `0b0001` | Digital voice          |
|         `0b0010` | Digital data           |
|         `0b0011` | Digital voice and data |
| `0b0100..0b1111` | Reserved               |

#### M17 channel encryption lookup table

|             Bits | Value     |
| ---------------: | --------- |
|         `0b0000` | Plain     |
|         `0b0001` | AES-256   |
|         `0b0010` | Scrambler |
| `0b0011..0b1111` | Reserved  |

#### CTCSS

CTCSS code frequency support is implemented as the 39 tones defined in ANSI/TIA-603-E-2016 1.3.5.2 plus 11 additional non-standard tones.

##### CTCSS Code Frequencies Lookup Table

<table style="border: none"> <tr style="border: none"></tr>
<td style="border: none; vertical-align: top;">

| Index | Frequency (Hz) |
| ----: | -------------- |
|     0 | 67.0           |
|     1 | 69.3           |
|     2 | 71.9           |
|     3 | 74.4           |
|     4 | 77.0           |
|     5 | 79.7           |
|     6 | 82.5           |
|     7 | 85.4           |
|     8 | 88.5           |
|     9 | 91.5           |
|    10 | 94.8           |
|    11 | 97.4           |
|    12 | 100.0          |
</td><td style="border: none; vertical-align: top;">

| Index | Frequency (Hz) |
| ----: | -------------- |
|    13 | 103.4          |
|    14 | 107.2          |
|    15 | 110.9          |
|    16 | 114.8          |
|    17 | 118.8          |
|    18 | 123.0          |
|    19 | 127.3          |
|    20 | 131.8          |
|    21 | 136.5          |
|    22 | 141.3          |
|    23 | 146.2          |
|    24 | 151.4          |
|    25 | 156.7          |
</td><td style="border: none; vertical-align: top;">

| Index | Frequency (Hz) |
| ----: | -------------- |
|    26 | 159.8          |
|    27 | 162.2          |
|    28 | 165.5          |
|    29 | 167.9          |
|    30 | 171.3          |
|    31 | 173.8          |
|    32 | 177.3          |
|    33 | 179.9          |
|    34 | 183.5          |
|    35 | 186.2          |
|    36 | 189.9          |
|    37 | 192.8          |
|    38 | 196.6          |
</td><td style="border: none; vertical-align: top;">

| Index | Frequency (Hz) |
| ----: | -------------- |
|    39 | 199.5          |
|    40 | 203.5          |
|    41 | 206.5          |
|    42 | 210.7          |
|    43 | 218.1          |
|    44 | 225.7          |
|    45 | 229.1          |
|    46 | 233.6          |
|    47 | 241.8          |
|    48 | 250.3          |
|    49 | 254.1          |
</td>
</table>

#### Layout

The memory locations listed below are relative to the end of the previous block, being either the header or a contact. The fields depend on the type of channel.

For DMR channels, this section is laid out in the following manner:

```
      00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F
0000 |<m|<t|<-power--->|<rx_frequen|<tx_frequen|<name
0010  -----------------------------------------------
0020  ---------------------------------------->|<descr
0030  -----------------------------------------------
0040  ---------------------------------------->|<ch_lo
0050  cation---------------->|<c|<d|<cont|
```

For FM channels, this section is laid out in the following manner:

```
      00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F
0000 |<m|<t|<-power--->|<rx_frequen|<tx_frequen|<name
0010  -----------------------------------------------
0020  ---------------------------------------->|<descr
0030  -----------------------------------------------
0040  ---------------------------------------->|<ch_loca
0050  tion------------------>|<r|<t
```

For M17 channels, this section is laid out in the following manner:

```
      00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F
0000 |<m|<t|<-power--->|<rx_frequen|<tx_frequen|<name
0010  -----------------------------------------------
0020  ---------------------------------------->|<descr
0030  -----------------------------------------------
0040  ---------------------------------------->|<ch_lo
0050  cation---------------->|<c|<m|<g|<cont|
```

### Bank Data Offsets

The Bank Data Offsets structure is variable in length depending on the number of banks. It is used primarily to find the seek position for banks, given that each bank definition is also variable in length. Without it, seeking to a bank would not be possible.

```
| Field   | Data Type   | Description                                                                      |
| ------- | ----------- | -------------------------------------------------------------------------------- |
| offsets | uint32_t[n] | The relative offset where the bank data should begin for each corresponding bank |

```

#### Layout

This structure is repeated for each bank present. One structure exists for each bank that exists, and its sort order indicates the banks index (i.e. the first offset appearing is the initial bank).

```
      00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F
0000 |<offset--->|...
```

### Bank

The Bank structure is variable in length depending on the number of channels. Some implementations may find it easier to split this section into a fixed header portion and a variable channel portion. The Bank structure is repeated for each bank that is present.

| Field    | Data Type   | Description                                              |
| -------- | ----------- | -------------------------------------------------------- |
| name     | char[32]    | The name of the bank                                     |
| ch_count | uint16_t    | Count of all of the channels in the bank                 |
| channels | uint16_t[n] | The indexes of the channels that are present in the bank |

#### Layout

This structure is repeated for each bank present. Its start is immediately after the preceding section, and the memory locations referenced below are relative to that. The final field _channels_ is optional and variable in size depending on the _ch_count_.

```
      00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F
0000 |<name------------------------------------------
0010  ---------------------------------------------->
0020 |<ch_c|?cha?|...
```

## Appendix

### References

[Refer to this header file](https://github.com/OpenRTX/OpenRTX/blob/master/openrtx/include/core/cps.h) for the first implementation of this format.

### Feedback

Persons interested in leaving feedback on the format standard are invited to submit [GitHub Issues](https://github.com/OpenRTX/OpenRTX/issues).

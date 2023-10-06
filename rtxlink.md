# RTXLink - OpenRTX Communication Protocol

This page describes `rtxlink`, the communication protocol used by the OpenRTX firmware to accomplish the following functions:

- Retrieval of radio info
- Reboot of the device
- Backup and restore of the nonvolatile memory
- File download and upload
- Change of some RTX parameters

## Physical Layer

The physical layer of the rtxlink protocol is a serial communication, which may either be based on a physical link (UART) or a Virtual COM over USB.
In case a physical link is used, its configuration is 8 data bits, no parity and either EIA hardware flow control, or CLOCAL mode (3-wire null-modem).
The default baud rate is 115200 bit per second but the change to a different datarate can be requested by the client device. The support for datarates
other than the default one is not mandatory, although an rtxlink host must support at least 115200 bit per second. The protocol tolerates buffering and
fragmentation of the transmitted data.

## Data Link Layer

The data link layer of the rtxlink protocol consists of variable-length frames encoded using the [Serial Line Internet Protocol (SLIP)](https://en.wikipedia.org/wiki/Serial_Line_Internet_Protocol).
Each SLIP frame must begin with an END character: incoming frames not beginning with and END character are accepted anyway, even if not strictly conformant to the specification. SLIP can extend
the frame length up to the double of the original length, this needs to be taken into account when sizing buffers during the protocol implementation.

The frames format is the following:

|  0  |    1    |  ... |  N-1 |  N  |
|:---:|:-------:|:----:|:----:|:---:|
| END | ProtoID | Data | CRC8 | END |

After the star of frame marker, the first byte of each frame is a protocol identifier describing the frame content. The last byte of the frame, following the content and preceding the frame end
marker contains the CRC-8 of the protocol identifier and data fields. The CRC is computed using the polynomial 0xA6 and ensures a minimum hamming distance of two for data blocks composed by more
than 2048 bytes.

The recognized protocol IDs are the following:

|  ID  |    Frame content     |
|:----:|:--------------------:|
| 0x00 | stdio redirection    |
| 0x01 | CAT command/response |
| 0x02 | FMP command/response |
| 0x03 | XMODEM command/frame |

## Application Layer

The application layer of rtxlink consists of two command different protocols: the Computer Aided Transceiver (CAT) protocol and the File Management Protocol (FMP).

### Computer Aided Transceiver (CAT)

The CAT protocol is based on a request/response command structure. Commands are byte-aligned and encoded in key-value pairs to achieve both simplicity of the protocol and ease of parsing. All the
commands and their arguments are encoded in little-endian format. The first byte of each CAT command or response packet is the packet opcode.

#### Get Requests

A get request is used by the rtxlink client to request the value of given entity. The command frame has a fixed length of three bytes, with the opcode set to 0x47 (ASCII character 'D'). Following
the opcode there are two bytes containing the resource ID. In response to a get request the host sends either a data response frame or, in case of failures, an ack frame reporting an error code.

|  0  |  1 |  2 |
|:---:|:--:|:--:|
| CMD | ID | ID |

#### Set Requests

A set request is used by the rtxlink client to change the value of an entity. The command frame has a variable length with a minimum size of four bytes and the opcode is 0x53 (ASCII character 'S').
Following the opcode there are two bytes containing the resource ID and N bytes of data. In response to a set request the host sends an ack frame with a status code.

|  0  | 1  |  2 | 3 | ... | N |
|:---:|:--:|:--:|:-:|:---:|:-:|
| CMD | ID | ID | V | ... | V |

#### Peek Requests

A peek request allow the client to read the content of the host internal memory at a given address. The command frame has a fixed length of six bytes, with the opcode set to 0x50 (ASCII character 'P').
After the command opcode, there is one byte of data length and four bytes of address. The host replies to a peek request with a data response packet or, in case of failures, an ack frame reporting an
error code.

|  0  |  1  |  2   |   3  |   4  |   5  |
|:---:|:---:|:----:|:----:|:----:|:----:|
| CMD | LEN | ADDR | ADDR | ADDR | ADDR |

#### Data response

A data response frame has the opcode set to 0x44 (ASCII character 'D') followed by N bytes of data. Data response frames are sent in reply to get or peek requests.

|   0  | 1 | ... | N |
|:----:|:-:|:---:|:-:|
| 0x44 | V | ... | V |

#### ACK response

An ack response frame has a fixed length of two bytes: the first byte (opcode) is always set to 0x41 (ASCII character 'A'), the second one carries a status code. A status code zero means that the previous
command has been successfully executed. Othewise, in case an error occurred, the status code field contains a POSIX error code describing the fault or 0xFF to signal a generic, unspecified error. The host
must at least use the values 0x00 and 0xFF for the status field.

|   0  |    1   |
|:----:|:------:|
| 0x41 | status |

#### Commands Summary

|   Role   | Command |   OpCode | Parameters     |  Length  | Description |
|:---------|:--------|:---------|:---------------|:-------- |:------------|
| Request  | Get     | 0x47 'G' | ID             | 3        | Gets the N Byte word corresponding to the specified ID. |
| Response | Data    | 0x44 'D' | Value          | Variable | The N Byte word which was requested. |
| Request  | Set     | 0x53 'S' | ID, Value      | Variable | Sets a N Byte word to the specified ID. |
| Response | Ack     | 0x41 'A' | Status         | 1        | Status of the previous Set command, success or error. |
| Request  | Peek    | 0x50 'P' | Address        | 5        | Gets the 32bit word corresponding to the specified 32-bit address. |
| Response | Data    | 0x44 'D' | Value          | 5        | The 32bit word which was requested. |

#### Get/set resource IDs

The tables below summarizes the CAT resource identifiers with their respective size, semantic and permissions.

| Permissions    | Symbol |
|:---------------|:-------|
| Read Only      | R      |
| Read and Write | RW     |
| Write Only     | W      |

| Symbol        | ID          | Type (Length) | Permissions | Description  |
|:--------------|:------------|:--------------|:------------|--------------|
| info          | 0x494E 'IN' | String (16)   | R           | Return radio identifier |
| rx_frequency  | 0x5246 'RF' | i32 (4)       | RW          | Get or set the current VFO receive frequency |
| tx_frequency  | 0x5446 'TF' | i32 (4)       | RW          | Get or set the current VFO transmit frequency |
| baud_rate     | 0x4252 'BR' | i32 (4)       | W           | Set the baud rate of the rtxlink interface |
| power_cycle   | 0x5043 'PC' | None          | W           | Reboot radio              |
| file_transfer | 0x4654 'FT' | None          | W           | Enable File Transfer Mode |

<!-- | free_space    | 0x4653 'FS' | i32 (4)       | R           | Return available space in Bytes | -->


### Appendix A - SLIP framing

The following are the standard value Bytes employed by the SLIP protocol.

| Hex value | Dec Value |	Oct Value |	Abbreviation | Description             |
|:----------|:----------|:------------|:-------------|:------------------------|
| 0xC0      | 192       | 	300       |	END          | Frame End               |
| 0xDB      | 219       | 	333       |	ESC          | Frame Escape            |
| 0xDC      | 220       | 	334       |	ESC_END      | Transposed Frame End    |
| 0xDD      | 221       | 	335       |	ESC_ESC      | Transposed Frame Escape |

SLIP modifies a standard TCP/IP datagram by:

- Appending a special "END" Byte to it, to distinguish datagram boundaries in the Byte stream,
- If the END Byte occurs in the data to be sent, the two Byte sequence ESC, ESC_END is sent instead,
- If the ESC Byte occurs in the data, the two Byte sequence ESC, ESC_ESC is sent.
- Variants of the protocol may begin, as well as end, packets with END.

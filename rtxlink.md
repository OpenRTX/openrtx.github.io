# RTXLink - OpenRTX Communication Protocol

This page describes `rtxlink`: a communication protocol implemented by
OpenRTX and used to accomplish the following functions:

- Get radio info
- Reboot radio
- Full flash backup/restore
- Download file
- Upload file
- Set RTX params

Additionally, the protocol has the following characteristics:

- SLIP is used for framing and encloses the whole protocol
- The protocol is Byte-oriented
- protocol is composed of getters and setters
- variables to get and set are encoded into opcodes
- plain ymodem is used for file transfer

The protocol stack is as follows:

| Application | CAT, FMP, XMODEM  | 
|:-----------:|:-----------------:|
| Framing     | SLIP              |
| Physical    | UART, VCOM        |

## Physical Layer - UART, VCOM

This protocol is designed to be executed on a serial link, which depending on the platform support might be a physical (UART) link or a Virtual COM over USB. Buffering is tolerated by the protocol. The baud rate is 115200.

## Data Link Layer

The data link layer of rtxlink is composed of frames with a variable length encoded using the SLIP protocol. Each SLIP frame must begin with an END character; incoming frames not beginning with and END character are accepted anyways, even if not strictly conformant to the specification.

### SLIP framing

The framing is performed using the [Serial Line Internet Protocol (SLIP)](https://en.wikipedia.org/wiki/Serial_Line_Internet_Protocol).
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

SLIP requires a serial port configuration of 8 data bits, no parity, and either EIA hardware flow control, or CLOCAL mode (3-wire null-modem) UART operation settings. 

SLIP can extend the frame length up to the double of the original length, this needs to be taken into account when sizing buffers during the protocol implementation.

### Frame format

|  0  |    1    |  ... |  N-1 |  N  |
|:---:|:-------:|:----:|:----:|:---:|
| END | ProtoID | Data | CRC8 | END |

Following the leading END marker, the first byte of each frame is a protocol identifier describing the frame content, while the last byte of the frame contains the CRC-8 of the protocol ID and data fields. The polynomial used for the CRC is 0xA6, ensuring a minimum hamming distance of 2 for data blocks composed by more than 2048 bytes. 

The recognized protocol IDs are the following:

|  ID  |    Frame content     |
|:----:|:--------------------:|
| 0x00 | stdio redirection    |
| 0x01 | CAT command/response |
| 0x02 | FMP command/response |
| 0x03 | XMODEM command/frame |

## Application Layer

The application layer is composed of two different protocols: a Computer Aided Transceiver (CAT) protocol to control the operation of the radio and a File Management Protocol (FMP) to request file transfers and memory backup and restore. The actual transfer of files and backup images is performed using the XMODEM protocol.

### Computer Aided Transceiver (CAT)

Byte encodings are little-endian.

CAT commands are Byte-aligned and encoded in key-value pairs to achieve both simplicity of the protocol and ease of parsing.

The CAT command identifies resources through 2 Byte IDs, and allows the host (main agent of the transmission) to get values associated with each ID or set a new value to a corresponding ID.

The first byte of a CAT command and response packet is the packet OpCode.

#### Get Requests

A get request asks the secondary entity on the RTXLink bus for a value associated with a specific ID. The request is composed by 3 bytes: the first byte is the command OpCode, followed by two symbol identifier bytes.

|  0  |  1 |  2 |
|:---:|:--:|:--:|
| CMD | ID | ID |

In response to a get request the radio sends either a data response packet or, in case of failures, an ack packet with the ERROR flag set 1.

#### Set Requests

Set requests allow to change a specific value associated with a certain ID.
Set requests are componsed by one command byte, 2 Bytes of symbol ID and N bytes of data.

|  0  | 1  |  2 | 3 | ... | N |
|:---:|:--:|:--:|:-:|:---:|:-:|
| CMD | ID | ID | V | ... | V |

The radio replies to a set request with an ack response packet with either the OK or ERROR flag set.

#### Peek Requests

Peek requests allow to read the content of a the radio internal memory at a given address. Peek requests are componsed by one command byte, one byte of data length and four bytes of address.

|  0  |  1  |  2   |   3  |   4  |   5  |
|:---:|:---:|:----:|:----:|:----:|:----:|
| CMD | LEN | ADDR | ADDR | ADDR | ADDR |

The radio replies to a peek request with a data response packet or, in case of errors, with an ack response packet with the ERROR flag set.

#### Data response

A data response packet starts with a response type byte set to 0x44 (ASCII characted 'D') followed by N bytes of data.

|   0  | 1 | ... | N |
|:----:|:-:|:---:|:-:|
| 0x44 | V | ... | V |

#### ACK response

An ack response packet is constituted of two bytes: the first byte (response type) is always set to 0x41 (ASCII character 'A') while the second one carries a status code. A status code equal to zero means that the previous command has been successfully executed. In case of an error the status code field contains a POSIX error code describing the fault or 0xFF to signal a generic, unspecified error.
The absolute minimum requirement is to have the status field set either to 0x00 or 0xFF.

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

#### Identifiers

OpenRTX CAT uses symbolic values to identify controllable features of the radio, the tables below summarizes the features and the respective sizes, semantics and permissions.

| Permissions    | Symbol |
|:---------------|:-------|
| Read Only      | R      |
| Read and Write | RW  |
| Write Only     | W      | 

| Symbol        | ID          | Type (Length) | Permissions | Description  |
|:--------------|:------------|:------------|:------------|--------------|
| info          | 0x494E 'IN' | String (10) | R           |  Return radio identifier |
| free_space    | 0x4653 'FS' | i32 (4)     | R           | Return available space in Bytes | 
| rx_frequency     | 0x5246 'RF' | i32 (4)     | RW          | Get or set the current VFO receive frequency | 
| tx_frequency     | 0x5446 'TF' | i32 (4)     | RW          | Get or set the current VFO transmit frequency | 
| power_cycle   | 0x5043 'PC' | None        | W           | Reboot radio              |
| file_transfer | 0x4654 'FT' | None        | W           | Enable File Transfer Mode |

### XMODEM

File transfers are performed throught the XMODEM protocol with 16-bit CRC and 1KB blocks.

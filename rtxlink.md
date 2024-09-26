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

|  0  |    1    |  ... |  N-2 |  N-1 |  N  |
|:---:|:-------:|:----:|:----:|:----:|:---:|
| END | ProtoID | Data | CRCL | CRCH | END |

After the start of frame marker, the first byte of each frame is a protocol identifier describing the frame content. After the payload data and preceding the frame end marker, the frame contains the
CRC-16 of the protocol identifier and data fields: the CRC is computed using the CCITT polynomial 0x1021 (CCITT CRC-16 polynomial) and transmittend in little-endian format.

The recognized protocol IDs are the following:

|  ID  |     Frame content      |
|:----:|:----------------------:|
| 0x00 | stdio redirection      |
| 0x01 | CAT command/response   |
| 0x02 | FMP command/response   |
| 0x03 | data transfer protocol |

## Application Layer

The application layer of rtxlink consists of two different command protocols: the Computer Aided Transceiver (CAT) protocol and the File Management Protocol (FMP).

### Computer Aided Transceiver (CAT)

The CAT protocol is based on a request/response command structure. Commands are byte-aligned and encoded in key-value pairs to achieve both simplicity of the protocol and ease of parsing. All the
commands and their arguments are encoded in little-endian format. The first byte of each CAT command or response packet is the packet opcode.

#### Get Requests

A get request is used by the rtxlink client to request the value of given entity. The command frame has a fixed length of three bytes, with the opcode set to 0x47 (ASCII character 'G'). Following
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
The command opcode byte is followed by one byte specifying the number of bytes to read and by the start address for the read operation. The length of the address depends on the host architecture and
it can be either 2, 4 or 8 bytes long; any other length is considered invalid. The host replies to a peek request with a data response packet or, in case of failures, an ack frame reporting an error code.

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
command has been successfully executed. Otherwise, in case an error occurred, the status code field contains a POSIX error code describing the fault or 0xFF to signal a generic, unspecified error. The host
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
| op_mode       | 0x4F4D 'OM' | i8 (1)        | RW          | Get or set the current operating mode |
| m17_callsign  | 0x4D43 'MC' | String (10)   | RW          | Get or set the M17 callsign |
| m17_dest      | 0x4D44 'MD' | String (10)   | RW          | Get or set the M17 destination address |
| baud_rate     | 0x4252 'BR' | i32 (4)       | W           | Set the baud rate of the rtxlink interface |
| power_cycle   | 0x5043 'PC' | None          | W           | Reboot radio              |
| file_transfer | 0x4654 'FT' | None          | W           | Enable File Transfer Mode |

<!-- | free_space    | 0x4653 'FS' | i32 (4)       | R           | Return available space in Bytes | -->

#### Operating Modes

| Value | Operating Mode        |
|:------|:----------------------|
| 0x00  | No mode set           |
| 0x01  | FM 25.0 kHz bandwidth |
| 0x02  | FM 20.0 kHz bandwidth |
| 0x03  | FM 12.5 kHz bandwidth |
| 0x04  | DMR                   |
| 0x05  | M17                   |

### File Management Protocol (FMP)

The file management protocol is used to read/write files from the radio filesystem and to dump or restore the content of one of its nonvolatile memories. To guarantee data consistency, before doing
any write operation, the device has to be put in file transfer mode by issuing the corresponding CAT command. The only possible way to resume back to the normal operation after the device has been
put in file transfer mode is through a power cycle; this ensures that all the changes made either in the files or to the nonvolatile memories are properly loaded.

| Command | OpCode | Request parameters   | Response arguments  | Description                                                       |
|:--------|:-------|:---------------------|:--------------------|:------------------------------------------------------------------|
| Meminfo | 0x01   | -                    | Array of Meminfo    | Return the descriptor of the available non volatile memories.     |
| Dump    | 0x02   | MemIndex             | Size                | Dump the full content of a nonvolatile memory.                    |
| Flash   | 0x03   | MemIndex, Size       | -                   | Write an image on a nonvolatile memory.                           |
| Read    | 0x04   | Path                 | Size                | Transfer the content of the file at the specified path.           |
| Write   | 0x05   | Path, Size           | -                   | Check if there is enough space and begin a write to the specified file, the size is mandatory. |
| List    | 0x06   | Path                 | Array of filenames  | List the files in a path as strings.                              |
| Move    | 0x07   | SourcePath, DestPath | -                   | Rename or move a file.                                            |
| Copy    | 0x08   | SourcePath, DestPath | -                   | Copy a file.                                                      |
| MkDir   | 0x09   | Path                 | -                   | Create a new directory.                                           |
| Remove  | 0x0a   | Path                 | -                   | Delete a file or directory.                                       |
| Reset   | 0xff   | -                    | -                   | Reset the FMP state machine, aborting the last pending operation. |

Sizes are expressed in 32 bit, little endian format. The maximum length for paths is 128 characters. If there is no filesystem, only the dump and flash commands are available.

#### Request Format

A request frame consists of at least two bytes: the first byte specifies the command, the second one the number of parameters. After the first two bytes, the frame contains N bytes specifying the
length of the parameters (one byte per parameter) followed by the parameters. If a command does not require parameters, the number of parameters is set to zero and the request frame consists
of only two bytes.

|  0  |    1     |       2        |  3  |       4        |    5    |  6  |    7    |
|:---:|:--------:|:--------------:|:---:|:--------------:|:-------:|:---:|:-------:|
| CMD | N Params | Param 1 length | ... | Param N length | Param 1 | ... | Param N |

#### Response Format

A response frame consists of at least three bytes: the first byte is the command byte, followed by a status byte and then by one specifying the number of arguments. The command byte is equal to the
one sent in the request, the status byte is set to zero to indicate a successful operation or to a POSIX error code in case of faults. A value of 0xFF indicates an unspecified error. Following the
first three bytes, the frame contains N bytes specifying the length of the arguments (one byte per argument) followed by the arguments. If a response does not contain arguments, the number of arguments
is set to zero and the frame is three bytes long.

|  0  |    1    |     2   |       3        |  4  |       5        |    6    |  7  |    8    |
|:---:|:-------:|:-------:|:--------------:|:---:|:--------------:|:-------:|:---:|:-------:|
| CMD | Status  | NParams | Param 1 length | ... | Param N length | Param 1 | ... | Param N |

#### Memory descriptor

The `meminfo` command returns a list of memory descriptors, providing the information of each nonvolatile memory on the device. The memory index for the dump and flash commands is given by the order
in which the corresponding descriptor appears in the argument list, with the first element having index 0. The memory descriptor is a binary data structure with the following format:

```C
typedef struct meminfo_t
{
    uint32_t size;  // Size of the memory in Bytes
    uint8_t  flags; // Memory type and access flags
    char name[27];  // Name of the memory
}
meminfo_t;          // sizeof(meminfo_t) == 32;
```

Or equivalently in Rust:

```Rust
#[derive(Copy, Clone, Debug)]
#[repr(C, packed)]
pub struct MemInfo {
    size:  u32,     // Size of the memory in Bytes
    flags: u8,      // Memory type and access flags
    name: [u8; 27], // Name of the memory
}
```

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

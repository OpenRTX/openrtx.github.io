# MD-UV380

## Device models
- Tytera MD-UV380
- Tytera MD-UV390
- Retevis RT3s

__Model variants__
- With GPS: with JumpStar JS-H210 module on display/keyboard PCB
- Without GPS: no module footprint present on display/keyboard PCB

## Specifications
* Display: 160x128 color TFT
* Frequency range
    * VHF: 136.000-174.000
    * UHF: 400.000-480.000

## Hardware components
### Clock tree configuration
To be found

## Memory mapping

### SPI Flash layout
Start  | End    | Section
  ---  | ---    | ---
0x2001 | 0x2005 | CPS timestamp
0x2040 | 0x2049 | Splash text line 1
0x2050 | 0x2059 | Splash text line 2
0x2180 | 0x59b9 | SMS quick text
0x416d0 | 0x416e8 | SMS Inbox header?
0x41798 | 0x41d70 | SMS Inbox
0x45100 | 0x45109 | SMS Outbox header?
0x451c8 | 0x45657 | SMS Outbox
0x48000 | 0x48039 | ???
0x48b00 | 0x48c83 | ???
0x48f00 | 0x48f43 | ???
0x48fc8 | 0x49018 | ???
0x60000 | 0xbc0d7 | ???
0x100000 | 0x103606 | ???
0x110000 | 0x13edf9 | Channels
0x140000 | 0x197e39 | Contacts
0xfe5000 | 0xfebff9 | ???
0xfec000 | 0xffffff | DMR ID database

#### CPS timestamp
0x00|0x01|0x2|0x3|0x4|0x5|0x6|0x7|0x8|0x9|0xA|0xB|0xC|0xD|0xE|0xF| 
---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
FF|year(first two digits)|year(last two digits)|month|date|hour|minute|seconds|00|01|01|02|FF|FF|FF|FF

#### SMS messages
Messages in SMS inbox are saved in reverse order (lowest address contains last received SMS)
Because of this the SMS Inbox start address could be lower (Inbox was not full on flash dump).

Every SMS is stored as ASCII text in 292 bytes of the flash.
There seems to be a 4-byte header for each SMS

Inbox/Outbox message header: `0x0b 0xef 0x21 0x02`
GPS coordinate message header: `0xc2 0xd2 0x21 0x02`
??? message header: `0x3e 0xf1 0x21 0xc2`

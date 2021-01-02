# MD-UV380
 
## Specifications
### Frequency range
* VHF: 136.000-174.000
* UHF: 400.000-480.000

## Hardware components
### Clock tree configuration
To be found

## Memory mapping

### External flash mapping
Start  | End    | Content
  ---  | ---    | ---
0x2001 | 0x2005 | CPS timestamp
0x2040 | 0x2049 | Splash text line 1
0x2050 | 0x2059 | Splash text line 2
0x2180 | 0x2189 | "Hello"


#### CPS timestamp
0x00|0x01|0x2|0x3|0x4|0x5|0x6|0x7|0x8|0x9|0xA|0xB|0xC|0xD|0xE|0xF| 
---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
FF|year(first two digits)|year(last two digits)|month|date|hour|minute|seconds|00|01|01|02|FF|FF|FF|FF

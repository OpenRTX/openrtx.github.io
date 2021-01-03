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

### GPIO mapping

##### Port A
GPIO  | Mode   | Function         | Notes
---   | ---    |   ---            |  ---
 PA0  |        |                  |
 PA1  | analog | battery voltage  | ADC1_IN1, 1:3 voltage divider
 PA2  |        |                  |
 PA3  |        |                  |
 PA4  |        |                  |
 PA5  |        |                  |
 PA6  | output | keyboard row 1   |
 PA7  |        |                  |
 PA8  | output | AT1846S SCL      |
 PA9  |        |                  |
 PA10 |        |                  |
 PA11 |        |                  |
 PA12 |        |                  |
 PA13 | output | mic power switch | not confirmed
 PA14 |        |                  |
 PA15 |        |                  |

##### Port B
GPIO  | Mode   | Function             | Notes
---   | ---    |   ---                |  ---
 PB0  |        |                      |
 PB1  |        |                      |
 PB2  |        |                      |
 PB3  | AF     | SPI1_SCK             | extarnal flash
 PB4  | AF     | SPI1_MISO            | extarnal flash
 PB5  | AF     | SPI1_MOSI            | extarnal flash
 PB6  |        |                      |
 PB7  |        |                      |
 PB8  |        |                      |
 PB9  |        |                      |
 PB10 |        |                      |
 PB11 | input  | channel selector     |
 PB12 |        |                      |
 PB13 |        |                      |
 PB14 |        |                      |
 PB15 |        |                      |

##### Port C
GPIO  | Mode          | Function          | Notes
---   | ---           |   ---             |  ---
 PC0  |               |                   |
 PC1  |               |                   |
 PC2  |               |                   |
 PC3  |               |                   |
 PC4  | output        | RF_APC_SW         | not confirmed
 PC5  | output        | 5TC - TX power en | not confirmed
 PC6  |               |                   |
 PC7  |               |                   |
 PC8  |               |                   |
 PC9  | bidirectional | AT1846S SDA       |
 PC10 |               |                   |
 PC11 |               |                   |
 PC12 |               |                   |
 PC13 |               |                   |
 PC14 |               |                   |
 PC15 |               |                   |

##### Port D
GPIO  | Mode      | Function               | Notes
---   | ---       |   ---                  |  ---
 PD0  | input/AF  | LCD_D2 - keyboard      | FSMC
 PD1  | input/AF  | LCD_D3 - keyboard      | FSMC
 PD2  | output    | keyboard row 2         |
 PD3  | output    | keyboard row 3         |
 PD4  | output    | LCD_RD                 |
 PD5  | output    | LCD_WR                 |
 PD6  | output    | LCD_CS                 |
 PD7  | output    | external flash CS      |
 PD8  | output    | LCD backlight          |
 PD9  |           |                        |
 PD10 |           |                        |
 PD11 |           |                        |
 PD12 | alternate | LCD_RS                 | FSMC
 PD13 | output    | LCD_RST                |
 PD14 | input/AF  | LCD_D0 - keyboard      | FSMC
 PD15 | input/AF  | LCD_D1 - keyboard      | FSMC

##### Port E
GPIO  | Mode     | Function             | Notes
---   | ---      |   ---                |  ---
 PE0  | output   | green LED            |
 PE1  | output   | red LED              |
 PE2  | output   | HR_C6000 chip select |
 PE3  | output   | HR_C6000 SCK         |
 PE4  | output   | HR_C6000 SDO         | MCU  -> chip
 PE5  | output   | HR_C6000 SDI         | chip -> MCU
 PE6  |          |                      |
 PE7  | input/AF | LCD_D4 - keyboard    | FSMC
 PE8  | input/AF | LCD_D5 - keyboard    | FSMC
 PE9  | input/AF | LCD_D6 - keyboard    | FSMC
 PE10 | input/AF | LCD_D7 - keyboard    | FSMC
 PE11 | input    | PTT                  | active low
 PE12 |          |                      |
 PE13 |          |                      |
 PE14 | input    | channel selector     |
 PE15 |          |                      |

### HR_C6000
The SPI control interface is connected to the following GPIOs:
* PE2 - chip select
* PE3 - SCK
* PE4 - SDO
* PE5 - SDI

These SPI connections are shared with some other chip, since the firwmare locks a semaphore/mutex before initiating any kind of data transfer through the SPI interface. The semaphore variable is at address 0x2001EFB8.

Functions identified:
* 0x0807A3C0 - software SPI implementation for the control interface
* 0x0807A43E - function for sending a data block through the control interface
* 0x0807A47E - function to acquire exclusive ownership of the SPI GPIOs and select the HR_C6000
* 0x0807A496 - function releasing exclusive ownership of the SPI GPIOs and deselecting the HR_C6000
* 0x080545F4 - function for writing to a control RAM register

### AT184S
Most of the baseband functionalities are managed by the "radio on a chip" AT1846S IC. The STM32F405 MCU sends data and commands through an I2C interface with SDA on PC9 and SCL on PA8. The interface is managed in software through bit-banging routines and the IC slave address is 0x5C (0x5D in read mode).

The functions to write and read data from the IC registers are protected by a semaphore located at 0x2001EFBC.

Functions identified:
* 0x08070446 - generation of I2C start condition
* 0x0807064A - routine to write one byte on the I2C bus
* 0x080704C6 - generation of I2C stop condition
* 0x080706CA - start of an I2C transfer, generation of start condition and sending of slave address
* 0x080706DC - routine to read one byte from the I2C bus
* 0x08070756 - function for writing an AT1846S register
* 0x0807079c - function for reading an AT1846S register

## Memory mapping

### Flash - other functions not listed in the above blocks
* 0x0806B96A: function activating AT1846S RX

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

## System tasks

##### "RF PLL" task
Body address | Stack address | ID  | Priority
     ---     |      ---      | --- |   ---
 0x0806B05C  |  0x200186CC   |  6  |   6

This task controls is in charge of managing the AT1846S baseband chip, setting the TX and RX frequencies and other functionalities to be discovered. Upon startup, the task calls the function at 0x0806B05C to perform the initial configuration of the AT1846S registers. Then it enters in an infinite loop which waits for new data on the mailbox at address 0x2001EFAC; this data is the processed in the function at 0x0804B5D4.


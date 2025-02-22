# Radtel RT-4D

## Specifications
* MCU: AT32F423CCT7
* DMR Chip: HF6853
* FM Broadcast Receiver Chip: BK1080
* Radio Chip: BK4819
* SPI Flash: PY25Q128HA 16MiB
* Display: 128x64 monochrome TFT
* 0x2800 Byte Bootloader

## Pinout

| Pin Number | Pin Name | Usage           |
|------------|----------|-----------------|
| 1          | V_DD     |                 |
| 2          | PC13     | SCK1080         |
| 3          | PC14     | SDA1080         |
| 4          | PC15     | FM POWER-EN     |
| 5          | PF0      | KEYOUT3         |
| 6          | PF1      | KEYIN1          |
| 7          | NRST     |                 |
| 8          | V_SSA    |                 |
| 9          | V_DDA    |                 |
| 10         | PA0      | KEYOUT1         |
| 11         | PA1      | KEYOUT2         |
| 12         | PA2      | AF MUTE4898     |
| 13         | PA3      | BACKLED-EN      |
| 14         | PA4      | AUDIO           |
| 15         | PA5      | TX DTMF         |
| 16         | PA6      | DIG-RXD         |
| 17         | PA7      | DIG-TXD         |
| 18         | PB0      | KEYOUT4         |
| 19         | PB1      | DIG POWER-EN    |
| 20         | PB2      | LOW BATTERY DET |
| 21         | PB10     | DIG/ANA SW      |
| 22         | PB11     | KEYIN4          |
| 23         | PF8      | DIG/ANA AF-SW   |
| 24         | V_DD     |                 |
| 25         | PB12     | W25Q-CS         |
| 26         | PB13     | W25Q-SCK        |
| 27         | PB14     | W25Q-DO         |
| 28         | PB15     | W25Q-DI         |
| 29         | PA8      | KEYIN2          |
| 30         | PA9      | KEYIN3          |
| 31         | PA10     | UV APCOUT-SW    |
| 32         | PA11     | TXD             |
| 33         | PA12     | RXD/PTT         |
| 34         | PA13     | TX-LED/PDA      |
| 35         | PF6      | LCD-RST         |
| 36         | V_DD     |                 |
| 37         | PA14     | RX-LED/PCK      |
| 38         | PA15     | LCD-CS          |
| 39         | PB3      | LCD-SCK         |
| 40         | PB4      | LCD-CD          |
| 41         | PB5      | LCD-SDA         |
| 42         | PB6      | DCS-DET         |
| 43         | PB7      | SDA4819         |
| 44         | BOOT0    |                 |
| 45         | PB8      | SCN4819         |
| 46         | PB9      | SCK4819         |
| 47         | V_SS     |                 |
| 48         | V_DD     |                 |

## Pictures

### PCB Front

![RT-4D PCB Front](../_media/rt4d_front.jpg)

### PCB Back

![RT-4D PCB Back](../_media/rt4d_back.jpg)

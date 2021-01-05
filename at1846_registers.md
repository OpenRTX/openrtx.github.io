# AT1846S register table

##### 0x04
 bit | function                          |            notes
 --- |   ---                             |   --- 
 0   |  clock mode (frequency range)     | 0 = 24\~28MHz <br> 1 = 12\~14MHz

##### 0x08
 bit | function                          |            notes
 --- |   ---                             |   --- 
  14 |  LDO bypass                       | 1 = bypass all the AT1846S internal LDOs. Must be 0 in VHF band.

##### 0x09
 bit | function                          |            notes
 --- |   ---                             |   --- 
 9:7 |  LDO output voltage               | 100=2.20V<br> 101=2.40V<br> 110=2.80V<br> 111=3.30V

##### 0x0A
 bit | function                          |            notes
 --- |   ---                             |
 5:0 |  PA bias voltage                  | 000000: 1.01V<br>000001:1.05V<br>000010:1.09V<br>000100: 1.18V<br>001000: 1.34V<br>010000: 1.68V<br>100000: 2.45V<br>1111111:3.13V


##### 0x0F
 bit | function                          |            notes
 --- |   ---                             |   --- 
1:0  |  band select                      | 00 = 400\~520MHz <br> 10 = 200\~260MHz <br> 11 = 134\~174MHz

##### 0x1F
 bit  | function                          |            notes
 ---  |   ---                             |   --- 
14:15 |  GPIO0 mode                       | 00 = hi-z<br> 01 = vox<br> 10 = low<br> 11 = high
13:12 |  GPIO0 mode                       | 00 = hi-z<br> 01 = squelch or squelch & CTC/DCS when sq_out_sel = 1<br> 10 = low<br> 11 = high
11:10 |  GPIO0 mode                       | 00 = hi-z<br> 01 = txon_rf<br> 10 = low<br> 11 = high
 9:8  |  GPIO0 mode                       | 00 = hi-z<br> 01 = rxon_rf<br> 10 = low<br> 11 = high
 7:6  |  GPIO0 mode                       | 00 = hi-z<br> 01 = sdo<br> 10 = low<br> 11 = high
 5:4  |  GPIO0 mode                       | 00 = hi-z<br> 01 = int<br> 10 = low<br> 11 = high
 3:2  |  GPIO0 mode                       | 00 = hi-z<br> 01 = code_out/code_in<br> 10 = low<br> 11 = high
 1:0  |  GPIO0 mode                       | 00 = hi-z<br> 01 = css_out/css_in/css_cmp<br> 10 = low<br> 11 = high


##### 0x29
 bit | function                          |            notes
 --- |   ---                             |   --- 
13:0 |  frequency upper part [29:16]     |

##### 0x2A
 bit | function                          |            notes
 --- |   ---                             |   --- 
15:0 |  frequency lower part [15:0]      |

##### 0x2B
 bit | function                          |            notes
 --- |   ---                             |   --- 
15:0 |  reference clock frequency in kHz | 12\~14MHz: crystal freq1000 <br> 24\~ 28MHz: (crystal freq/2)1000

##### 0x2C
 bit | function                          |            notes
 --- |   ---                             |
15:0 |  ADC clock frequency in kHz       | 12\~14MHz: crystal freq1000 <br> 24\~ 28MHz: (crystal freq/2)1000

##### 0x2D
 bit | function                             |            notes
 --- |   ---                                |   --- 
  9  | css_cmp interrupt enable             |
  8  | rxon_rf interrupt enable             |
  7  | txon_rf interrupt enable             |
  6  | dtmf_idle interrupt enable           |
  5  | ctcss phase shift detect int. enable |
  4  | idle state time out int. enable      |
  3  | rxon_rf timerout interrupt enable    |
  2  | sq interrupt enable                  |
  1  | txon_rf time out interrupt enable    |
  0  | vox interrupt enable                 |

##### 0x30
 bit  | function                          |            notes
 ---  |   ---                             |   --- 
13:12 |  channel mode                     | 11 = 25khz channel mode <br> 00 = 12.5khz channel mode <br> 10,01 = reserved
  11  |  enable tail noise elimination    |
 9:8  |  st mode                          | 11 = reserved<br> 10 = txon_rf & rxon_rf auto<br> 01 = rxon_rf auto, txon_rf manual<br> 00 = txon_rf & rxon_rf manual
  6   |  tx enable                        |
  5   |  rx enable                        |
  4   |  on-chip VOX enable               |
  3   |  on-chip squelch enable           |
  2   |  chip power enable                | 1 = chip enabled <br> 0 = deep sleep

##### 0x35
 bit  | function                          |            notes
 ---  |   ---                             |   --- 
15:0  |  DTMF tone freq 1                 | register_value = freq  (2^12)

##### 0x36
 bit  | function                          |            notes
 ---  |   ---                             |   --- 
15:0  |  DTMF tone freq 2                 | register_value = freq  (2^12)

##### 0x3C
 bit  | function                          |            notes
 ---  |   ---                             |   --- 
15:14 |  voice selection                  | 00  = Tx voice signal from MIC <br> 01 = Tx inner sine tone setted by tone2 <br> 10 = Tx code from GPIO1 code_in (gpio1<1:0> must be
set to 01) <br> 11 = not Tx any signal

##### 0x41
 bit  | function                          |            notes
 ---  |   ---                             |   --- 
15:0  |  VOX open threshold               |

##### 0x42
 bit  | function                          |            notes
 ---  |   ---                             |   --- 
15:0  |  VOX close threshold              |

##### 0x43
 bit  | function                          |            notes
 ---  |   ---                             |   --- 
12:6  |  transmitter FM deviation         | CTC/DCS and voice FM deviation
 5:0  |  transmitter FM deviation         | CTC/DCS only FM deviation

##### 0x44
 bit  | function                          |            notes
 ---  |   ---                             |   --- 
 7:4  |  RX voice range - volume 1        | (0000)-15dB\~(1111)0dB, step 1dB
 3:0  |  RX voice range - volume 2        | (0000)-15dB\~(1111)0dB, step 1dB

##### 0x45
 bit  | function                          |            notes
 ---  |   ---                             |   --- 
15:14 |  tail elimi. CTCSS phase shift    | 00 = 120 degree shift<br> 01 = 180 degree shift<br> 10 = 240 degree shift<br> 11 = reserved
  11  |  enable CDCSS detection           |
  10  |  enable CTC/DCS detection         |
  7   |  enable CDCSS inverse code detect |
  4   |  CDCSS code selection             | 1 = 24 bit code<br>0 = 23 bit code
  3   |  CTCSS code selection             | 1 = ctcss_cmp/cdcss_cmp out via gpio<br> 0 = ctcss/cdcss sdo out vio gpio
 2:0  |  CTC/DCS mode selection           | 000 = disable<br> 001 = inner ctcss en<br> 010 = inner cdcss en 101 = outter ctcss en<br> 110 = outter cdcss en <br> others = disable

##### 0x48
 bit  | function                          |            notes
 ---  |   ---                             |   --- 
 9:0  |  squelch open threshold           |

##### 0x49
 bit  | function                          |            notes
 ---  |   ---                             |   --- 
 9:0  |  squelch close threshold          |

##### 0x4A
 bit  | function                          |            notes
 ---  |   ---                             |   --- 
15:0  |  CTCSS frequency                  | CTCSS freq = reg_value2^16 khz.<br> It must be set to 134.4Hz when use standard cdcss mode.<br> When use ctcss/cdcss, this register must be set both in rx and tx state

##### 0x4B
 bit  | function                          |            notes
 ---  |   ---                             |   --- 
 7:0  |  CDCSS code [23:16]               | MSB sent first

##### 0x4A
 bit  | function                          |            notes
 ---  |   ---                             |   --- 
15:0  |  CDCSS code [23:16]               |

##### 0x54
 bit  | function                          |            notes
 ---  |   ---                             |   --- 
  7   |  squelch output selection         | If 1, the output gpio6 is sq & css_cmp, else, the output gpio is sq only.

##### 0x58
 bit  | function                            |            notes
 ---  |   ---                               |   --- 
 9:7  | RX CTC/DCS hi-pass filter bandwidth | 000(narrow)\~111(wide)
  6   | RX CTC/DCS hi-pass filter bypass    | 1 = bypass<br> 0 = normal
 5:4  | voice low-pass filter bypass        | 11 = bypass<br> 0 = normal
  3   | pre/de-emphasis bypass              | 1 = bypass<br> 0 = normal
  2   | CTC/DCS low-pass filter bypass      | 1 = bypass<br> 0 = normal
 1:0  | voice hi-pass filter bypass         | 11 = bypass<br> 0 = normal

##### 0x5C
 bit  | function                          |            notes
 ---  |   ---                             |   --- 
  12  |  DTMF idle                        |
  10  |  RX enabled                       |
  9   |  TX enabled                       |
  7   |  CTCSS phase shift detected       |
  2   |  CTC/DCS compared                 |
  1   |  squelch status                   |
  0   |  VOX status                       |

##### 0x5F
 bit  | function                          |            notes
 ---  |   ---                             |   --- 
 9:0  |  RSSI level                       | read only register, unit 1/8dB

##### 0x60
 bit  | function                          |            notes
 ---  |   ---                             |   --- 
14:0  |  voice signal strength indication | read only register, unit mV

##### 0x63
 bit  | function                          |            notes
 ---  |   ---                             |   --- 
 9:8  |  DTMF mode                        | 11 = transmit or receive Dtmf single tone2<br> 01 =transmit or receive Dtmf dual tone1+tone2<br> others = disable
 7:4  |  DTMF time 1                      | Time interval for dual tone transmission. Time = register_value5ms
 3:0  |  DTMF time 2                      | Time interval for DTMF idle state. Time = register_value5ms

##### 0x66
 bit | function                           |            notes
 --- |   ---                              |   --- 
15:8 | DTMF C0                            | 01100001 12.8MHz and 25.6MHz<br> 01100001 13MHz and 26MHz
 7:0 | DTMF C1                            | 01011011 12.8MHz and 25.6MHz<br> 01011110 13MHz and 26MHz

##### 0x67
 bit | function                           |            notes
 --- |   ---                              |   --- 
15:8 | DTMF C2                            | 01010011 12.8MHz and 25.6MHz<br> 01010111 13MHz and 26MHz
 7:0 | DTMF C3                            | 01001011 12.8MHz and 25.6MHz<br> 01001011 13MHz and 26MHz

##### 0x68
 bit | function                           |            notes
 --- |   ---                              |   --- 
15:8 | DTMF C4                            | 00101100 12.8MHz and 25.6MHz<br> 00110001 13MHz and 26MHz
 7:0 | DTMF C5                            | 00011110 12.8MHz and 25.6MHz<br> 00011110 13MHz and 26MHz

##### 0x69
 bit | function                           |            notes
 --- |   ---                              |   --- 
15:8 | DTMF C6                            | 00001010 12.8MHz and 25.6MHz<br> 00001111 13MHz and 26MHz
 7:0 | DTMF C7                            | 11110110 12.8MHz and 25.6MHz<br> 11111011 13MHz and 26MHz

##### 0x6C
 bit | function                           |            notes
 --- |   ---                              |   --- 
10:5 | DTMF index [5:0]                   | [5:3] tone1 detect index<br> [2:0] tone2 detect index, will be used in single tone mode
  4  | DTMF code not valid                |
 3:0 | DTMF code [3:0]                    |

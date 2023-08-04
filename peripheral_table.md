# Peripheral allocation table

This page provides an overview on which hardware peripherals are used by the OpenRTX firmware on the various platforms.

## STM32F405

### MD-3x0

| Peripheral     | Status  | Function |
|:--------------:|:-------:|:--------:|
|  FSMC          | USED    | Memory-mapped display management |
|  TIM1          |         |          |
|  TIM2          | USED    | Time base for ADC sampling rate in audio input stream driver |
|  TIM3          | USED    | PWM time base for tone generator: CH2 CTCSS, CH3 "beep", 109.375kHz |
|  TIM4          |         |          |
|  TIM5          |         |          |
|  TIM6          |         |          |
|  TIM7          | USED    | Tone generator time base for DMA transfers when in AFSK/audio mode |
|  TIM8          | USED    | CH1 LCD backlight dimming, 100kHz, 8 bit |
|  TIM9          |         |          |
|  TIM10         |         |          |
|  TIM11         |         |          |
|  TIM12         |         |          |
|  TIM13         |         |          |
|  TIM14         | USED    | Tone generator time base for CTCSS/beep sine wave generation |
|  USART1        |         |          |
|  USART2        |         |          |
|  USART3        | USED    | GPS data RX |
|  SPI1          | USED    | External flash memory |
|  SPI2          |         |          |
|  SPI3          |         |          |
|  ADC1          | USED    | Sampling of Vbat, RSSI, volume pot. and vox level. Free-running conversion. |
|  ADC2          | USED    | Sampling of audio data in input stream driver |
|  ADC3          |         |          |
| DMA1, Stream 0 |         |          |
| DMA1, Stream 1 |         |          |
| DMA1, Stream 2 | USED    | Transfer of audio samples to PWM in tone generator, very high priority |
| DMA1, Stream 3 |         |          |
| DMA1, Stream 4 |         |          |
| DMA1, Stream 5 |         |          |
| DMA1, Stream 6 |         |          |
| DMA1, Stream 7 |         |          |
| DMA2, Stream 0 | USED    | Transfer of ADC samples, middle priority |
| DMA2, Stream 1 |         |          |
| DMA2, Stream 2 | USED    | Transfer of ADC2 samples in input stream driver, high priority |
| DMA2, Stream 3 |         |          |
| DMA2, Stream 4 |         |          |
| DMA2, Stream 5 |         |          |
| DMA2, Stream 6 |         |          |
| DMA2, Stream 7 | USED    | Framebuffer transfer to display, low priority. |


### MD-UV3x0

| Peripheral     | Status  | Function |
|:--------------:|:-------:|:--------:|
|  FSMC          | USED    | Memory-mapped display management |
|  TIM1          |         |          |
|  TIM2          | USED    | Time base for ADC sampling rate in audio input stream driver |
|  TIM3          | USED    | PWM time base for tone generator: CH2 CTCSS, CH3 "beep", 109.375kHz |
|  TIM4          |         |          |
|  TIM5          |         |          |
|  TIM6          |         |          |
|  TIM7          | USED    | Tone generator time base for DMA transfers when in AFSK/audio mode |
|  TIM8          |         |          |
|  TIM9          |         |          |
|  TIM10         |         |          |
|  TIM11         | USED    | Timebase for LCD backlight dimming software PWM |
|  TIM12         |         |          |
|  TIM13         |         |          |
|  TIM14         | USED    | Tone generator time base for CTCSS/beep sine wave generation |
|  USART1        |         |          |
|  USART2        |         |          |
|  USART3        | USED    | GPS data RX |
|  SPI1          | USED    | External flash memory |
|  SPI2          |         |          |
|  SPI3          |         |          |
|  ADC1          | USED    | Sampling of Vbat, RSSI, volume pot. and vox level. Free-running conversion. |
|  ADC2          | USED    | Sampling of audio data in input stream driver |
|  ADC3          |         |          |
| DMA1, Stream 0 |         |          |
| DMA1, Stream 1 |         |          |
| DMA1, Stream 2 | USED    | Transfer of audio samples to PWM in tone generator, very high priority |
| DMA1, Stream 3 |         |          |
| DMA1, Stream 4 |         |          |
| DMA1, Stream 5 |         |          |
| DMA1, Stream 6 |         |          |
| DMA1, Stream 7 |         |          |
| DMA2, Stream 0 | USED    | Transfer of ADC samples, middle priority |
| DMA2, Stream 1 |         |          |
| DMA2, Stream 2 | USED    | Transfer of ADC2 samples in input stream driver, high priority |
| DMA2, Stream 3 |         |          |
| DMA2, Stream 4 |         |          |
| DMA2, Stream 5 |         |          |
| DMA2, Stream 6 |         |          |
| DMA2, Stream 7 | USED    | Framebuffer transfer to display, low priority. |


### GDx
* ADC0: Vbat and vox level, sampling on API call.
* FTM0: backlight PWM, 58.5kHz, 8 bit

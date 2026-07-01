# Circuit Bending Setup

ESP32-S3-DevKitC-1 based circuit bending board with camera, TFT touchscreen display, SD card, I2S speaker amp, and analog microphone.

## Hardware

- **MCU:** ESP32-S3-DevKitC-1 (N16R8)
- **Camera:** OV2640 via 24-pin 1mm pitch FPC connector (J3)
- **Display:** TFT touchscreen (J4)
- **Storage:** MicroSD card (J27, Hirose DM3AT-SF-PEJM5)
- **Speaker amp:** MAX98357A I2S class D amp (U2)
- **Microphone:** MAX4466 analog mic preamp (U4)
- **Power:** IP2312 LiPo charger (U5) + MT3608 boost converter to 5V (U1)
- **LEDs:** 3x WS2812B addressable RGB (D5, D6, D7), daisy chained
- **Bend points:** Single pin 2.54mm headers throughout (J2, J5, J10–J26 etc.)

## Pin Map (WIll be updated and corrected soon)

### Camera (OV2640 DVP 8-bit)

| Signal | GPIO |
|---|---|
| xclk | 15 |
| pclk | 13 |
| vsync | 6 |
| href | 7 |
| d2 | 11 |
| d3 | 9 |
| d4 | 8 |
| d5 | 10 |
| d6 | 12 |
| d7 | 18 |
| d8 | 17 |
| d9 | 16 |
| sda (SCCB) | 4 |
| scl (SCCB) | 5 |

> pwdn and rstb are hardwired by R7/R8/R10, no GPIO needed.
> d0 and d1 are not connected (OV2640 outputs 8-bit only).

### Shared SPI Bus (TFT + touch + SD)

| Signal | GPIO |
|---|---|
| sck | 39 |
| mosi | 40 |
| miso | 41 |
| lcdcs | 42 |
| lcdrst | 38 |
| lcddc/rs | 47 |
| t_irq | 21 |
| sdcs | 43 |
| t_cs | 44 |

### I2S Amp (MAX98357A)

| Signal | GPIO |
|---|---|
| din | 2 |
| bclk | 14 |
| lrclk | 48 |

> sd_mode and gain are hardwired via R1, not software controlled.

### Microphone (MAX4466)

| Signal | GPIO |
|---|---|
| mic | 1 |

> Must be ADC1 (GPIO1-10) — ADC2 is unreliable with WiFi active.

### LEDs (WS2812B)

| Signal | GPIO |
|---|---|
| led_data | 0 |

> D5 → D6 → D7 daisy chained. One GPIO drives all three.

## Pins Avoided and Why

| GPIO | Reason |
|---|---|
| 3, 45, 46 | Strapping pins |
| 19, 20 | Native USB D-/D+ |
| 35, 36, 37 | Internally wired to octal PSRAM on N16R8 |

## Power

- Battery: 3.7V LiPo via JST PH 2.0mm (BT1)
- Charging: IP2312 module (U5) via USB-C
- 5V rail: MT3608 boost converter (U1) from battery
- 3.3V rail: onboard ESP32-S3 regulator
- 1.2V rail: NIC5365 LDO (U6) for camera
- 2.8V rail: MIC5504 LDO (U7) for camera

## Flashing

Flash and monitor over the **native USB port** (GPIO19/20, labeled "USB" on the DevKitC-1). Do not use the UART port (labeled "UART") — GPIO43/44 are repurposed for SPI chip selects and that port will show garbage. Select **USB CDC On Boot** in your build config.

## Built in KiCad
- KiCad E.D.A. 9.0.6

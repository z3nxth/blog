---
title: Installing Custom Firmware onto the T-Embed CC1101
date: 2025-06-27
categories: Hardware
tags:
  - guide
  - hardware
  - tools
---
The Lilygo T-Embed CC1101 is a **pocket-sized dev board with in-built Sub-GHz, IR, NFC, WiFi, BLE, BadUSB and more.** It uses an **ESP-32 S3** for the microcontroller. I've made this blog as I've noticed as the community is still small, and more tutorials always need to be made.

## Introduction
 
_⚠️ Disclaimer: Don't be a skid, use everything legally and responsibly :)_
The Lilygo T-Embed CC1101 is a **pocket-sized dev board with in-built Sub-GHz, IR, NFC, WiFi, BLE, BadUSB and more.** It uses an **ESP-32 S3** for the microcontroller. I've made this blog as I've noticed as the community is still small, and more tutorials always need to be made.


Using custom firmware (ie. [Bruce](https://bruce.computer/)) takes these capabilities to the next level, truly allowing them to become the cheaper "swiss army knife" alternative to the Flipper Zero.
## Hardware and Specifications

- **Microcontroller**: ESP32-S3-WROOM-1
- **Display**: 1.9" ST7789V TFT (320x17
- **Radio**: CC1101 Sub-GHz transceiver (300–928 MHz)
- **NFC**: PN532 module
- **Audio**: Microphone and I2S speaker support
- **LEDs**: 8x WS2812 RGB LEDs
- **Battery**: 3.7V 1300mAh LiPo with BQ25896 charger and BQ27220 fuel gauge
- **Interfaces**: USB-C, SPI, I2C, UART(GitHub, Bastelgarage)

## Setup Guide

1. Pull the magnetic back off the TMB.
    ![MagneticBack](/assets/images/magneticback.png)
2. Unplug the battery cable.
    ![BatteryCable](/assets/images/batterycable.png)
3. Visit [Bruce Flasher](https://bruce.computer/flasher) to flash the TMB. Select the **Latest release > Lilygo > T-Embed CC1101**
4. While holding the **central button on the encoder and the RST button on the PCB** (next to the ESP32S3 Chip), plug the TMB into your computer via a USB-C cable **_that supports data transfer.**_
![RST](/assets/images/rst.png)

5. Click **install**. Select the T-Embed's serial port and begin to flash.

Bruce should be running successfully on the TMB now.

## Usage and Conclusion

[MITM's Video covering EVERYTHING](https://www.youtube.com/watch?v=BUwrWKDqtak)

> [! TIP]  
> To power off your TMB navigate to the home screen, hold the button on the top and rotate the encoder anti-clockwise until a message showing "Entering Deep Sleep [1/3]" shows up. Hold for 3 seconds. Afterwards, to power on again simply hold the top button.

Bruce, in its usage becomes more straightforward over time. On the whole, I have noticed that the TMB is less "polished" than my flipper zero, which frankly I have no right to complain due to its outstanding price and usage of CFW.
Sometimes, I'd notice when running a program that I couldn't exit out of it with the top key or anything, and would have to remove the cover and reboot through the little buttons under the case.

Additionally, what I think is a great shame is the lack of ability to emulate NFC and to even read RFID. I think through an external module, however, this would be possible.

Apart from those caveats, I have no complaints with the TMBCC1101 and would recommend, but perhaps not to a complete beginner as usage and installation is a bit counter-intuitive, as opposed to for example the Flipper Zero with [Momentum](https://momentum-fw.dev/).

## Next Steps

- Explore the **Bruce wiki** **to familiarise yourself with Bruce's amazing capabilities.**
- Install the IRDB onto the SD card, to be able to access a wide database of **IR files and remotes.**
- Install a popular SubGHz repo onto the SD card, allowing you to access a **wide range of .sub files.**
- Customise your TMB by using a **theme**
- Expand the TMB's range of **SubGHz by using an external CC1101 Module** [Tutorial soon]
- Expand the TMB's capabilities by adding an NRF module to add **jamming capabilities.** [Tutorial soon]


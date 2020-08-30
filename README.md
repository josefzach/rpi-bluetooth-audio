# rpi-bluetooth-audio
Yet another headless bluetooth A2DP bluetooth audio setup that lets you stream music from a bluetooth device to your raspberry pi.

# Prerequisits
* Raspberry Pi 1(B)
* Bluetooth dongle
* Raspbian OS Buster (2020-08-20-raspios-buster-armhf-lite)

# Instructions

## Clean Raspbian OS Installation

* Download image from https://www.raspberrypi.org/downloads/raspberry-pi-os/
* flash the image using dd: ´´´unzip -p 2020-08-20-raspios-buster.zip | sudo dd of=/dev/sdX bs=4M status=progress conv=fsync
´´´


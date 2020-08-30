# rpi-bluetooth-audio
Yet another headless bluetooth A2DP bluetooth audio setup that lets you stream music from a bluetooth device to your raspberry pi.
Why another raspberri pi bluetooth audio instruction?
1. For myself as a reference. If it is of help to somebody out there - even better.
2. This instruction is based on a few instructions that I found on the net. It is a collection of the parts that best fit my needs. All instructions are listed as references below.

# About
This project is about using an old Raspberry 1B as a headless bluetooth audio receiver for streaming music to my car´s hi-fi. The requirements were to 

# Prerequisits
* Raspberry Pi 1(B)
* Bluetooth dongle
* Raspbian OS Buster (2020-08-20-raspios-buster-armhf-lite)

# Instructions

## Clean Raspbian OS Installation

1. Download image from https://www.raspberrypi.org/downloads/raspberry-pi-os/
2. Flash to sd card: `unzip -p 2020-08-20-raspios-buster.zip | sudo dd of=/dev/sdX bs=4M status=progress conv=fsync`
3. Change default user passwort with `passwd` (default password is `raspberry`)
4. `sudo raspi-config` to resize partition to use all space available on sd card
5. `sudo apt-get update`, `sudo apt-get upgrade`
6. `sudo usermod -a -G bluetooth pi` to add user pi to the bluetooth group
7. `sudo raspi-config` to set automatic log in for user pi (can be skipped?)

## Configure Bluetooth Stack
1. Make RPi discoverable as bluetooth sink (can be skipped?)
   - `sudo nano /etc/bluetooth/main.conf`
   - Uncomment/Edit `Class=0x41C`
   - Uncomment/Edit `DiscoverableTimeout = 0`
   - `sudo systemctl restart bluetooth.service`
2. Set-up bluetooth connection agent (with PIN)
   - `sudo apt-get install bluez-tools`
   - Install new service `bt-agent.service`
   ``` [Unit]
Description=Bluetooth Auth Agent
After=bluetooth.service
PartOf=bluetooth.service

[Service]
Type=simple
ExecStart=/usr/bin/bt-agent -c NoInputNoOutput -p /etc/bluetooth/pin.conf
ExecStartPost=/bin/sleep 1
ExecStartPost=/bin/hciconfig hci0 sspmode 0```

   
## Set-up Audio Streaming
2. Install BlueZ ALSA (link between bluetooth and ALSA): `sudo apt-get install bluealsa`
3. Add line `bluealsa-aplay 00:00:00:00:00:00` to file `/etc/rc.local` to forward audio to audio device
   - Note: Attempts to install a service `a2dp-playback.service` as suggested in [2] were not successful



# References
1. ['Another How to turn your Pi in a Bluetooth Speaker Tutorial'](https://www.raspberrypi.org/forums/viewtopic.php?f=35&t=235519&sid=632ae5b5a8d9d618e8c36c154af730b3) by DrFunk. Uses PulseAudio. I found the part about automatic bluetooth pairing especially helpful!
2. [Headless A2DP Audio Streaming on Raspbian Stretch](https://gist.github.com/Pindar/e259bec5c3ab862f4ff5f1fbcb11bfc1) by Pindar. 
3. [Musik per Bluetooth an einen Lautsprecher senden (German)](https://www.raspberry-pi-geek.de/ausgaben/rpg/2018/04/musik-per-bluetooth-an-einen-lautsprecher-senden/) by Bernhard Bablok

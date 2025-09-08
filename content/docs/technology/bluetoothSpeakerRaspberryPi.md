+++
title = 'Bluetooth Speaker'
date = 2018-11-13T20:45:00
draft = false
tags = ["RaspberryPi", "speaker", "bluetooth"]
categories = ["tutorials", "raspberry-pi"]
+++

# Raspberry Pi : Connect Bluetooth Speaker

Connecting Raspberry Pi to Mi Pocket Speaker 2

```pi@raspberrypi:~ $ sudo bluetoothctl
[bluetooth]# power on
Changing power on succeeded
[bluetooth]# agent on
Agent registered
[bluetooth]# scan on
Discovery started
[CHG] Controller B8:27:EB:79:F6:B7 Discovering: yes
[bluetooth]# default-agent
Default agent request successful
[CHG] Device 00:EC:0A:57:F2:65 LegacyPairing: yes
[bluetooth]# list
Controller B8:27:EB:79:F6:B7 raspberrypi [default]
[bluetooth]# devices
Device 5C:72:FE:B7:35:FD 5C-72-FE-B7-35-FD
Device 28:F0:76:0A:B9:7B 28-F0-76-0A-B9-7B
Device 00:EC:0A:57:F2:65 Redmi
[NEW] Device 74:A3:4A:15:DD:71 Mi Pocket Speaker 2
[bluetooth]# pair 74:A3:4A:15:DD:71
Attempting to pair with 74:A3:4A:15:DD:71
[CHG] Device 74:A3:4A:15:DD:71 Connected: yes
[CHG] Device 74:A3:4A:15:DD:71 UUIDs:
        00001108-0000-1000-8000-00805f9b34fb
        0000110b-0000-1000-8000-00805f9b34fb
        0000110c-0000-1000-8000-00805f9b34fb
        0000110e-0000-1000-8000-00805f9b34fb
        0000111e-0000-1000-8000-00805f9b34fb
[CHG] Device 74:A3:4A:15:DD:71 Paired: yes
Pairing successful
[CHG] Device 74:A3:4A:15:DD:71 Connected: no
[bluetooth]# info 74:A3:4A:15:DD:71
Device 74:A3:4A:15:DD:71
        Name: Mi Pocket Speaker 2
        Alias: Mi Pocket Speaker 2
        Class: 0x240404
        Icon: audio-card
        Paired: yes
        Trusted: no
        Blocked: no
        Connected: no
        LegacyPairing: yes
        UUID: Headset                   (00001108-0000-1000-8000-00805f9b34fb)
        UUID: Audio Sink                (0000110b-0000-1000-8000-00805f9b34fb)
        UUID: A/V Remote Control Target (0000110c-0000-1000-8000-00805f9b34fb)
        UUID: A/V Remote Control        (0000110e-0000-1000-8000-00805f9b34fb)
        UUID: Handsfree                 (0000111e-0000-1000-8000-00805f9b34fb)
[bluetooth]# trust 74:A3:4A:15:DD:71
[CHG] Device 74:A3:4A:15:DD:71 Trusted: yes
Changing 74:A3:4A:15:DD:71 trust succeeded
[bluetooth]# info 74:A3:4A:15:DD:71
Device 74:A3:4A:15:DD:71
        Name: Mi Pocket Speaker 2
        Alias: Mi Pocket Speaker 2
        Class: 0x240404
        Icon: audio-card
        Paired: yes
        Trusted: yes
        Blocked: no
        Connected: no
        LegacyPairing: yes
        UUID: Headset                   (00001108-0000-1000-8000-00805f9b34fb)
        UUID: Audio Sink                (0000110b-0000-1000-8000-00805f9b34fb)
        UUID: A/V Remote Control Target (0000110c-0000-1000-8000-00805f9b34fb)
        UUID: A/V Remote Control        (0000110e-0000-1000-8000-00805f9b34fb)
        UUID: Handsfree                 (0000111e-0000-1000-8000-00805f9b34fb)
[bluetooth]# quit

pi@raspberrypi:~ $ sudo apt-get update && sudo apt-get install pulseaudio -y
pi@raspberrypi:~ $ mkdir -p scripts
pi@raspberrypi:~ $ vim scripts/autopair
#!/bin/bash
bluetoothctl << EOF
connect [enter your MAC add]
EOF
pi@raspberrypi:~ $ chmod +x !$
chmod +x scripts/autopair
pi@raspberrypi:~ $ sudo vim /boot/config.txt
#dtparam=audio=on
pi@raspberrypi:~ $ vim ~/scripts/on.py
#!/usr/bin/python```

## Monitor removal of bluetooth reciever

```import os
import sys
import subprocess
import time
def blue_it():
    status = subprocess.call('ls /dev/input/event0 2>/dev/null', shell=True)
    while status == 0:
        print("Bluetooth UP")
        print(status)
        time.sleep(15)
        status = subprocess.call('ls /dev/input/event0 2>/dev/null', shell=True)
    else:
        waiting()
def waiting():
    subprocess.call('killall -9 pulseaudio', shell=True)
    time.sleep(3)
    subprocess.call('pulseaudio --start', shell=True)
    time.sleep(2)
    status = subprocess.call('ls /dev/input/event0 2>/dev/null', shell=True)
    while status == 2:
        print("Bluetooth DOWN")
        print(status)
        subprocess.call('~/scripts/autopair', shell=True)
        time.sleep(15)
        status = subprocess.call('ls /dev/input/event0 2>/dev/null', shell=True)
    else:
        blue_it()
blue_it()
pi@raspberrypi:~ $chmod +x ~/scripts/on.py
pi@raspberrypi:~ $vim ~/.bashrc
pulseaudio --start
wait
~/python/on.py```

### Reboot

If the connection fails, remove the device and pair-trust-connect

```remove aa:bb:cc:dd:ee:ff

### Test the sound

```pi@raspberrypi:~ $wget http://rpf.io/lamp3 -O example.mp3 --no-check-certificate
pi@raspberrypi:~ $ omxplayer -o alsa ~/example.mp3```

When you connect two Bluetooth devices together (i.e. you "pair" them) you only establish a connection between the two: network link, audio sink, audio source. If you connect your Pi to a Bluetooth audio source, it doesn't necessarily mean you want to use the new audio sink at once. You need a different command for that.
In short, under GNU/Linux in this very case:
- there is a command for pairing Bluetooth devices
- there is a command to select what audio device will play the sound

These two do not overlap.
Selecting what device plays sound is done through a dedicated application called PulseAudio daemon (a service that runs in the background), which manages sound on most GNU/Linux distributions. It doesn't control Bluetooth, the latter of which is managed by the Bluez daemon. That's why you have two separate commands.

If you want to play audio through a recently paired Bluetooth audio device, here's what to do:
list audio sinks : pacmd list-sinks | grep -E 'name:|index' . The * locates the currently selected sink. Note what follows name: for the Bluetooth device the Pi is connected to.
set the default audio sink : pacmd set-default-sink <the name of the desired BT audio sink>

---
{{< home-link "Home" >}} | {{< section-index >}}
+++
title = 'Raspberry Pi Setup'
date = 2013-07-24T10:55:52+01:00
draft = false
tags: [RaspberryPi]
categories: [tutorials, raspberry-pi]
+++
## Setup

Steps I followed to set-up my raspberry Pi with Raspbian

Also configure -
- XBMC (for playing videos)
- NFS (to access external hard drive from any device in the house)
- VNC viewer (to access the desktop from any device in the house)

### Format SD Card

- **Windows**
1. Download SD Formatter
https://www.sdcard.org/downloads/formatter_4/
2. Install SD Formatter and run the software
3. Set "FORMAT SIZE ADJUSTMENT" to ON in the Options menu.
4. Make sure you have selected the Drive your SD Card is inserted in
5. Format the memory card

- **Linux**
1. Use gparted (or the command-line version parted if you prefer), if you don't have it, install it as you usually would.
2. Format the entire disk as FAT / FAT16 (make sure you select the correct disk)

### Install OS

**NOOBS**
1. Download NOOBS (New Out Of Box Software)
	http://www.raspberrypi.org/downloads
2. Unzip the compressed file to memory card
3. Insert the card into the memory slot on the Raspberry Pi, connect monitor, keyboard, mouse; and then turn on the power supply.
4. You get a NOOBS menu, I wanted to install Rasbian so I selected the same from the menu. (remember on first start NOOBS selects HDMI as the output so any other display you need to press  the number buttons 1-4 on your keyboard)
5. Rasbian will get installed typically in less than 5 minutes, you get another menu at this point where you need to click on “Finish.” (Your mouse will not work at this point and you need a keyboard to do this. If you dont have a keyboard remember your ssh is configured by now so you can ssh from a remote machine to Raspberry Pi and do this from your ssh window.)

> login as: pi  
> pi@192.168.0.6's password:  
> Linux raspberrypi 3.6.11+ #474 PREEMPT Thu Jun 13 17:14:42 BST 2013 armv6l  

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.

NOTICE: the software on this Raspberry Pi has not been fully configured. Please run 'sudo raspi-config'

> pi@raspberrypi ~ $ sudo raspi-config

┌─────────┤ Raspberry Pi Software Configuration Tool (raspi-config) ├──────┐
│ Setup Options                                                                │
│                                                                              │
│    1 Expand Filesystem              Ensures that all of the SD card s        │
│    2 Change User Password           Change password for the default u        │
│    3 Enable Boot to Desktop         Choose whether to boot into a des        │
│    4 Internationalisation Options   Set up language and regional sett        │
│    5 Enable Camera                  Enable this Pi to work with the R        │
│    6 Add to Rastrack                Add this Pi to the online Raspber        │
│    7 Overclock                      Configure overclocking for your P        │
│    8 Advanced Options               Configure advanced settings              │
│    9 About raspi-config             Information about this configurat        │
│                                                                              │
│                                                                              │
│                     <Select>                     <Finish>                    │
│                                                                              │
└────────────────────────────────────────────────────┘
| Tables   |      Are      |  Cool |
|----------|:-------------:|------:|
| col 1 is |  left-aligned | $1600 |
| col 2 is |    centered   |   $12 |
| col 3 is | right-aligned |    $1 |

Notes:
If using RCA for display edit /boot/config.txt, uncomment following line
sdtv_mode=2

References:
http://elinux.org/RPi_Easy_SD_Card_Setup

────────────────────────────────────────────────────
Stock No. Description Price
[756-8308](https://uk.rs-online.com/web/p/microcontroller-development-tools/7568308) Raspberry Pi Type B Single Board Computer 512MB £26.00  
[726-3069](http://uk.rs-online.com/web/p/plug-in-power-supply/7263069/) Micro USB UK power supply for Raspberry Pi £5.19  
Case-CLR Pi Type B Case - Clear £3.99  
[3A114805](http://www.amazon.co.uk/dp/B007PYBOEU/ref=pe_385721_37038051_pe_217191_31005151_M3T1_dp_i1) SanDisk Ultra 16GB SDHC UHS-I Class 10 Card £10.73  
────────────────────────────────────────────────────

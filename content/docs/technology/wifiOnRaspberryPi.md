+++
title = 'WiFi'
date = 2013-12-14T22:47:00
draft = false
tags = ["RaspberryPi", "WiFi"]
categories = ["tutorials", "raspberry-pi"]
+++

# Wi-Fi Set-up for Raspberry Pi

Make sure your interface file and wpa_supplicant.conf look like the ones below -

---
```pi@raspberrypi ~ $ sudo nano /etc/network/interfaces

auto lo

iface lo inet loopback
iface eth0 inet dhcp

allow-hotplug wlan0
iface wlan0 inet dhcp
wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
iface default inet dhcp

pi@raspberrypi ~ $ sudo nano /etc/wpa_supplicant/wpa_supplicant.conf

ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
ssid="YOURSSID"
psk="YOURPASSWORD"

# Protocol type can be: RSN (for WP2) and WPA (for WPA1)
proto=WPA

# Key management type can be: WPA-PSK or WPA-EAP (Pre-Shared or Enterprise)
key_mgmt=WPA-PSK

# Pairwise can be CCMP or TKIP (for WPA2 or WPA1)
pairwise=TKIP

#Authorization option should be OPEN for both WPA1/WPA2 (in less commonly used are SHARED and LEAP)
auth_alg=OPEN
}


pi@raspberrypi ~ $ sudo reboot```

To view all WiFi networks -
```pi@raspberrypi ~ sudo iwlist wlan0 scan | grep ESSID```

To view WiFi network interface -
```pi@raspberrypi ~ $ iwconfig```

## 💾 Hardware Info

```
────────────────────────────────────────────────────
Stock No. Description Price
760-3621 Edimax Wireless Nano USB Adapter £8.95
────────────────────────────────────────────────────
```

---
{{< home-link "Home" >}} | {{< section-index >}}  

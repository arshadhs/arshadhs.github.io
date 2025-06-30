+++
title = "Camera with Raspberry Pi"
date = 2013-12-12T22:51:00
draft = false
tags = ["RaspberryPi"]
categories = ["tutorials", "raspberry-pi"]
+++

# 📷 Camera with Raspberry Pi

A quick guide to setting up the official Raspberry Pi camera module and installing useful Python imaging tools.

---

## 🔧 System Update

Update your Raspberry Pi system and firmware:

```bash
sudo apt-get install rpi-update
sudo apt-get update
sudo apt-get upgrade
```

---

## 🎛️ Enable the Camera

Access the configuration menu to enable the camera:

```bash
sudo raspi-config
```

- Navigate to **Interface Options**
- Select **Camera** and choose **Enable**
- Then reboot your Pi:

```bash
sudo reboot
```

---

## 🐍 Install Python Imaging Tools

Install Python Imaging (PIL/Tk) for working with images:

```bash
sudo apt-get install python-imaging-tk
```

---

## 💾 Hardware Info

```
────────────────────────────────────────────────────────────
Stock No.   Description                          Price (ex. VAT)
775-7731    Raspberry Pi HD Video Camera module  £16.03
────────────────────────────────────────────────────────────
```

This module allows you to capture still images and video with commands like `raspistill` and `raspivid`.

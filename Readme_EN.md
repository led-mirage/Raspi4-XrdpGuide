# Installation Guide for xrdp on Raspberry Pi 4

Created on August 22, 2023  
Updated on August 22, 2023  
Copyright (c) 2023 led-mirage

## Introduction

This guide explains the procedure to install xrdp on Raspberry Pi 4.

By installing xrdp, you can remotely connect to the Raspberry Pi 4 from a Windows PC via Remote Desktop Protocol (RDP). VNC might also work, but personally, I find RDP to be more user-friendly.

The environment used for verification is as follows:

- Raspberry Pi 4 Model B 4GB
- Raspberry Pi OS 64bit Bullseye
- xrdp 0.9.17
- Windows 10 / 11 (RDP client side)

## Precautions

The goal is to enable remote control of the Raspberry Pi without connecting to a display. However, please note that proper connection and operation cannot be performed without the following two settings:

- Editing the xorg.conf file
- Enabling VNC

Details will be explained later.

## Installation

Install xrdp by executing the following commands from the terminal:

```bash
sudo apt update
sudo apt install xrdp
```

After installation, you can check the version with the following command:

```bash
xrdp --version
```

## Editing the xorg.conf File

You might think that this is the end, but for some reason, you cannot remotely connect from Windows with just this. Open and edit the following file:

```
/etc/X11/xrdp/xorg.conf
```

You will need administrative rights to save, so use the following command to open the text editor:

```bash
sudo nano /etc/X11/xrdp/xorg.conf
```

Once opened, edit the following line located towards the bottom:

Before editing
```conf
    Option "DRMDevice" "/dev/dri/renderD128"
```

After editing
```conf
    #Option "DRMDevice" "/dev/dri/renderD128"
    Option "DRMDevice" ""
```

After editing, save the file with Ctrl + O -> Enter. Exit nano with Ctrl + X.

## Restarting xrdp

Restart xrdp with the following command:

```bash
sudo systemctl restart xrdp
```

## Enabling VNC

With the above steps, you can connect, but if VNC is not enabled, for some reason, the operation from a Windows PC becomes abnormally slow. Enable VNC with the following operation:

```
Settings -> Raspberry Pi Configuration -> Interfaces -> VNC (Enabled)
```

This phenomenon seems to occur when connecting to a Raspberry Pi without a display via RDP. If your Raspberry Pi is always connected to a display, this step might not be necessary. Try it out and if you feel the response is very slow, you may want to configure it.

## Connection Test

Please shut down the Raspberry Pi, disconnect the display cable, and restart.

Then, if you can connect and operate without problems from a Windows PC via Remote Desktop Connection, it's OK.

## Reference URLs

https://github.com/neutrinolabs/xrdp/issues/2060  
https://qiita.com/otti83/items/1855807b87ca7a0500dc

---
This document was translated into English by ChatGPT.
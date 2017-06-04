---
layout: post
title : "WOL with Realteak NICs on Windows 10"
date : 26.08.2015 17:16:00
tags: [wol, win10, realtek]
---
{% include JB/setup %}

## TL;DR

To restore WOL functionality disable Fast Start and install the Realtek device driver from [here](http://www.realtek.com.tw/downloads/downloadsView.aspx?Langid=1&PNid=13&PFid=5&Level=5&Conn=4&DownTypeID=3&GetDown=false).

## Long Story

After upgrading several machines from Windows 7 / 8.1 to Windows 10, I noticed that it was no longer possible to wake them over the network using the Wake on LAN magic packet.

### Fast Start

One reason is that Windows 10 (as 8.1) due to its fast start feature shuts down the computer in the [System Power State S4](https://msdn.microsoft.com/en-us/library/windows/hardware/ff564575%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396) instead of S5. In S4 all connected devices are powered off including the NIC. Therefore, the system is unable to receive the WOL Magic Packet for wakeup. When starting up from S4, on the other hand, the system can load a hibernation image from disk which eliminates the need to initialize hardware and dramatically cuts startup speed.

At a later time, the device manufactors may update their drivers and/or BIOS/UEFI firmware to allow WOL also from shut down state S4. For now, however, one has to disable shutting down to S4 to be able to use WOL.

To do this, right click *Start*, go to *Energy Options* and then select *Select what the power button does*. Elevate and then disable *Fast Start*.

### Realtek

A second issue is that the Realtek device driver which is currently installed from Windows Update seems to have a bug which avoids proper configuration of the machine to wake up after shutdown; wakeup from Sleep works, however.

The device driver available from the Realtek website seems to have this bug fixed. You can find it [here](http://www.realtek.com.tw/downloads/downloadsView.aspx?Langid=1&PNid=13&PFid=5&Level=5&Conn=4&DownTypeID=3&GetDown=false).

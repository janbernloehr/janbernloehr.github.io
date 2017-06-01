---
layout: post
title : "RasPi Stream WebCam"
date : 06.01.2016 00:51:00
tags: []
---
{% include JB / setup %}

Logitech C615

# Check wecam formats
avprobe -f video4linux2 -list_formats all /dev/video0

# Build mjpg_streamer
https://wiki.ubuntuusers.de/MJPG-Streamer

# Note that one has to patch for MJPG support !!!

LD_LIBRARY_PATH=/home/pi/mjpg-streamer/ /home/pi/mjpg-streamer/mjpg_streamer -i "input_uvc.so -r 320x240 -f 30 -n" -o "output_http.so -p 8071 -w /home/pi/mjpg-streamer/www"

# Web
http://192.168.178.22:8071/index.html

Without index.html it did not work on my end...

# Write Systemd Script to autostart
http://unix.stackexchange.com/questions/206439/how-to-make-a-systemd-service-run-forever-bootup-is-not-yet-finished-please-t

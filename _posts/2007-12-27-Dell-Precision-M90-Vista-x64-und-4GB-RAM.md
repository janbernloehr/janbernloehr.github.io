---
layout: post
title : "Dell Precision M90, Vista x64 und 4GB RAM"
date : 27.12.2007 02:04:24
tags: [dell, vista, x64]
---
{% include JB/setup %}

Da die Speicherpreise gerade so niedrig wie nie sind, habe mich noch kurz vor Weihnachten dazu entschlossen, meinem **Dell Precision M90** 4GB RAM zu spendieren. Die 4GB Problematik bei 32-bit Betriebssystem war mir bekannt, aber da ich ja stolzer Besitzer von Vista x64 bin, dachte ich, man könnte das einfach ignorieren.

Falsch gedacht, denn das M90 hat einen **Intel i945 PM Chipsatz**, der leider nur einen **32-bit Adressraum** hat. Der Chipsatz kann also <u>nicht mehr</u> als 4GB addressieren. Da noch 512 MB für die Grafikkarte und ein paar MB für sonstige Optionen wie BIOS caching etc. reserviert sind, bleiben 3326MB für die Speicheradressierung **auch bei Vista 64-bit**.

Es bleiben bei mir also ca. 700mb ungenutzt, das ist aber immernoch besser als nur 2gb.

[![image](http://www.vb-magazin.de/janm/blog/images/DellPrecisionM90Vistax64und4GBRAM_1C23/image_thumb.png)](http://www.vb-magazin.de/janm/blog/images/DellPrecisionM90Vistax64und4GBRAM_1C23/image.png)

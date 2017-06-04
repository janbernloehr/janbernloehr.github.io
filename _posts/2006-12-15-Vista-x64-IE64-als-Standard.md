---
layout: post
title : "Vista x64: IE64 als Standard"
date : 15.12.2006 22:14:26
tags: [vista, x64]
---
{% include JB/setup %}

In Vista x64 ist der Standardbrowser die 32-bit Version des Internet Explorers. Um dies zu ändern, kann man folgenden Registrypfad auf den Pfad der 64-Bit Version ändern:

`HKEY_CLASSES_ROOT\IE.HTTP\shell\open\command`

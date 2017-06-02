---
layout: post
title : "WPF Threadsicherheit"
date : 21.11.2006 15:57:21
tags: [.net, wpf, threading]
---
{% include JB/setup %}

Überrascht stelle ich fest, dass in der WPF Final das PropertyChanged Event threadsicher ist.

Das bedeutet, dass das Event von jedem Thread aus ausgelöst werden darf ohne es vorher auf den Ui Thread zu synchronisieren.

Leider gilt das für die CollectionChanged Events nicht.

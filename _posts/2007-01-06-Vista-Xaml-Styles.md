---
layout: post
title : "Vista Xaml Styles"
date : 06.01.2007 19:12:22
tags: [WPF, Vista]
---
{% include JB / setup %}

Da WPF eine völlig eigenständige Renderengine ist, greift sie nicht auf die GDI Funktionen von Windows zu, sondern zeichnet alles selbst.  
Dies bedeutet auch, dass alle Windows Styles also Vistas Aero, XPs Luna und der klassische Windows Style in WPF nachgebaut werden mussten.

Die WPF Samples des Windows SDKs enthalten diese Styles im Xaml Format.  
In folgenden Verzeichnissen findet man die Styles:

| Name    | Pfad                               |
| ------- | ---------------------------------- |
| Aero    | \WPFSamples\Core\AeroTheme\XAML    |
| Luna    | \WPFSamples\Core\LunaTheme\XAML    |
| Royale  | \WPFSamples\Core\RoyaleTheme\XAML  |
| Classic | \WPFSamples\Core\ClassicTheme\XAML |

Viel Spaß damit!

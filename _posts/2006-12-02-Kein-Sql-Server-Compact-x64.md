---
layout: post
title : "Kein Sql Server Compact x64"
date : 02.12.2006 00:09:04
tags: [.net, vista, sql-server, sql-compact]
---
{% include JB/setup %}

Der Sql Server Compact wird nicht als x64 Version zur Verfügung stehen. Dies erfordert eine kleine Anpassung an .NET Projekten die den Sql Server Compact benutzen und eventuell auf x64 Systemen ausgeführt werden.

Die Assembly des Sql Server Compact ist explizit als x86 (32-Bit) Assembly markiert. Versucht eine unveränderte .NET 2.0 Anwendung diese Assembly auf einem x64 Windows zu laden, wird eine BadImageFormatException ausgelöst.

Um dies zu verhindern, kann man die Architektur der .NET Anwendung unter den Build Einstellungen explizit auf x86 stellen. Das .NET Framework läd die Anwendung jetzt im WOW Kompatibilitätsmodus also als 32-Bit Anwendung. Der Sql Server Compact ist nun wieder einsetzbar.

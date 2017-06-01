---
layout: post
title : "WCF HTTP Hosting - ohne Admin Rechte"
date : 15.06.2008 13:49:36
tags: []
---
{% include JB / setup %}

WCF bietet von Haus aus die Möglichkeit einen Service in einem Prozess anstatt im IIS zu hosten. Dadurch ist das Hosting auch wesentlich flexibler, denn zum einen erfordert dann ein HTTP Hosting nicht zwingend die Installation des IIS auf dem Server, zum anderen stehen auch andere Protokolle wie TCP/IP und NetPipes zur Vefügung, die dem IIS fehlen.

Der einzige Haken daran ist, dass ein Prozess, der ohne Admin Rechte läuft, nicht die Rechte hat um einen Service via HTTP zu hosten.   
Versucht man z.B. einen Windows Service ohne diese Rechte zu starten, erhält man die Fehlermeldung:

*Service cannot be started. System.ServiceModel.AddressAccessDeniedException: HTTP could not register URL *[*http://+:7270/[ServiceName]/*](http://+:7270/[ServiceName]/)*. Your process does not have access rights to this namespace.*

Es gibt aber einen Weg, den Wcf Service auch ohne Admin Rechte zu hosten. Unter Windows Vista / Windows Server 2008 steht dafür das Tool **<u>netsh</u>** zur Verfügung. Dadurch ist es möglich, einem Benutzer oder einer Gruppe Rechte auf einen HTTP Namespace, also z.b. "[http://+:7270/ServiceName](http://+:7270/ServiceName)" zu geben.

Die Syntax dafür lautet

netsh http add urlacl url=http://+:80/MyUri user=DOMAIN\user

Für Windows XP oder Windows Server 2003 übernimmt das Tool **<u>httpcfg</u>** diese Aufgabe.

Weitere Informationen liefert der Artikel [Configuring Namespace Reservations](http://msdn.microsoft.com/en-us/library/ms733768.aspx) ([http://msdn.microsoft.com/en-us/library/ms733768.aspx](http://msdn.microsoft.com/en-us/library/ms733768.aspx))

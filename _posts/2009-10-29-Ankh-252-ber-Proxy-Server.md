---
layout: post
title : "Ankh &#252;ber Proxy Server"
date : 29.10.2009 11:28:24
tags: []
---
{% include JB / setup %}

Surft man über einen Proxy Server und will auf seine SVN Repositories über Ankh zugreifen, genügen die globalen Einstellungen des InternetExplorers nicht.

Dennoch verfügt Ankh über die Möglichkeit einen Proxy Server zu verwenden. Dazu muss die folgende Datei editiert werden: 

**%APPDATA%\Subversion\servers**

Dazu müssen die Elemente http-proxy-host usw. auskommentiert werden.

http-proxy-host = your-proxy.com   
http-proxy-port = 80

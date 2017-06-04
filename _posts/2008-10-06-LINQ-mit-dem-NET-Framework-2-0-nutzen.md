---
layout: post
title : "LINQ mit dem .NET Framework 2.0 nutzen"
date : 06.10.2008 23:34:11
tags: [.net, linq, visual-studio]
---
{% include JB/setup %}

LINQ ist ein neues Feature von C# 3.0 und VB 9, das mit dem .NET Framework 3.5 Einzug erhält.

Klar, dass .NET Anwendungen, die LINQ verwenden, dann auch ein installiertes .NET Framework 3.5 erfordern? Nicht ganz!

Schaut man sich .NET 3.5 genauer an, stellt man fest, dass die <u>CLR sich nicht geändert</u> hat, also die Laufzeitumgebung immernoch die des <u>Frameworks 2.0</u> ist. Auch der C# 3.0 bzw. VB9 Compiler generiert MSIL, die <u>100% kompatibel zu .NET 2.0</u> ist.

Und genau dadurch wird es möglich, Anwendungen, die lokale Linq Queries (also Linq-to Objects) einsetzten, auch auf dem .NET Framework 2.0 auszuführen. Möglich macht dies das **Multi Targeting** feature von Visual Studio 2008 und eine **nur 60kb große** Assembly [LinqBridge](http://www.albahari.com/nutshell/linqbridge.aspx), die vom [LinqPad](http://www.linqpad.net/) Author <u>zur freien Verfügung</u> gestellt wird.

Wie das genau funktioniert, wird in diesem Artikel beschrieben:  
[http://www.albahari.com/nutshell/linqbridge.aspx](http://www.albahari.com/nutshell/linqbridge.aspx)

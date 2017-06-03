---
layout: post
title : "ASP.NET Team Todo: DataPager"
date : 18.08.2007 11:33:23
tags: [.NET, ASP .NET]
---
{% include JB/setup %}

Neu in .NET 3.5 ist der DataPager. Wie sein Name schon erraten lässt, erweitert dieses Control das ListView um die Paging Funktionalität.

Die momentane Implementierung des DataPagers hat jedoch (noch) einige Macken.

Es ist beispielsweise nicht möglich, den aktuellen PageIndex im Code festzulegen, ein Szenario bei dem der PageIndex aus dem QueryString übergeben werden soll ist somit nicht umsetzbar.

Aus meiner Sicht noch viel gravierender ist, dass der DataPager nicht auf dem Datenbankserver paged, sondern alle Daten herunterläd und nur eine Auswahl anzeigt. Sollen viele Datensätze gepaged werden, kann dies zu einem Performance Problem werden.

Außerdem wäre es wünschenswert, wenn der DataPager außer dem ListView auch noch den DataRepeater und das GridView erweitern könnte.

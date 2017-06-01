---
layout: post
title : "Benutzereingaben pr&#252;fen: Dateinamen"
date : 18.08.2007 11:14:57
tags: [.NET]
---
{% include JB / setup %}

In vielen Anwendungen haben Benutzer die Möglichkeit, Dateinamen selbst zu vergeben. Dabei muss der Anwendungsentwickler darauf achten, dass keine "illegalen" Zeichen im Dateinamen enthalten sind (Für Windows wären das / \ : * ? " < > und |).

Dies lässt sich mit einem einfachen Regular Expression verhindern.

 <div class="wlWriterSmartContent" id="F2210F5F-69EB-4d4c-AFF7-B8A050E9CC72:b07f9e39-5300-4229-a9c0-86f7b2d9f6eb" contenteditable="false" style="padding-right: 0px; display: inline; padding-left: 0px; float: none; padding-bottom: 0px; margin: 0px; padding-top: 0px">

<div><span style="color: #000000;">filename </span><span style="color: #000000;">=</span><span style="color: #000000;"> System.Text.RegularExpressions.Regex.Replace(filename, </span><span style="color: #800000;">"</span><span style="color: #800000;">[/\\:\*\?""<>\|]</span><span style="color: #800000;">"</span><span style="color: #000000;">, </span><span style="color: #800000;">""</span><span style="color: #000000;">)</span></div>
</div>

So werden einfach alle illegalen Zeichen aus dem Dateinamen entfernt.

Achtung: Dies nur auf den Dateinamen (beispiel.txt), nicht auf den ganzen Pfad (C:\Dokumente\beispiel.txt) anwenden!

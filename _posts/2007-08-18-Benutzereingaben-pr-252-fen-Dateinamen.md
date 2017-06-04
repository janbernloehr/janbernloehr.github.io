---
layout: post
title : "Benutzereingaben pr&#252;fen: Dateinamen"
date : 18.08.2007 11:14:57
tags: [.net, regex]
---
{% include JB/setup %}

In vielen Anwendungen haben Benutzer die Möglichkeit, Dateinamen selbst zu vergeben. Dabei muss der Anwendungsentwickler darauf achten, dass keine "illegalen" Zeichen im Dateinamen enthalten sind (Für Windows wären das / \ : * ? " < > und |).

Dies lässt sich mit einem einfachen Regular Expression verhindern.

``` vb
filename = System.Text.RegularExpressions.Regex.Replace(filename, "[/\\:\*\?""<>\|]", "")
```

So werden einfach alle illegalen Zeichen aus dem Dateinamen entfernt.

Achtung: Dies nur auf den Dateinamen (beispiel.txt), nicht auf den ganzen Pfad (C:\Dokumente\beispiel.txt) anwenden!

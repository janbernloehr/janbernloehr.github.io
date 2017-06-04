---
layout: post
title : "ASP .NET und LINQ ein (fast) perfektes Team!"
date : 15.08.2006 04:17:00
tags: [.net, asp.net, linq]
---
{% include JB/setup %}

*Dieser Blog Beitrag soll kurz beschreiben, wie man LINQ in ASP .NET Anwendungen einsetzen kann. *

Seit längerem beobachte ich das [LINQ Projekt](http://msdn.microsoft.com/data/ref/linq/) von Microsoft, das eine SQL-ähnliche Abfrage Syntax direkt in Visual Basic integriert. 

Beispiel:

``` vb
    Dim data = From x In Northwind.Customers _ 

    Where x.CustomerName.Startswith("A") _ 

    Select x.CustomerId
```

Dazu kommen noch sehr viele und interessante Neuerungen in [Visual Basic 9](http://msdn.microsoft.com/vbasic/future/). 

Bis vor kurzem wurde LINQ nur in Consolen- und Windowsanwendungen demonstriert. Vor einigen Wochen gab Microsoft Research jedoch ein neues Tool namens [B-LINQ](http://www.asp.net/sandbox/app_blinq.aspx?tabid=62) frei, welches eine ASP .NET Anwendung generiert, die automatisch Insert, Update, Delte und View Seiten für alle Tabellen in der Datenbank bereitstellt. Jedoch gab es Probleme, wenn man diese Projekte in Visual Studio 2005 öffnete. 

Nach einigem suchen und probieren fand ich jedoch eine [gute Möglichkeit](http://forums.microsoft.com/MSDN/ShowPost.aspx?PostID=296346&SiteID=1) LINQ auch in ASP .NET Anwendungen zu testen. Dazu muss zunächst die Web Application Erweiterung für Visual Studio installiert werden, die es ermöglicht, wieder Visual Studio 2003 ähnliche Web Applications mit CodeBehind anstatt den „neuen“ Web Sites mit CodeBeside zu erstellen. 

Zuletzt wollte ich meine Testanwendung auf einem WebServer testen. Da LINQ eigentlich konformes .NET 2.0 ist, das nur von einem anderen Compiler erstellt wurde, und „normale“ Windows- und Consolenanwendungen auch problemslos funktionieren, musste es auch eine Möglichkeit für ASP .NET geben. 

Das Problem bei ASP .NET Anwendungen ist jedoch, dass die Seiten bei jedem Aufruf teilweise neu übersetzt werden. Da ich aber wie gesagt den LINQ Compiler nicht installieren konnte, musste das verhindert werden.Dies kann man mit dem Tool aspnet_compiler erreichen, welches sich im .NET Framework Verzeichnis befindet. Dieses Tool kann die WebSeite so vorkompilieren, dass sämtlicher Inhalt (auch die .aspx Seiten) in die Assembly intergiert werden. Somit muss die WebSeite beim Aufruf nicht neu erstellt werden und die LINQ Kommandos funktionieren problemlos. 

```
aspnet_compiler -p PhysicalProjectLocation -v / ProjectPublishDirectory
```

Hinweis: Für LINQ gibt es keine Go-Live Lizenz, weshalb es nicht möglich ist, LINQ in komerziellen Projekten einzusetzen.

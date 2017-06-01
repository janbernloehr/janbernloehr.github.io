---
layout: post
title : "ASP .NET und LINQ ein (fast) perfektes Team!"
date : 15.08.2006 04:17:00
tags: [.NET, ASP .NET, LINQ]
---
{% include JB / setup %}

*Dieser Blog Beitrag soll kurz beschreiben, wie man LINQ in ASP .NET Anwendungen einsetzen kann. *

Seit längerem beobachte ich das [LINQ Projekt](http://msdn.microsoft.com/data/ref/linq/) von Microsoft, das eine SQL-ähnliche Abfrage Syntax direkt in Visual Basic integriert. 

Beispiel:
 <div class="wlWriterSmartContent" id="57F11A72-B0E5-49c7-9094-E3A15BD5B5E7:56e3440a-7d15-4e21-ac72-18007c302c61" contenteditable="false" style="padding-right: 0px; display: inline; padding-left: 0px; float: none; padding-bottom: 0px; margin: 0px; padding-top: 0px">

<div><span style="color: #0000FF; ">Dim</span><span style="color: #000000; "> data </span><span style="color: #000000; ">=</span><span style="color: #000000; "> From x </span><span style="color: #0000FF; ">In</span><span style="color: #000000; "> Northwind.Customers _ 

    Where x.CustomerName.Startswith(</span><span style="color: #000000; ">"</span><span style="color: #000000; ">A</span><span style="color: #000000; ">"</span><span style="color: #000000; ">) _ 

    </span><span style="color: #0000FF; ">Select</span><span style="color: #000000; "> x.CustomerId </span></div>
</div>

Dazu kommen noch sehr viele und interessante Neuerungen in [Visual Basic 9](http://msdn.microsoft.com/vbasic/future/). 

Bis vor kurzem wurde LINQ nur in Consolen- und Windowsanwendungen demonstriert. Vor einigen Wochen gab Microsoft Research jedoch ein neues Tool namens [B-LINQ](http://www.asp.net/sandbox/app_blinq.aspx?tabid=62) frei, welches eine ASP .NET Anwendung generiert, die automatisch Insert, Update, Delte und View Seiten für alle Tabellen in der Datenbank bereitstellt. Jedoch gab es Probleme, wenn man diese Projekte in Visual Studio 2005 öffnete. 

Nach einigem suchen und probieren fand ich jedoch eine [gute Möglichkeit](http://forums.microsoft.com/MSDN/ShowPost.aspx?PostID=296346&SiteID=1) LINQ auch in ASP .NET Anwendungen zu testen. Dazu muss zunächst die Web Application Erweiterung für Visual Studio installiert werden, die es ermöglicht, wieder Visual Studio 2003 ähnliche Web Applications mit CodeBehind anstatt den „neuen“ Web Sites mit CodeBeside zu erstellen. 

Zuletzt wollte ich meine Testanwendung auf einem WebServer testen. Da LINQ eigentlich konformes .NET 2.0 ist, das nur von einem anderen Compiler erstellt wurde, und „normale“ Windows- und Consolenanwendungen auch problemslos funktionieren, musste es auch eine Möglichkeit für ASP .NET geben. 

Das Problem bei ASP .NET Anwendungen ist jedoch, dass die Seiten bei jedem Aufruf teilweise neu übersetzt werden. Da ich aber wie gesagt den LINQ Compiler nicht installieren konnte, musste das verhindert werden.Dies kann man mit dem Tool aspnet_compiler erreichen, welches sich im .NET Framework Verzeichnis befindet. Dieses Tool kann die WebSeite so vorkompilieren, dass sämtlicher Inhalt (auch die .aspx Seiten) in die Assembly intergiert werden. Somit muss die WebSeite beim Aufruf nicht neu erstellt werden und die LINQ Kommandos funktionieren problemlos. 

<div class="wlWriterSmartContent" id="57F11A72-B0E5-49c7-9094-E3A15BD5B5E7:6e76b70b-7555-4a5d-bc3e-892154861042" contenteditable="false" style="padding-right: 0px; display: inline; padding-left: 0px; float: none; padding-bottom: 0px; margin: 0px; padding-top: 0px">

<div><span style="color: #000000; ">aspnet_compiler </span><span style="color: #000000; ">-</span><span style="color: #000000; ">p PhysicalProjectLocation </span><span style="color: #000000; ">-</span><span style="color: #000000; ">v </span><span style="color: #000000; ">/</span><span style="color: #000000; "> ProjectPublishDirectory </span></div>
</div><span style="font-size: 10px; font-family: courier new"><span style="color: black"></span></span>

<span style="color: red">Hinweis: Für LINQ gibt es keine Go-Live Lizenz, weshalb es nicht möglich ist, LINQ in komerziellen Projekten einzusetzen.</span>

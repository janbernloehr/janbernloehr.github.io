---
layout: post
title : "AjaxMin – JavaScript und CSS Dateien automatisch schrumpfen"
date : 25.05.2010 14:18:10
tags: []
---
{% include JB/setup %}

Während der Entwicklung von Web-Anwendungen ist man meist über gut dokumentierte JavaScripts und CSS Stylesheets, die viel Whitespace zur Strukturierung enthalten, dankbar. Geht es jedoch ans Deployment, möchte man diese Dateien möglichst klein halten, um den Seitenaufbau im Browser zu beschleunigen. Im Web findet man zahlreiche Tools sogenannte “Minifier”, die die Dateien verkleinern, indem Kommentare und Whitespace entfernt werden. In großen Projekten mit vielen Skripts und Styles ist dieser Vorgang jedoch sehr mühsam und nach jeder Änderung müssen die Dateien neu “minifiziert” werden.

Abhilfe schaft “**AjaxMin**” eine Visual Studio Erweiterung von Microsoft, die diese Arbeit automatisch durchführt. AjaxMin definiert einen Build-Taks, der sich in jedes Projekt einbinden lässt und bei jedem Compile-Vorgang automatisch alle Skripte und Style minifiziert und mit der Endung “min.js” bzw. “min.cs” neben die bestehenden Dateien legt. Damit reduziert sich der Aufwand für die Minifizierung auf Null :)

[Übersicht über AjaxMin](http://www.asp.net/ajaxlibrary/AjaxMinQuickStart.ashx)

[Download von AjaxMin](http://aspnet.codeplex.com/releases/view/40584)

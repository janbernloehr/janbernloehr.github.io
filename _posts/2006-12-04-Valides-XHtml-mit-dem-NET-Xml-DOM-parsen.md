---
layout: post
title : "Valides XHtml mit dem .NET Xml DOM parsen"
date : 04.12.2006 23:08:14
tags: [asp.net]
---
{% include JB/setup %}

Die HTML Ausgabe von Webseiten zu parsen stellt meist eine sehr unschöne Aufgabe dar. Seelig sind die, deren Webseite wenigstens den Xhtml Standard erfüllt also ein valides Xml Dokument ausgibt. Dadurch wird es möglich das Dokument in jeder .NET Anwendung als Xml Dokument zu laden und dann mit effizienten Methoden wie Xpath nach Informationen zu durchforsten.

Ob es sich um ein valides XHTML Dokument handlelt, kann man beispielsweise mit dem Markup Validation Service des W3C überprüfen ([http://validator.w3.org/](http://validator.w3.org/ "http://validator.w3.org/")).

Ein Grund mehr also in eigenen ASP .NET Anwendungen valides XHTML auszugeben, wenn man die Informationen auf der Seite vollständig genutzt sehen möchte.

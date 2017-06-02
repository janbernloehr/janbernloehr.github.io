---
layout: post
title : "WPF lokalisieren"
date : 03.01.2007 23:36:05
tags: [.net, wpf, localization]
---
{% include JB/setup %}

.NET setzt beim Programmstart die CultureInfo automatisch auf die systemweit Eingestellte.

WPF Elemente lassen sich jedoch von dieser Einstellung nicht beeindrucken.  
![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/WPFlokalisieren_14BDC/image04.png) 

Das wird vor allem dann zum Problem, wenn man z.B. Datums-Eingaben in Textfeldern parst ...

Die Lösung des Problems ist jedoch sehr einfach, indem man im Xaml Element das xml:lang Attribut setzt.

    <Window x:Class="Window1" xml:lang="de-de" xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" >

![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/WPFlokalisieren_14BDC/image09.png) 

Bisher habe ich jedoch noch nicht herausgefunden, wie man diesen Vorgang automatisiert, sodass sich die CultureInfo der im System eingestellten anpasst ...

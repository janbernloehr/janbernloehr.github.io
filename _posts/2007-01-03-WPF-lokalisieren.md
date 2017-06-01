---
layout: post
title : "WPF lokalisieren"
date : 03.01.2007 23:36:05
tags: [.NET, WPF]
---
{% include JB / setup %}

.NET setzt beim Programmstart die CultureInfo automatisch auf die systemweit Eingestellte.

WPF Elemente lassen sich jedoch von dieser Einstellung nicht beeindrucken.  
![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/WPFlokalisieren_14BDC/image04.png) 

Das wird vor allem dann zum Problem, wenn man z.B. Datums-Eingaben in Textfeldern parst ...

Die Lösung des Problems ist jedoch sehr einfach, indem man im Xaml Element das xml:lang Attribut setzt.

 <div class="wlWriterSmartContent" id="57F11A72-B0E5-49c7-9094-E3A15BD5B5E7:9fcac49c-4f45-47e5-bdcb-abb2a515997b" contenteditable="false" style="padding-right: 0px; display: inline; padding-left: 0px; float: none; padding-bottom: 0px; margin: 0px; padding-top: 0px"> <div><span style="color: #0000FF; "><</span><span style="color: #800000; ">Window </span><span style="color: #FF0000; ">x:Class</span><span style="color: #0000FF; ">="Window1"</span><span style="color: #FF0000; "> xml:lang</span><span style="color: #0000FF; ">="de-de"</span><span style="color: #FF0000; "> xmlns</span><span style="color: #0000FF; ">="http://schemas.microsoft.com/winfx/2006/xaml/presentation"</span><span style="color: #FF0000; "> xmlns:x</span><span style="color: #0000FF; ">="http://schemas.microsoft.com/winfx/2006/xaml"</span><span style="color: #FF0000; "> </span><span style="color: #0000FF; ">></span></div> </div>

![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/WPFlokalisieren_14BDC/image09.png) 

Bisher habe ich jedoch noch nicht herausgefunden, wie man diesen Vorgang automatisiert, sodass sich die CultureInfo der im System eingestellten anpasst ...

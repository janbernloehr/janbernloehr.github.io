---
layout: post
title : "Visual Basic: Endlich richtig Casten"
date : 06.03.2008 13:17:49
tags: []
---
{% include JB/setup %}

In diesem Artikel werden VB Funktionen wie CType, DirectCast, CInt, CStr, ... betrachtet.

Ist **Option Strict** aktiviert, fordert der Visual Basic Compiler, dass man beim Zugriff auf Funktionen und Eigenschaften eines Objekts den Datentyp angibt.
 <div style="padding-right: 0px; padding-left: 0px; float: none; padding-bottom: 0px; margin: 0px; padding-top: 0px; display: inline" id="scid:F2210F5F-69EB-4d4c-AFF7-B8A050E9CC72:9894d533-7962-408d-b956-59d34da6e3d0" class="wlWriterSmartContent">Beispiel 1

<div><span style="color: #999999;">1</span> <span style="color: #0000FF;">Dim</span><span style="color: #000000;"> person1 </span><span style="color: #0000FF;">As</span><span style="color: #000000;"> Person 
    </span><span style="color: #999999;">2</span> <span style="color: #000000;">person1 </span><span style="color: #000000;">=</span><span style="color: #000000;"> </span><span style="color: #0000FF;">New</span><span style="color: #000000;"> Person
    </span><span style="color: #999999;">3</span> <span style="color: #000000;">Console.WriteLine(person1.Name) </span><span style="color: #008000;">'</span><span style="color: #008000;"> funktioniert </span><span style="color: #008000;">
    </span><span style="color: #999999;">4</span> <span style="color: #008000;"></span><span style="color: #0000FF;">Dim</span><span style="color: #000000;"> person2 </span><span style="color: #0000FF;">As</span><span style="color: #000000;"> </span><span style="color: #0000FF;">Object</span><span style="color: #000000;"> 
    </span><span style="color: #999999;">5</span> <span style="color: #000000;">person2 </span><span style="color: #000000;">=</span><span style="color: #000000;"> </span><span style="color: #0000FF;">New</span><span style="color: #000000;"> Person
    </span><span style="color: #999999;">6</span> <span style="color: #000000;">Console.WriteLine(person2.Name) </span><span style="color: #008000;">'</span><span style="color: #008000;"> Compilerfehler, da Name keine Eigenschaft von Object ist.</span></div>
</div>

In diesem Beispiel müsste man zuerst das Objekt person2 in den Typ Person casten.

### Was ist ein Cast?

Bei einem Cast eines Objekts o in den Typ T, teilt man dem Compiler mit, dass er das Objekt o nun als eine Instanz des Typs T betrachten soll.

<div style="padding-right: 0px; padding-left: 0px; float: none; padding-bottom: 0px; margin: 0px; padding-top: 0px; display: inline" id="scid:F2210F5F-69EB-4d4c-AFF7-B8A050E9CC72:bb322baa-da84-44a7-a677-b2a2b2304a0c" class="wlWriterSmartContent">Beispiel 2

<div><span style="color: #999999;">1</span> <span style="color: #0000FF;">Dim</span><span style="color: #000000;"> o </span><span style="color: #0000FF;">As</span><span style="color: #000000;"> </span><span style="color: #0000FF;">Object</span><span style="color: #000000;"> 
    </span><span style="color: #999999;">2</span> <span style="color: #000000;">o </span><span style="color: #000000;">=</span><span style="color: #000000;"> </span><span style="color: #0000FF;">New</span><span style="color: #000000;"> Person 
    </span><span style="color: #999999;">3</span> <span style="color: #000000;"></span><span style="color: #008000;">'</span><span style="color: #008000;"> o ist vom Typ Person, also kann man dies dem Compiler mitteilen </span><span style="color: #008000;">
    </span><span style="color: #999999;">4</span> <span style="color: #008000;"></span><span style="color: #0000FF;">Dim</span><span style="color: #000000;"> p </span><span style="color: #0000FF;">As</span><span style="color: #000000;"> Person 
    </span><span style="color: #999999;">5</span> <span style="color: #000000;">p </span><span style="color: #000000;">=</span><span style="color: #000000;"> </span><span style="color: #0000FF;">DirectCast</span><span style="color: #000000;">(o, Person)</span></div>
</div>

Hier wurde dem Compiler mitgeteilt, das Objekt o als Instanz des Typs Person zu betrachten. Nun kann man auf die Eigenschaften und Funktionen, die Person bereitstellt, zugreifen. 

Beim Casten bleiben die zugrundeliegenden Daten unverändert, man ändert nur die "Sichtweise". 

VBs DirectCast ist ein "pures" Casting, es findet also keine Umwandlung statt, und damit ist es equivalent mit dem C# Cast. 

<div style="padding-right: 0px; padding-left: 0px; float: none; padding-bottom: 0px; margin: 0px; padding-top: 0px; display: inline" id="scid:F2210F5F-69EB-4d4c-AFF7-B8A050E9CC72:d1867096-0785-41e7-8bee-e9ea832363ab" class="wlWriterSmartContent">VB

<div><span style="color: #0000FF;">Dim</span><span style="color: #000000;"> p </span><span style="color: #0000FF;">As</span><span style="color: #000000;"> Person </span><span style="color: #000000;">=</span><span style="color: #000000;"> </span><span style="color: #0000FF;">DirectCast</span><span style="color: #000000;">(o, Person)</span></div>
</div>
<div style="padding-right: 0px; padding-left: 0px; float: none; padding-bottom: 0px; margin: 0px; padding-top: 0px; display: inline" id="scid:F2210F5F-69EB-4d4c-AFF7-B8A050E9CC72:e5813cdc-c50f-4981-8de3-a3e86e2d73d9" class="wlWriterSmartContent">C#

<div><span style="color: #999999;">1</span> <span style="color: #000000;">Person p </span><span style="color: #000000;">=</span><span style="color: #000000;"> (Person)o;</span></div>
</div>

### Was ist eine Conversion?

Unter VB steht neben DirectCast noch das Schlüsselwort CType zur Verfügung. Dieses Schlüsselwort kann neben "purem" Casting auch noch Conversions also Umwandlungen durchführen.

<div style="padding-right: 0px; padding-left: 0px; float: none; padding-bottom: 0px; margin: 0px; padding-top: 0px; display: inline" id="scid:F2210F5F-69EB-4d4c-AFF7-B8A050E9CC72:91f546a4-5aad-49cb-bfed-959619e74864" class="wlWriterSmartContent">Beispiel 3

<div><span style="color: #999999;">1</span> <span style="color: #0000FF;">Dim</span><span style="color: #000000;"> o </span><span style="color: #0000FF;">As</span><span style="color: #000000;"> </span><span style="color: #0000FF;">Object</span><span style="color: #000000;"> 
    </span><span style="color: #999999;">2</span> <span style="color: #000000;">o </span><span style="color: #000000;">=</span><span style="color: #000000;"> </span><span style="color: #800000;">"</span><span style="color: #800000;">12</span><span style="color: #800000;">"</span><span style="color: #000000;"> 
    </span><span style="color: #999999;">3</span> <span style="color: #000000;"></span><span style="color: #0000FF;">Dim</span><span style="color: #000000;"> i </span><span style="color: #0000FF;">As</span><span style="color: #000000;"> </span><span style="color: #0000FF;">Integer</span><span style="color: #000000;"> 
    </span><span style="color: #999999;">4</span> <span style="color: #000000;">i </span><span style="color: #000000;">=</span><span style="color: #000000;"> </span><span style="color: #0000FF;">CType</span><span style="color: #000000;">(o, </span><span style="color: #0000FF;">Integer</span><span style="color: #000000;">)</span></div>
</div>

In diesem Beispiel wird der Variable o der Wert "12" zugewiesen, somit ist sie vom Typ String. Eine Umwandlung mit DirectCast zum Typ Integer würde einen Fehler erzeugen, da der zugrundeliegende Datentyp nicht Integer sonder String ist und somit noch eine Umwandlung erforderlich wäre. 

CType wandelt also die zugrundeliegenden Daten (hier vom Typ String in Integer). 

Alternativ zu CType stellt Visual Basic noch spezielle Konvertierungsschlüsselwörter wie CStr, CInt, CDbl, CDate, etc. (Im Folgenden C* genannt) zur Verfügung, die für die Umwandlung in die entsprechenden Datentypen zur Verfügung stehen. Diese sind kürzer als die CType Notation und sparen somit Tipparbeit. 

<div style="padding-right: 0px; padding-left: 0px; float: none; padding-bottom: 0px; margin: 0px; padding-top: 0px; display: inline" id="scid:F2210F5F-69EB-4d4c-AFF7-B8A050E9CC72:3fafc08b-3e1b-4f02-a93f-c38d49610454" class="wlWriterSmartContent">Beispiel 4

<div><span style="color: #999999;">1</span> <span style="color: #0000FF;">Dim</span><span style="color: #000000;"> o </span><span style="color: #0000FF;">As</span><span style="color: #000000;"> </span><span style="color: #0000FF;">Object</span><span style="color: #000000;"> 
    </span><span style="color: #999999;">2</span> <span style="color: #000000;">o </span><span style="color: #000000;">=</span><span style="color: #000000;"> </span><span style="color: #800000;">"</span><span style="color: #800000;">12</span><span style="color: #800000;">"</span><span style="color: #000000;"> 
    </span><span style="color: #999999;">3</span> <span style="color: #000000;"></span><span style="color: #0000FF;">Dim</span><span style="color: #000000;"> i </span><span style="color: #0000FF;">As</span><span style="color: #000000;"> </span><span style="color: #0000FF;">Integer</span><span style="color: #000000;"> 
    </span><span style="color: #999999;">4</span> <span style="color: #000000;">i </span><span style="color: #000000;">=</span><span style="color: #000000;"> </span><span style="color: #0000FF;">CInt</span><span style="color: #000000;">(o)</span></div>
</div>

Beim Umwandeln von primitiven Datentypen wie Strings, Integers, etc. werden sowohl CType als auch C* vom Compiler in Aufrufe von Microsoft.VisualBasic.Conversion umgewandelt.

[![image](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualBasicEndlichrichtigCasten_CF01/image_thumb.png)](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualBasicEndlichrichtigCasten_CF01/image.png) 

Ein Aufruf von CType(o, String) bzw. CStr wird vom Compiler in Microsoft.VisualBasic.Conversions.Str(o) umgewandelt.

Neben den VB Spezifischen Umwandlungen stellt das .NET Framework mit der Convert Klasse eine für alle .NET Sprachen einheitliche Möglichkeit zur Verfügung Daten umzuwandeln.

<div style="padding-right: 0px; padding-left: 0px; float: none; padding-bottom: 0px; margin: 0px; padding-top: 0px; display: inline" id="scid:F2210F5F-69EB-4d4c-AFF7-B8A050E9CC72:256a09f4-48f8-437a-b477-424c811d6b68" class="wlWriterSmartContent">Beispiel 4

<div><span style="color: #999999;">1</span> <span style="color: #0000FF;">Dim</span><span style="color: #000000;"> o </span><span style="color: #0000FF;">As</span><span style="color: #000000;"> </span><span style="color: #0000FF;">Object</span><span style="color: #000000;"> 
    </span><span style="color: #999999;">2</span> <span style="color: #000000;">o </span><span style="color: #000000;">=</span><span style="color: #000000;"> </span><span style="color: #800000;">"</span><span style="color: #800000;">12</span><span style="color: #800000;">"</span><span style="color: #000000;"> 
    </span><span style="color: #999999;">3</span> <span style="color: #000000;"></span><span style="color: #0000FF;">Dim</span><span style="color: #000000;"> i </span><span style="color: #0000FF;">As</span><span style="color: #000000;"> </span><span style="color: #0000FF;">Integer</span><span style="color: #000000;"> 
    </span><span style="color: #999999;">4</span> <span style="color: #000000;">i </span><span style="color: #000000;">=</span><span style="color: #000000;"> Convert.ToInt32(o)</span></div>
</div>

### Performancebetrachtung

Die Visual Studio Intellisense empfiehlt stets eine Umwandlung mittels CType oder C* ist dann DirectCast vollkommen unnötig?

Hier gilt es zwei Fälle zu unterschieden:

1.  Typen, die nicht IConvertible implementieren (idR. Refrenztypen: Controls, Forms, selbst definierte Klassen, Interfaces, etc.)  
In Beispiel 1 wurde eine Variable vom Typ Object, die eine Instanz des Person Typs enthält, gecastet. Da es sich hier um einen Referenz handelt und keine Konvertierung möglich ist, wird CType vom Compiler als DirectCast übersetzt. Es macht hier also keinen Unterschied CType oder DirectCast zu nehmen. 
Typen, die IConvertible implementieren (idR. Datentypen: Integer, Double, Date, ... aber auch String)  
Hier macht es einen deutlichen Unterschied ob man CType, C*, DirectCast oder Convert verwendet.  
Um Aussage etwas mit Zahlen zu unterlegen habe ich einen kleinen Benchmark geschrieben, der die Dauer in Millisekunden angibt, die es auf meinem System benötigt hat die gewünschte Operation 100.000.000 mal durchzuführen.  

[![image](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualBasicEndlichrichtigCasten_CF01/image_thumb_3.png)](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualBasicEndlichrichtigCasten_CF01/image_3.png) 

Für mich ergibt sich daraus

*   Wann immer möglich DirectCast verwenden, da es bis zu 10 mal schneller ist als CType. 
Ist eine Umwandlung des zugrundeliegenden Werts notwendig, dann zeigen die Methoden der .NET Convert Klasse deutlich bessere Performance als die Conversion Methoden von Visual Basic, weshalb man CInt(o), etc. durch Convert.ToInt32(o), etc. ersetzten sollte.  
Ausnahme bilden hier CStr und CDate, die nicht in VisualBasic.Conversion vertreten sind, sondern direkt Methoden des Frameworks aufrufen. 
Stellen die Datentypen also eigene Umwandlungsmethoden zur Verfügung wie Integer.Parse bzw. Object.ToString, dann sind diese zu empfehlen.

### Ergebnis

Dieser Artikel hat gezeigt, dass die Empfehlung der Visual Studio Intellisense bequem ist und Schreibarbeit spart, jedoch auch Performanceeinbußen mit sich bringt. Der tatsächliche Verlust ist zwar von Anwendung zu Anwendung verschieden aber in einigen Fallen macht er sich sogar mit Faktor 10 bemerkbar.

Schade ist, dass die Implementierung der Visual Basic Conversion doch deutlich langsamer ist als die des .NET Frameworks. Grund dafür ist wahrscheinlich die Abwärtskompatibilität zu VB6 ...

Ich werde mich in Zukunft an DirectCast und Convert halten, was auch den Vorteil hat, dass C#-ler meinen Code leichter lesen können.

**Source Code**

Den verwendeten Source Code habe ich in diesem Beispielprojekt zusammengefasst.

[http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualBasicEndlichrichtigCasten_CF01/ConversionPerformance.zip](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualBasicEndlichrichtigCasten_CF01/ConversionPerformance.zip "http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualBasicEndlichrichtigCasten_CF01/ConversionPerformance.zip")

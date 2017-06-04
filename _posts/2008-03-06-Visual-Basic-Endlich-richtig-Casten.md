---
layout: post
title : "Visual Basic: Endlich richtig Casten"
date : 06.03.2008 13:17:49
tags: [vb]
---
{% include JB/setup %}

In diesem Artikel werden VB Funktionen wie CType, DirectCast, CInt, CStr, ... betrachtet.

Ist **Option Strict** aktiviert, fordert der Visual Basic Compiler, dass man beim Zugriff auf Funktionen und Eigenschaften eines Objekts den Datentyp angibt.

Beispiel 1

``` vb
Dim person1 As Person 
person1 = New Person
Console.WriteLine(person1.Name) ' funktioniert 
Dim person2 As Object 
person2 = New Person
Console.WriteLine(person2.Name) ' Compilerfehler, da Name keine Eigenschaft von Object ist.
```

In diesem Beispiel müsste man zuerst die Instanz `person2` in den Typ `Person` casten.

### Was ist ein Cast?

Bei einem Cast einer Instanz `o` in den Typ `T`, teilt man dem Compiler mit, dass er `o` nun als eine Instanz des Typs `T` betrachten soll.

Beispiel 2

``` vb
Dim o As Object
o = New Person ' o ist vom Typ Person, also kann man dies dem Compiler mitteilen 
Dim p As Person 
p = DirectCast(o, Person)
```

Hier wurde dem Compiler mitgeteilt, das Objekt `o` als Instanz des Typs `Person` zu betrachten. Nun kann man auf die Eigenschaften und Funktionen, die Person bereitstellt, zugreifen. 

Beim Casten bleiben die zugrundeliegenden Daten unverändert, man ändert nur die "Sichtweise". 

VBs `DirectCast` ist ein "pures" Casting, es findet also keine Umwandlung statt, und damit ist es equivalent mit dem C# Cast. 

``` vb
Dim p As Person = DirectCast(o, Person)

Person p = (Person)o;
```

### Was ist eine Conversion?

Unter VB steht neben `DirectCast` noch das Schlüsselwort `CType` zur Verfügung. Dieses Schlüsselwort kann neben "purem" Casting auch noch Conversions also Umwandlungen durchführen.

Beispiel 3

``` vb
Dim o As Object 
o = "12" 
Dim i As Integer 
i = CType(o, Integer)
```

In diesem Beispiel wird der Variable `o` der Wert "12" zugewiesen, somit ist sie vom Typ `String`. Eine Umwandlung mit DirectCast zum Typ Integer würde einen Fehler erzeugen, da der zugrundeliegende Datentyp nicht Integer sonder String ist und somit noch eine Umwandlung erforderlich wäre. 

CType wandelt also die zugrundeliegenden Daten (hier vom Typ String in Integer). 

Alternativ zu CType stellt Visual Basic noch spezielle Konvertierungsschlüsselwörter wie CStr, CInt, CDbl, CDate, etc. (Im Folgenden C* genannt) zur Verfügung, die für die Umwandlung in die entsprechenden Datentypen zur Verfügung stehen. Diese sind kürzer als die CType Notation und sparen somit Tipparbeit. 

Beispiel 4

``` vb
Dim o As Object 
o = "12" 
Dim i As Integer 
i = CInt(o)
```

Beim Umwandeln von primitiven Datentypen wie Strings, Integers, etc. werden sowohl CType als auch C* vom Compiler in Aufrufe von Microsoft.VisualBasic.Conversion umgewandelt.

[![image](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualBasicEndlichrichtigCasten_CF01/image_thumb.png)](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualBasicEndlichrichtigCasten_CF01/image.png) 

Ein Aufruf von CType(o, String) bzw. CStr wird vom Compiler in Microsoft.VisualBasic.Conversions.Str(o) umgewandelt.

Neben den VB Spezifischen Umwandlungen stellt das .NET Framework mit der Convert Klasse eine für alle .NET Sprachen einheitliche Möglichkeit zur Verfügung Daten umzuwandeln.

Beispiel 5

``` vb
Dim o As Object 
o = "12" 
Dim i As Integer 
i = Convert.ToInt32(o)
```

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

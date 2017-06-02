---
layout: post
title : "Vista: Einen Prozess mit Admin Rechten starten"
date : 28.10.2006 12:04:37
tags: [.net, vista, uac]
---
{% include JB/setup %}

In Vista sorgt die UAC (User Account Protection) dafür, dass Prozesse standardmäßig **<u>keine</u>** Administrator Rechte haben, selbst wenn man als Administrator angemeldet ist.

Jedoch gibt es mehrere Möglichkeiten, einen Prozess doch mit Admin Rechten zu starten.

### 1. Manuell

Ein Rechtsklick auf die Anwendung öffnet das Context-Menü mit dem Eintrag ![](http://vb-magazin.de/janm/blog/images/VistaEinenProzessmitAdminRechtenstarten_A15F/image013.png) .

Wählt man dieser Option wird man als Administrator um Zustimmung gebeten oder als Standard Benutzer um ein Admin Passwort:

![](http://vb-magazin.de/janm/blog/images/VistaEinenProzessmitAdminRechtenstarten_A15F/cred14.jpg) 

### 2. Manifestdatei

Um das starten mit Admin Rechten zu erzwingen, kann man der Anwendung ein Manifest hinzufügen, das Informationen über die benötigten Rechte enthält.

Die Manifestdatei muss sich im Verzeichnis der Anwendung befinden und den Namen Anwendungsname.exe.manifest tragen. Für notepad.exe wäre das Manifest demnach notepad.exe.manifest.

**Inhalt der Manifestdatei**

````xml
<textarea><?xml version="1.0" encoding="UTF-8" standalone="yes"?> <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0"> <dependency> <dependentAssembly> <assemblyIdentity type="win32" name="Microsoft.Windows.Common-Controls" version="6.0.0.0" processorArchitecture="*" publicKeyToken="6595b64144ccf1df" language="*" /> </dependentAssembly> </dependency> <v3:trustInfo xmlns:v3="urn:schemas-microsoft-com:asm.v3"> <v3:security> <v3:requestedPrivileges> <!-- level can be "asInvoker", "highestAvailable", or "requireAdministrator" --> <v3:requestedExecutionLevel level="highestAvailable" /> </v3:requestedPrivileges> </v3:security> </v3:trustInfo> </assembly> </textarea>
````

Beim Ausführen wird ein Standardbenutzer nun aufgefordert, die Anwendung als Administrator zu starten.

**Hinweis:** Das Manifest wird von früheren Windows Versionen ignoriert.

Entwickelt man eine Anwendung mit Visual Studio, kann man das Manifest dem Projekt hinzufügen und als *Buildaktion* für die Datei "Inhalt" und "Immer kopieren" wählen. Beim Kompilieren wird dann die Manifestdatei automatisch in das Ausgabeverzeichnis kopiert. Dadurch wird garantiert, dass die Anwendung auf Vista nur mit Admin Rechten gestartet werden kann.

### 3. Zur Laufzeit mit System.Diagnostics

Häufig muss eine Anwendung, die über kein Manifest verfügt, jedoch zur Laufzeit mit Admin Rechten gestartet werden.

Ein Ausführen von System.Diagnostics.Process.Start(fileName, arguments) führt auf den ersten Blick nicht ans Ziel.  
Mit einem kleinen Kniff kann man dies jedoch auch mit den .NET Bordmitteln erreichen:

````vb
Private Function StartProcessAsAdmin(ByVal fileName As String, ByVal arguments As String) As System.Diagnostics.Process
    Dim pi As System.Diagnostics.ProcessStartInfo pi = New System.Diagnostics.ProcessStartInfo(fileName, arguments)
    
    If System.Environment.OSVersion.Version.Major >= 6 Then 
        pi.Verb = "runas" 
    End If 
    
    Return System.Diagnostics.Process.Start(pi) 
End Function
````

Ein Aufruf von StartProcessAsAdmin(fileName, arguments) sorgt nun dafür, dass der Prozess auch in Vista mit Administrator Rechten gestartet wird.

Das *Verb* **runas** hat den gleichen Effekt wie ein Klick auf ![](http://vb-magazin.de/janm/blog/images/VistaEinenProzessmitAdminRechtenstarten_A15F/image013.png).

Auf Windows Versionen vor Vista wird jedoch der Dialog "Ausführen als ..." angezeigt, weshalb die Version auf größergleich 6.x also Vista geprüft wird.

### Zusammenfassung

UAC ist ein Ansatz, der den Benutzer schützen soll. Überraschenderweise bedeutet es für die Entwickler keinen erheblichen Mehraufwand, kompatible Anwendungen zu entwickeln.

.NET unterstützt einen dabei mit Bordmitteln und mit wenigen Zeilen, ist die eigene Anwendung zukunftssicher, falls sie überhaupt Administrator Rechte benötigt.

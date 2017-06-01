---
layout: post
title : "Audio Wave Resource abspielen"
date : 12.01.2007 15:32:26
tags: [.NET]
---
{% include JB / setup %}

Viele Anwendungen benutzen Sounds um den Benutzer über bestimmte Ereignisse zu informieren. Jedoch will man die Audio Dateien nicht "lose" im Dateisystem herumliegen lassen, sondern mit der Anwendung bündeln. Dazu bietet sich das Resourcen System von Visual Basic an.

Hier eine Schritt für Schritt Anleitung zum Abspielen von Wave Dateien aus Resourcen:

1. Wave Datei mittels *Projekt\Vorhandenes Element hinzufügen* in das Projekt einfügen  
2. Das Item im Projekt Explorer auswählen und in den Eigenschaften als Build-Aktion *Eingebettete Resource* einstellen  
3. Folgende Klasse bietet die Funktionalität zum Abspielen

````vb
Public Class Winmm
    Public Const SND_ASYNC As UInt32 = 1
    Public Const SND_MEMORY As UInt32 = 4 

    Declare Auto Function PlaySound Lib "Winmm.dll" (ByVal rsc As IntPtr, ByVal hMod As IntPtr, ByVal dwFlags As UInt32) As Boolean 

    Declare Auto Function PlaySound Lib "Winmm.dll" (ByVal Sound As String, ByVal hMod As IntPtr, ByVal dwFlags As UInt32) As Boolean 

    Declare Auto Function PlaySound Lib "Winmm.dll" (ByVal data As Byte(), ByVal hMod As IntPtr, ByVal dwFlags As UInt32) As Boolean 

    Public Shared Sub PlayWavResource(ByVal wav As String)
        Dim namesp As String
        Dim stream As System.IO.Stream 
    
        namesp = System.Reflection.Assembly.GetExecutingAssembly.GetName.Name
        stream = System.Reflection.Assembly.GetExecutingAssembly.GetManifestResourceStream(namesp & "." & wav) 
    
        If stream Is Nothing Then Return 
    
        Dim data(CInt(stream.Length - 1)) As Byte 
    
        stream.Read(data, 0, CInt(stream.Length)) 
    
        PlaySound(data, IntPtr.Zero, SND_ASYNC Or SND_ASYNC)
    End Sub
End Class
````

4. Die Resource MeineTolleWaveDatei.wav lässt sich so abspielen

````vb
Winmm.PlayWavResource("MeineTolleWaveDatei.wav")
````

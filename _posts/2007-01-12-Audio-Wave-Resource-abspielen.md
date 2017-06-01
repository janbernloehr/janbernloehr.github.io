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
 <div class="wlWriterSmartContent" id="57F11A72-B0E5-49c7-9094-E3A15BD5B5E7:ab0e1900-7cfa-498f-9746-5bddb7888ebc" contenteditable="false" style="padding-right: 0px; display: inline; padding-left: 0px; float: none; padding-bottom: 0px; margin: 0px; padding-top: 0px">

<div><span style="color: #0000FF; ">Public</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Class</span><span style="color: #000000; "> Winmm
    </span><span style="color: #0000FF; ">Public</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Const</span><span style="color: #000000; "> SND_ASYNC </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> UInt32 </span><span style="color: #000000; ">=</span><span style="color: #000000; "> </span><span style="color: #000000; ">1</span><span style="color: #000000; ">
    </span><span style="color: #0000FF; ">Public</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Const</span><span style="color: #000000; "> SND_MEMORY </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> UInt32 </span><span style="color: #000000; ">=</span><span style="color: #000000; "> </span><span style="color: #000000; ">4</span><span style="color: #000000; "> 

    </span><span style="color: #0000FF; ">Declare</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Auto</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Function</span><span style="color: #000000; "> PlaySound </span><span style="color: #0000FF; ">Lib</span><span style="color: #000000; "> </span><span style="color: #000000; ">"</span><span style="color: #000000; ">Winmm.dll</span><span style="color: #000000; ">"</span><span style="color: #000000; "> (</span><span style="color: #0000FF; ">ByVal</span><span style="color: #000000; "> rsc </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> IntPtr, </span><span style="color: #0000FF; ">ByVal</span><span style="color: #000000; "> hMod </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> IntPtr, </span><span style="color: #0000FF; ">ByVal</span><span style="color: #000000; "> dwFlags </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> UInt32) </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Boolean</span><span style="color: #000000; "> 

    </span><span style="color: #0000FF; ">Declare</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Auto</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Function</span><span style="color: #000000; "> PlaySound </span><span style="color: #0000FF; ">Lib</span><span style="color: #000000; "> </span><span style="color: #000000; ">"</span><span style="color: #000000; ">Winmm.dll</span><span style="color: #000000; ">"</span><span style="color: #000000; "> (</span><span style="color: #0000FF; ">ByVal</span><span style="color: #000000; "> Sound </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">String</span><span style="color: #000000; ">, </span><span style="color: #0000FF; ">ByVal</span><span style="color: #000000; "> hMod </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> IntPtr, </span><span style="color: #0000FF; ">ByVal</span><span style="color: #000000; "> dwFlags </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> UInt32) </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Boolean</span><span style="color: #000000; "> 

    </span><span style="color: #0000FF; ">Declare</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Auto</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Function</span><span style="color: #000000; "> PlaySound </span><span style="color: #0000FF; ">Lib</span><span style="color: #000000; "> </span><span style="color: #000000; ">"</span><span style="color: #000000; ">Winmm.dll</span><span style="color: #000000; ">"</span><span style="color: #000000; "> (</span><span style="color: #0000FF; ">ByVal</span><span style="color: #000000; "> data </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Byte</span><span style="color: #000000; ">(), </span><span style="color: #0000FF; ">ByVal</span><span style="color: #000000; "> hMod </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> IntPtr, </span><span style="color: #0000FF; ">ByVal</span><span style="color: #000000; "> dwFlags </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> UInt32) </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Boolean</span><span style="color: #000000; "> 

    </span><span style="color: #0000FF; ">Public</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Shared</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Sub</span><span style="color: #000000; "> PlayWavResource(</span><span style="color: #0000FF; ">ByVal</span><span style="color: #000000; "> wav </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">String</span><span style="color: #000000; ">)
    </span><span style="color: #0000FF; ">Dim</span><span style="color: #000000; "> namesp </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">String</span><span style="color: #000000; ">
    </span><span style="color: #0000FF; ">Dim</span><span style="color: #000000; "> stream </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> System.IO.Stream 

    namesp </span><span style="color: #000000; ">=</span><span style="color: #000000; "> System.Reflection.Assembly.GetExecutingAssembly.GetName.Name
    stream </span><span style="color: #000000; ">=</span><span style="color: #000000; "> System.Reflection.Assembly.GetExecutingAssembly.GetManifestResourceStream(namesp </span><span style="color: #000000; ">&</span><span style="color: #000000; "> </span><span style="color: #000000; ">"</span><span style="color: #000000; ">.</span><span style="color: #000000; ">"</span><span style="color: #000000; "> </span><span style="color: #000000; ">&</span><span style="color: #000000; "> wav) 

    </span><span style="color: #0000FF; ">If</span><span style="color: #000000; "> stream </span><span style="color: #0000FF; ">Is</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Nothing</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Then</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Return</span><span style="color: #000000; "> 

    </span><span style="color: #0000FF; ">Dim</span><span style="color: #000000; "> data(</span><span style="color: #0000FF; ">CInt</span><span style="color: #000000; ">(stream.Length </span><span style="color: #000000; ">-</span><span style="color: #000000; "> </span><span style="color: #000000; ">1</span><span style="color: #000000; ">)) </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Byte</span><span style="color: #000000; "> 

    stream.Read(data, </span><span style="color: #000000; ">0</span><span style="color: #000000; ">, </span><span style="color: #0000FF; ">CInt</span><span style="color: #000000; ">(stream.Length)) 

    PlaySound(data, IntPtr.Zero, SND_ASYNC </span><span style="color: #0000FF; ">Or</span><span style="color: #000000; "> SND_ASYNC)
    </span><span style="color: #0000FF; ">End Sub</span><span style="color: #000000; ">
    </span><span style="color: #0000FF; ">End Class</span><span style="color: #000000; "> 

    </span></div>
</div>

4. Die Resource MeineTolleWaveDatei.wav lässt sich so abspielen

<div class="wlWriterSmartContent" id="57F11A72-B0E5-49c7-9094-E3A15BD5B5E7:df181d56-9199-4451-9c90-025fbd2d6535" contenteditable="false" style="padding-right: 0px; display: inline; padding-left: 0px; float: none; padding-bottom: 0px; margin: 0px; padding-top: 0px">

<div><span style="color: #000000; ">Winmm.PlayWavResource(</span><span style="color: #000000; ">"</span><span style="color: #000000; ">MeineTolleWaveDatei.wav</span><span style="color: #000000; ">"</span><span style="color: #000000; ">)</span></div>
</div>

---
layout: post
title : "DirectSound in der eigenen Anwendung nutzen"
date : 16.01.2007 17:05:56
tags: [.NET]
---
{% include JB / setup %}

Microsoft bietet seit .NET 2.0 im Framework Methoden um Sounds wiederzugeben. Jedoch sind diese sehr unflexibel und stoßen schnell an ihre Grenzen, wenn es z.B. darum geht mehrere Sounds gleichzeitig wiederzugeben. Außerdem wird die vorhandene Hardware garnicht genutzt sondern immer alles im Softwaremodus wiedergegeben.

Abhilfe schafft hier DirectSound, für das auch ein .NET Layer namens MDX verfügbar ist. MDX wird automatisch bei der Installation des [DirectX SDK](http://www.microsoft.com/downloads/Browse.aspx?DisplayLang=en&nr=20&categoryid=2&sortCriteria=date)s installiert.

Nach der Installation sind viele neue Assemblies verfügbar, wir benötigen lediglich **Microsoft.DirectX.DirectSound**.  
![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/DirectSoundindereigenenAnwendungnutzen_F069/image04.png) 

Ist diese Referenz hinzugefügt ist man nur noch wenige Schritte von einem hörbaren Resultat entfernt.

Zunächst muss ein **Device** erstellt und initialisiert werden.
 <div class="wlWriterSmartContent" id="57F11A72-B0E5-49c7-9094-E3A15BD5B5E7:277f0b11-baa6-4643-81fb-5f43d9213149" contenteditable="false" style="padding-right: 0px; display: inline; padding-left: 0px; float: none; padding-bottom: 0px; margin: 0px; padding-top: 0px">

<div><span style="color: #0000FF; ">Dim</span><span style="color: #000000; "> dev </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> Microsoft.DirectX.DirectSound.Device

    dev </span><span style="color: #000000; ">=</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">New</span><span style="color: #000000; "> Microsoft.DirectX.DirectSound.Device
    dev.SetCooperativeLevel(</span><span style="color: #0000FF; ">Me</span><span style="color: #000000; ">, CooperativeLevel.Priority)</span></div>
</div>

Mit **SetCooperativeLevel** muss ein Bezug auf eine sichtbare Windows Form gesetzt werden.  
<u>Das Device sollte nur einmal zu Programmstart erstellt und initialisiert werden.</u>

Mit dem initialisierten Device kann man nun einen **SecondaryBuffer** erstellen, in den der Sound geladen wird.

<div class="wlWriterSmartContent" id="57F11A72-B0E5-49c7-9094-E3A15BD5B5E7:678a7bd9-ff17-4063-9e49-19e64b873a38" contenteditable="false" style="padding-right: 0px; display: inline; padding-left: 0px; float: none; padding-bottom: 0px; margin: 0px; padding-top: 0px">

<div><span style="color: #0000FF; ">Dim</span><span style="color: #000000; "> buff </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> SecondaryBuffer

    buff </span><span style="color: #000000; ">=</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">New</span><span style="color: #000000; "> SecondaryBuffer(</span><span style="color: #000000; ">"</span><span style="color: #000000; ">D:\Windows Exclamation.wav</span><span style="color: #000000; ">"</span><span style="color: #000000; ">, dev)</span></div>
</div>

Nun kann der Sound mit der **Play** Methode abgespielt werden.

 <div class="wlWriterSmartContent" id="57F11A72-B0E5-49c7-9094-E3A15BD5B5E7:196a863e-9152-4150-8993-45623ad5baad" contenteditable="false" style="padding-right: 0px; display: inline; padding-left: 0px; float: none; padding-bottom: 0px; margin: 0px; padding-top: 0px"> <div><span style="color: #000000; ">buff.Play(</span><span style="color: #000000; ">0</span><span style="color: #000000; ">, BufferPlayFlags.Default)</span></div> </div>

Analog dazu funktioniert das Abspielen eines Sounds aus einem Stream, wie das z.B. bei eingebetteten Resourcen der Fall ist. Anstatt des Pfads wird lediglich der Stream angegeben.

 <div class="wlWriterSmartContent" id="57F11A72-B0E5-49c7-9094-E3A15BD5B5E7:19a6df7f-263e-437d-b75c-2e1fd3094297" contenteditable="false" style="padding-right: 0px; display: inline; padding-left: 0px; float: none; padding-bottom: 0px; margin: 0px; padding-top: 0px"> <div><span style="color: #0000FF; ">Dim</span><span style="color: #000000; "> buff </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> SecondaryBuffer buff </span><span style="color: #000000; ">=</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">New</span><span style="color: #000000; "> SecondaryBuffer(stream, _SoundDevice) buff.Play(</span><span style="color: #000000; ">0</span><span style="color: #000000; ">, BufferPlayFlags.Default)</span></div> </div>

**Zusammenfassung**

DirectSound bietet eine einfache und kompfortable Möglichkeit Sounds abzuspielen. Dazu kann man die Methoden beliebig durch 3D wiedergabe oder Hardwarebeschleunigung erweitern. Auch das Abspielen von Sounds aus Streams ist problemlos möglich, was das weitergeben der Sounds als Resource sehr erleichtert.

Das einzige "Manko" ist, dass auf dem Zielrechner eine Installation von DirectX 9.0c und der hier benutzten MDX version vorausgesetzt wird. Die benötigten Setups hierfür befinden sich im Installationsverzeichnis des SDKs unter \Redist.

Source Code:

<div class="wlWriterSmartContent" id="57F11A72-B0E5-49c7-9094-E3A15BD5B5E7:9c474464-ae36-4ff9-b073-0f639f9f5f01" contenteditable="false" style="padding-right: 0px; display: inline; padding-left: 0px; float: none; padding-bottom: 0px; margin: 0px; padding-top: 0px">

<div><span style="color: #008000; ">'</span><span style="color: #008000; "> Initialisierung ...</span><span style="color: #008000; ">
    </span><span style="color: #0000FF; ">Dim</span><span style="color: #000000; "> player </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> SoundPlayer
    player </span><span style="color: #000000; ">=</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">New</span><span style="color: #000000; "> SoundPlayer(</span><span style="color: #0000FF; ">Me</span><span style="color: #000000; ">)

    </span><span style="color: #008000; ">'</span><span style="color: #008000; "> Abspielen</span><span style="color: #008000; ">
    </span><span style="color: #000000; ">player.PlaySoundFromEmbeddedResource(</span><span style="color: #000000; ">"</span><span style="color: #000000; ">Windows Exclamation.wav</span><span style="color: #000000; ">"</span><span style="color: #000000; ">)</span></div>
</div>
<div class="wlWriterSmartContent" id="57F11A72-B0E5-49c7-9094-E3A15BD5B5E7:461e8fcb-94a9-4845-a63e-db4e3af85eb7" contenteditable="false" style="padding-right: 0px; display: inline; padding-left: 0px; float: none; padding-bottom: 0px; margin: 0px; padding-top: 0px">

<div><span style="color: #000000; ">
    </span><span style="color: #0000FF; ">Public</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Class</span><span style="color: #000000; "> SoundPlayer
        </span><span style="color: #0000FF; ">Dim</span><span style="color: #000000; "> dev </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> Microsoft.DirectX.DirectSound.Device

        </span><span style="color: #0000FF; ">Public</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Sub</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">New</span><span style="color: #000000; ">(</span><span style="color: #0000FF; ">ByVal</span><span style="color: #000000; "> owner </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> Form)
            dev </span><span style="color: #000000; ">=</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">New</span><span style="color: #000000; "> Microsoft.DirectX.DirectSound.Device
            dev.SetCooperativeLevel(owner, CooperativeLevel.Priority)
        </span><span style="color: #0000FF; ">End Sub</span><span style="color: #000000; ">

        </span><span style="color: #0000FF; ">Public</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Sub</span><span style="color: #000000; "> PlaySoundFromFile(</span><span style="color: #0000FF; ">ByVal</span><span style="color: #000000; "> fileName </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">String</span><span style="color: #000000; ">)
            </span><span style="color: #0000FF; ">Dim</span><span style="color: #000000; "> buff </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> SecondaryBuffer

            buff </span><span style="color: #000000; ">=</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">New</span><span style="color: #000000; "> SecondaryBuffer(fileName, dev)
            buff.Play(</span><span style="color: #000000; ">0</span><span style="color: #000000; ">, BufferPlayFlags.Default)
        </span><span style="color: #0000FF; ">End Sub</span><span style="color: #000000; ">

        </span><span style="color: #0000FF; ">Public</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Sub</span><span style="color: #000000; "> PlaySoundFromEmbeddedResource(</span><span style="color: #0000FF; ">ByVal</span><span style="color: #000000; "> fileName </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">String</span><span style="color: #000000; ">)
            </span><span style="color: #0000FF; ">Dim</span><span style="color: #000000; "> namesp </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">String</span><span style="color: #000000; ">
            </span><span style="color: #0000FF; ">Dim</span><span style="color: #000000; "> stream </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> System.IO.Stream

            namesp </span><span style="color: #000000; ">=</span><span style="color: #000000; "> System.Reflection.Assembly.GetExecutingAssembly.GetName.Name
            stream </span><span style="color: #000000; ">=</span><span style="color: #000000; "> System.Reflection.Assembly.GetExecutingAssembly.GetManifestResourceStream(namesp </span><span style="color: #000000; ">&</span><span style="color: #000000; "> </span><span style="color: #000000; ">"</span><span style="color: #000000; ">.</span><span style="color: #000000; ">"</span><span style="color: #000000; "> </span><span style="color: #000000; ">&</span><span style="color: #000000; "> fileName)

            </span><span style="color: #0000FF; ">If</span><span style="color: #000000; "> stream </span><span style="color: #0000FF; ">Is</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Nothing</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Then</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Throw</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">New</span><span style="color: #000000; "> ArgumentException(</span><span style="color: #0000FF; ">String</span><span style="color: #000000; ">.Format(</span><span style="color: #000000; ">"</span><span style="color: #000000; ">Embedded Resource '{0}' wurde nicht gefunden!</span><span style="color: #000000; ">"</span><span style="color: #000000; ">, fileName))

            PlaySoundFromStream(stream)
        </span><span style="color: #0000FF; ">End Sub</span><span style="color: #000000; ">

        </span><span style="color: #0000FF; ">Public</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">Sub</span><span style="color: #000000; "> PlaySoundFromStream(</span><span style="color: #0000FF; ">ByVal</span><span style="color: #000000; "> stream </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> System.IO.Stream)
            </span><span style="color: #0000FF; ">Dim</span><span style="color: #000000; "> buff </span><span style="color: #0000FF; ">As</span><span style="color: #000000; "> SecondaryBuffer

            buff </span><span style="color: #000000; ">=</span><span style="color: #000000; "> </span><span style="color: #0000FF; ">New</span><span style="color: #000000; "> SecondaryBuffer(stream, dev)
            buff.Play(</span><span style="color: #000000; ">0</span><span style="color: #000000; ">, BufferPlayFlags.Default)
        </span><span style="color: #0000FF; ">End Sub</span><span style="color: #000000; ">
    </span><span style="color: #0000FF; ">End Class</span></div>
</div>

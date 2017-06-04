---
layout: post
title : "DirectSound in der eigenen Anwendung nutzen"
date : 16.01.2007 17:05:56
tags: [.net, mdx, directsound]
---
{% include JB/setup %}

Microsoft bietet seit .NET 2.0 im Framework Methoden um Sounds wiederzugeben. Jedoch sind diese sehr unflexibel und stoßen schnell an ihre Grenzen, wenn es z.B. darum geht mehrere Sounds gleichzeitig wiederzugeben. Außerdem wird die vorhandene Hardware garnicht genutzt sondern immer alles im Softwaremodus wiedergegeben.

Abhilfe schafft hier DirectSound, für das auch ein .NET Layer namens MDX verfügbar ist. MDX wird automatisch bei der Installation des [DirectX SDK](http://www.microsoft.com/downloads/Browse.aspx?DisplayLang=en&nr=20&categoryid=2&sortCriteria=date)s installiert.

Nach der Installation sind viele neue Assemblies verfügbar, wir benötigen lediglich **Microsoft.DirectX.DirectSound**.  
![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/DirectSoundindereigenenAnwendungnutzen_F069/image04.png) 

Ist diese Referenz hinzugefügt ist man nur noch wenige Schritte von einem hörbaren Resultat entfernt.

Zunächst muss ein **Device** erstellt und initialisiert werden.
 ``` vb
Dim dev As Microsoft.DirectX.DirectSound.Device

dev = New Microsoft.DirectX.DirectSound.Device
dev.SetCooperativeLevel(Me, CooperativeLevel.Priority)
```

Mit **SetCooperativeLevel** muss ein Bezug auf eine sichtbare Windows Form gesetzt werden.  
<u>Das Device sollte nur einmal zu Programmstart erstellt und initialisiert werden.</u>

Mit dem initialisierten Device kann man nun einen **SecondaryBuffer** erstellen, in den der Sound geladen wird.

``` vb
Dim buff As SecondaryBuffer

buff = New SecondaryBuffer("D:\Windows Exclamation.wav", dev)
```

Nun kann der Sound mit der **Play** Methode abgespielt werden.

``` vb
buff.Play(0, BufferPlayFlags.Default)
```

Analog dazu funktioniert das Abspielen eines Sounds aus einem Stream, wie das z.B. bei eingebetteten Resourcen der Fall ist. Anstatt des Pfads wird lediglich der Stream angegeben.

``` vb
Dim buff As SecondaryBuffer buff = New SecondaryBuffer(stream, _SoundDevice) buff.Play(0, BufferPlayFlags.Default)
```

**Zusammenfassung**

DirectSound bietet eine einfache und kompfortable Möglichkeit Sounds abzuspielen. Dazu kann man die Methoden beliebig durch 3D wiedergabe oder Hardwarebeschleunigung erweitern. Auch das Abspielen von Sounds aus Streams ist problemlos möglich, was das weitergeben der Sounds als Resource sehr erleichtert.

Das einzige "Manko" ist, dass auf dem Zielrechner eine Installation von DirectX 9.0c und der hier benutzten MDX version vorausgesetzt wird. Die benötigten Setups hierfür befinden sich im Installationsverzeichnis des SDKs unter \Redist.

Source Code:

``` vb
' Initialisierung ...
Dim player As SoundPlayer
player = New SoundPlayer(Me)

' Abspielen
player.PlaySoundFromEmbeddedResource("Windows Exclamation.wav")
```

``` vb
Public Class SoundPlayer
        Dim dev As Microsoft.DirectX.DirectSound.Device

        Public Sub New(ByVal owner As Form)
            dev = New Microsoft.DirectX.DirectSound.Device
            dev.SetCooperativeLevel(owner, CooperativeLevel.Priority)
        End Sub

        Public Sub PlaySoundFromFile(ByVal fileName As String)
            Dim buff As SecondaryBuffer

            buff = New SecondaryBuffer(fileName, dev)
            buff.Play(0, BufferPlayFlags.Default)
        End Sub

        Public Sub PlaySoundFromEmbeddedResource(ByVal fileName As String)
            Dim namesp As String
            Dim stream As System.IO.Stream

            namesp = System.Reflection.Assembly.GetExecutingAssembly.GetName.Name
            stream = System.Reflection.Assembly.GetExecutingAssembly.GetManifestResourceStream(namesp & "." & fileName)

            If stream Is Nothing Then Throw New ArgumentException(String.Format("Embedded Resource '{0}' wurde nicht gefunden!", fileName))

            PlaySoundFromStream(stream)
        End Sub

        Public Sub PlaySoundFromStream(ByVal stream As System.IO.Stream)
            Dim buff As SecondaryBuffer

            buff = New SecondaryBuffer(stream, dev)
            buff.Play(0, BufferPlayFlags.Default)
        End Sub
    End Class
```

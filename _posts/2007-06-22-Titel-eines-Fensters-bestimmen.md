---
layout: post
title : "Titel eines Fensters bestimmen"
date : 22.06.2007 09:04:37
tags: [.NET]
---
{% include JB / setup %}

Visual Basic .NET Bordmittel ermöglichen es nicht, den Fenstertitel eines anderen Prozesses zu bestimmen. Jedoch ist dies mit der Win32 Api möglich, die Teil des Betriebssystems ist.

Dafür benötigt man zwei Funktionen:

    <span style="color: #0000ff">Declare</span> Auto <span style="color: #0000ff">Function</span> FindWindow <span style="color: #0000ff">Lib</span> "<span style="color: #8b0000">user32</span>" (<span style="color: #0000ff">ByVal</span> lpClassName <span style="color: #0000ff">As</span> <span style="color: #0000ff">String</span>, <span style="color: #0000ff">ByVal</span> lpWindowName <span style="color: #0000ff">As</span> <span style="color: #0000ff">String</span>) <span style="color: #0000ff">As</span> IntPtr

        <span style="color: #0000ff">Declare</span> Auto <span style="color: #0000ff">Function</span> GetWindowText <span style="color: #0000ff">Lib</span> "<span style="color: #8b0000">user32</span>" (<span style="color: #0000ff">ByVal</span> hwnd <span style="color: #0000ff">As</span> IntPtr, <span style="color: #0000ff">ByVal</span> lpString <span style="color: #0000ff">As</span> Text.StringBuilder, <span style="color: #0000ff">ByVal</span> nMaxCount <span style="color: #0000ff">As</span> <span style="color: #0000ff">Integer</span>) <span style="color: #0000ff">As</span> <span style="color: #0000ff">Integer</span>

**FindWindow** gibt das Handle eines Fensters zurück. Für uns ist der Parameter lpClassName entscheidend, dem man die Fensterklasse übergibt. (Z.B. Notepad)

**GetWindowText** schreibt den Titel eines Fensters in einen string-buffer.

### Beispiel: Titel eines Notepad Fensters bestimmen.

Zuerst muss das Handle des Fensters bestimmt werden.

<span style="color: #0000ff">Dim</span> hWnd <span style="color: #0000ff">As</span> IntPtr 
    hWnd = Win32Functions.FindWindow("<span style="color: #8b0000">Notepad</span>", <span style="color: #0000ff">Nothing</span>)

Nun kann man die Länge des Fenstertitels erfragen.

<span style="color: #0000ff">Dim</span> titleLength <span style="color: #0000ff">As</span> <span style="color: #0000ff">Integer</span>
    titleLength = Win32Functions.GetWindowTextLength(hWnd) + 1

Mit Fenster-Handle und Titel-Länge lässt sich nun der Titel abfragen.

<span style="color: #0000ff">Dim</span> title <span style="color: #0000ff">As</span> Text.StringBuilder

    title = <span style="color: #0000ff">New</span> Text.StringBuilder(titleLength)

    Win32Functions.GetWindowText(hWnd, title, titleLength)

    Console.WriteLine(title.ToString())

Der Titel des Fensters ist nun im StringBuilder title gespeichert. 

![image](http://vb-magazin.de/janm/blog/images/TiteleinesFenstersbestimmen_7967/image.png) 

### Source Code

<span style="color: #0000ff">Module</span> <span style="color: #0000ff">Module</span>1
        <span style="color: #0000ff">Sub</span> Main()
            <span style="color: #0000ff">Dim</span> hWnd <span style="color: #0000ff">As</span> IntPtr
            <span style="color: #0000ff">Dim</span> title <span style="color: #0000ff">As</span> <span style="color: #0000ff">String</span>

            ' Handle des Fensters bestimmen
            hWnd = Win32Functions.FindWindow("<span style="color: #8b0000">Notepad</span>", <span style="color: #0000ff">Nothing</span>)

            Console.WriteLine("<span style="color: #8b0000">Handle: {0}</span>", hWnd.ToString)

            <span style="color: #0000ff">If</span> hWnd = IntPtr.Zero <span style="color: #0000ff">Then</span>
                Console.WriteLine("<span style="color: #8b0000">No window found :(</span>")
            <span style="color: #0000ff">Else</span>
                ' Titel des Fensters bestimmen
                title = GetWindowTitle(hWnd)

                Console.WriteLine("<span style="color: #8b0000">Title: {0}</span>", title)
            <span style="color: #0000ff">End</span> <span style="color: #0000ff">If</span>

            Console.ReadLine()
        <span style="color: #0000ff">End</span> <span style="color: #0000ff">Sub</span>

        <span style="color: #0000ff">Private</span> <span style="color: #0000ff">Function</span> GetWindowTitle(<span style="color: #0000ff">ByVal</span> hWnd <span style="color: #0000ff">As</span> IntPtr) <span style="color: #0000ff">As</span> <span style="color: #0000ff">String</span>
            <span style="color: #0000ff">Dim</span> titleLength <span style="color: #0000ff">As</span> <span style="color: #0000ff">Integer</span>
            <span style="color: #0000ff">Dim</span> title <span style="color: #0000ff">As</span> Text.StringBuilder

            ' Länge des Titles bestimmmen (0 = 1, 1 = 2 usw. wichtig für die erstellung des <span style="color: #0000ff">string</span> buffers)
            titleLength = Win32Functions.GetWindowTextLength(hWnd) + 1

            <span style="color: #0000ff">If</span> titleLength = 1 <span style="color: #0000ff">Then</span> <span style="color: #0000ff">Return</span> <span style="color: #0000ff">String</span>.Empty

            ' <span style="color: #0000ff">string</span> buffer erstellen
            title = <span style="color: #0000ff">New</span> Text.StringBuilder(titleLength)

            ' Titel <span style="color: #0000ff">in</span> den buffer schreiben
            Win32Functions.GetWindowText(hWnd, title, titleLength)

            <span style="color: #0000ff">Return</span> title.ToString
        <span style="color: #0000ff">End</span> <span style="color: #0000ff">Function</span>
    <span style="color: #0000ff">End</span> <span style="color: #0000ff">Module</span>

    ' Notwendige Win32 Api Deklarationen (Siehe http:<span style="color: #008000">//www.pinvoke.net)</span>
    <span style="color: #0000ff">Public</span> <span style="color: #0000ff">Class</span> Win32Functions
        <span style="color: #0000ff">Declare</span> Auto <span style="color: #0000ff">Function</span> FindWindow <span style="color: #0000ff">Lib</span> "<span style="color: #8b0000">user32</span>" (<span style="color: #0000ff">ByVal</span> lpClassName <span style="color: #0000ff">As</span> <span style="color: #0000ff">String</span>, <span style="color: #0000ff">ByVal</span> lpWindowName <span style="color: #0000ff">As</span> <span style="color: #0000ff">String</span>) <span style="color: #0000ff">As</span> IntPtr

        <span style="color: #0000ff">Declare</span> Auto <span style="color: #0000ff">Function</span> GetWindowText <span style="color: #0000ff">Lib</span> "<span style="color: #8b0000">user32</span>" (<span style="color: #0000ff">ByVal</span> hwnd <span style="color: #0000ff">As</span> IntPtr, <span style="color: #0000ff">ByVal</span> lpString <span style="color: #0000ff">As</span> Text.StringBuilder, <span style="color: #0000ff">ByVal</span> nMaxCount <span style="color: #0000ff">As</span> <span style="color: #0000ff">Integer</span>) <span style="color: #0000ff">As</span> <span style="color: #0000ff">Integer</span>

        <span style="color: #0000ff">Declare</span> Auto <span style="color: #0000ff">Function</span> GetWindowTextLength <span style="color: #0000ff">Lib</span> "<span style="color: #8b0000">user32</span>" (<span style="color: #0000ff">ByVal</span> hwnd <span style="color: #0000ff">As</span> IntPtr) <span style="color: #0000ff">As</span> <span style="color: #0000ff">Integer</span>
    <span style="color: #0000ff">End</span> <span style="color: #0000ff">Class</span>

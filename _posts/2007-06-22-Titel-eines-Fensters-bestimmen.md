---
layout: post
title : "Titel eines Fensters bestimmen"
date : 22.06.2007 09:04:37
tags: [.net, win32, interop]
---
{% include JB/setup %}

Visual Basic .NET Bordmittel ermöglichen es nicht, den Fenstertitel eines anderen Prozesses zu bestimmen. Jedoch ist dies mit der Win32 Api möglich, die Teil des Betriebssystems ist.

Dafür benötigt man zwei Funktionen:

``` vb
Declare Auto Function FindWindow Lib "user32" (ByVal lpClassName As String, ByVal lpWindowName As String) As IntPtr

Declare Auto Function GetWindowText Lib "user32" (ByVal hwnd As IntPtr, ByVal lpString As Text.StringBuilder, ByVal nMaxCount As Integer) As Integer
```

**FindWindow** gibt das Handle eines Fensters zurück. Für uns ist der Parameter lpClassName entscheidend, dem man die Fensterklasse übergibt. (Z.B. Notepad)

**GetWindowText** schreibt den Titel eines Fensters in einen string-buffer.

### Beispiel: Titel eines Notepad Fensters bestimmen.

Zuerst muss das Handle des Fensters bestimmt werden.

``` vb
Dim hWnd As IntPtr 
hWnd = Win32Functions.FindWindow("Notepad", Nothing)
```

Nun kann man die Länge des Fenstertitels erfragen.

``` vb
Dim titleLength As Integer
titleLength = Win32Functions.GetWindowTextLength(hWnd) + 1
```

Mit Fenster-Handle und Titel-Länge lässt sich nun der Titel abfragen.

``` vb
Dim title As Text.StringBuilder

title = New Text.StringBuilder(titleLength)

Win32Functions.GetWindowText(hWnd, title, titleLength)

Console.WriteLine(title.ToString())
```

Der Titel des Fensters ist nun im StringBuilder title gespeichert. 

![image](/assets/images/TiteleinesFenstersbestimmen_7967/image.png) 

### Source Code

``` vb
Module Module1
    Sub Main()
        Dim hWnd As IntPtr
        Dim title As String
        ' Handle des Fensters bestimmen
        hWnd = Win32Functions.FindWindow("Notepad", Nothing)
        Console.WriteLine("Handle: {0}", hWnd.ToString)
        If hWnd = IntPtr.Zero Then
            Console.WriteLine("No window found :(")
        Else
            ' Titel des Fensters bestimmen
            title = GetWindowTitle(hWnd)
            Console.WriteLine("Title: {0}", title)
        End If
        Console.ReadLine()
    End Sub

    Private Function GetWindowTitle(ByVal hWnd As IntPtr) As String
        Dim titleLength As Integer
        Dim title As Text.StringBuilder
        ' Länge des Titles bestimmmen (0 = 1, 1 = 2 usw. wichtig für die erstellung des string buffers)
        titleLength = Win32Functions.GetWindowTextLength(hWnd) + 1
        If titleLength = 1 Then Return String.Empty
        ' string buffer erstellen
        title = New Text.StringBuilder(titleLength)
        ' Titel in den buffer schreiben
        Win32Functions.GetWindowText(hWnd, title, titleLength)
        Return title.ToString
    End Function
End Module

' Notwendige Win32 Api Deklarationen (Siehe http://www.pinvoke.net)
Public Class Win32Functions
    Declare Auto Function FindWindow Lib "user32" (ByVal lpClassName As String, ByVal lpWindowName As String) As IntPtr
    Declare Auto Function GetWindowText Lib "user32" (ByVal hwnd As IntPtr, ByVal lpString As Text.StringBuilder, ByVal nMaxCount As Integer) As Integer
    Declare Auto Function GetWindowTextLength Lib "user32" (ByVal hwnd As IntPtr) As Integer
End Class
```
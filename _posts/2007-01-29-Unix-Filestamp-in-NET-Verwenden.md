---
layout: post
title : "Unix Filestamp in .NET Verwenden"
date : 29.01.2007 23:20:43
tags: [.net, unix, interop]
---
{% include JB/setup %}

Hin und wieder muss man sich doch auch mal mit den anderen unterhalten ;) Dabei kann es vorkommen, dass ein Datum nicht wie gewohnt in der komfortablen **DateTime** Version vorkommt, sondern als Unix Filestamp in einem Byte Array.

Aber für .NET ist die Konvertierung ein Kinderspiel:

````vb
''' <summary>
''' Converts a byte array representing a UNIX FileStamp to a datetime value.  
''' </summary>  
Private Function UnixFileStampToDateTime(ByVal bytes As Byte()) As DateTime
    Dim lngSeconds As UInt32 
    lngSeconds = BitConverter.ToUInt32(bytes, 0)
    Return (New System.DateTime(1970, 1, 1, 0, 0, 0)).AddSeconds(lngSeconds)
End Function

''' <summary>  
''' Converts a datetime value to a byte array representing a UNIX FileStamp.  
''' </summary>  
Private Function DateTimeToFileStamp(ByVal [date] As Date) As Byte() 
    Dim lngSeconds As Long 
    lngSeconds = CLng([date].Subtract(New System.DateTime(1970, 1, 1, 0, 0, 0)).TotalSeconds)
    Return BitConverter.GetBytes((CUInt(lngSeconds))) 
End Function
````

Hinweis: Bei Litte-Endian muss das byte Array vorher noch umgekehrt (**Array.Reverse**) werden.

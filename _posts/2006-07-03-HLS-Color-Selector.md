---
layout: post
title : "HLS Color Selector"
date : 03.07.2006 22:11:00
tags: [.net, wpf, hls]
---
{% include JB/setup %}

I searched the web for a while to find a WPF HLS Color selector like Interactive Designer is equipped with. 

![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/070406_0011_HLSColorS1.png) 

Since I didn’t find anything I thought about creating my own one. 

The most difficult task was to convert .NET Rgb Color Values to HLS values (the difference is explained [here](http://en.wikipedia.org/wiki/HLS_color_space)). In my opinion the HLS color system is superior to the RGB system. After hours of searching I found a VBA sample and ported it to WPF (the code can be found at the bottom of the article). 

The next thing to do was to create a new slider for the __Hue__ and a triangle for __Lightness__ and __Saturation__ values. 

__The Slider__

It was easy to create a Color Slider since it is possible to override the default template. I created a gradient from hue 0 over hue 0.1, hue 0.2 … to hue 1.0 and put it into the background. At last I restyled the value selection thumb using Interactive Designer and the result looked like this. 

![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/070406_0011_HLSColorS2.png) 

__The Triangle__

The triangle was a bit more complex than the slider since I had to start from scratch. I created the triangle shape using Paths and Interactive Designer. The I assigned 3 backgrounds with different opacity masks to it. 

1.  Background Black From lower left to upper right

Background White From upper left to lower right

Background Colored HLS(CurrentHue, 0.5, 1) from right to left

To show the current selected Lightness and Saturation I added an ellipse. 

![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/070406_0011_HLSColorS3.png) 

The next step was to attach _MouseDown_, _MouseUp_ and _MouseMove_ event Handlers to the triangle. Whenever the mouse is moved over the triangle with the left button pressed the ellipse should follow. To ensure that the ellipse is moved correctly when you leave the triangle with the left button pressed I assigned _Mouse.Capture(element, Element)_ to the element on _MouseDown_ and _Mouse.Capture(element, None)_ on _MouseUp_. 

Last but not least I had to write the code that generates the appropriate color from slider and ellipse position. The complete code can be found [here](http://www.dev-jc-vb.de/dev-jc-vb/blog/ColorSelector.zip). 

__The Result__

![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/070406_0011_HLSColorS4.png) 

In my opinion it was worth to spent some time to create this control. I hope that more people will begin building their own controls and make them available to the public. My control is not perfect and could be done in other ways. This is article is supposed to show only <u>one</u> possible way. 

__Rgb <--> Hls Converter Code__

``` vb
Imports System.Windows.Media 



Public Class HlsConverter 

Public Shared Function ConvertFromHls(ByVal value As HlsValue) As Color 

Return ConvertFromHls(value.Hue, value.Lightness, value.Saturation) 

End Function 




Public Shared Function ConvertFromHls(ByVal h As Double, ByVal l As Double, ByVal s As Double) As Color 

Dim p1 As Double 

Dim p2 As Double 




Dim r As Double 

Dim g As Double 

Dim b As Double 


h *= 360 



If l <= 0.5 Then 

p2 = l * (1 + s) 

Else 

p2 = l + s - l * s 

End If 

p1 = 2 * l - p2 

If s = 0 Then 

r = l 

g = l 

b = l 

Else 

r = QqhToRgb(p1, p2, h + 120) 

g = QqhToRgb(p1, p2, h) 

b = QqhToRgb(p1, p2, h - 120) 

End If 




r *= Byte.MaxValue 

g *= Byte.MaxValue 

b *= Byte.MaxValue 




Return Color.FromRgb(CByte(r), CByte(g), CByte(b)) 

End Function 




Public Shared Function QqhToRgb(ByVal q1 As Double, ByVal q2 As _ 

Double, ByVal hue As Double) As Double 

If hue > 360 Then 

hue = hue - 360 

ElseIf hue < 0 Then 

hue = hue + 360 

End If 

If hue < 60 Then 

QqhToRgb = q1 + (q2 - q1) * hue / 60 

ElseIf hue < 180 Then 

QqhToRgb = q2 

ElseIf hue < 240 Then 

QqhToRgb = q1 + (q2 - q1) * (240 - hue) / 60 

Else 

QqhToRgb = q1 

End If 

End Function 




Public Shared Function ConvertToHls(ByVal color As Color) As HlsValue 

Dim max As Double 

Dim min As Double 

Dim diff As Double 

Dim r_dist As Double 

Dim g_dist As Double 

Dim b_dist As Double 




Dim r As Double 

Dim g As Double 

Dim b As Double 




Dim h As Double 

Dim l As Double 

Dim s As Double 




r = color.R / Byte.MaxValue 

g = color.G / Byte.MaxValue 

b = color.B / Byte.MaxValue 




max = R 

If max < G Then max = G 

If max < B Then max = B 




min = R 

If min > G Then min = G 

If min > B Then min = B 




diff = max - min 

L = (max + min) / 2 

If Math.Abs(diff) < 0.00001 Then 

s = 0 

h = 0 

Else 

If l <= 0.5 Then 

s = diff / (max + min) 

Else 

s = diff / (2 - max - min) 

End If 




r_dist = (max - r) / diff 

g_dist = (max - g) / diff 

b_dist = (max - b) / diff 




If r = max Then 

h = b_dist - g_dist 

ElseIf g = max Then 

h = 2 + r_dist - b_dist 

Else 

h = 4 + g_dist - r_dist 

End If 




h = h * 60 

If h < 0 Then h = h + 360 

End If 




h /= 360 

Return New HlsValue(h, l, s) 

End Function 

End Class 




Public Structure HlsValue 

Private mHue As Double 

Private mLightness As Double 

Private mSaturation As Double 




Public Sub New(ByVal h As Double, ByVal l As Double, ByVal s As Double) 

mHue = h 

mLightness = l 

mSaturation = s 

End Sub 




Public Property Hue() As Double 

Get 

Return mHue 

End Get 

Set(ByVal value As Double) 

mHue = value 

End Set 

End Property 




Public Property Lightness() As Double 

Get 

Return mLightness 

End Get 

Set(ByVal value As Double) 

mLightness = value 

End Set 

End Property 




Public Property Saturation() As Double 

Get 

Return mSaturation 

End Get 

Set(ByVal value As Double) 

mSaturation = value 

End Set 

End Property 




Public Shared Operator <>(ByVal left As HlsValue, ByVal right As HlsValue) As Boolean 

Dim bool1 As Boolean 


bool1 = left.Lightness <> right.Lightness Or left.Saturation <> right.Saturation 



If Not bool1 Then 

Return left.Hue <> right.Hue And (left.Hue > 0 And right.Hue < 1) And (left.Hue < 1 And right.Hue > 0) 

Else 

Return bool1 

End If 

End Operator 




Public Shared Operator =(ByVal left As HlsValue, ByVal right As HlsValue) As Boolean 

Dim bool1 As Boolean 


bool1 = left.Lightness = right.Lightness And left.Saturation = right.Saturation 



If bool1 Then 

Return Math.Round(left.Hue, 2) = Math.Round(right.Hue, 2) Or (Math.Round(left.Hue, 2) = 1 And Math.Round(right.Hue, 2) = 0) Or (Math.Round(left.Hue, 2) = 0 And Math.Round(right.Hue, 2) = 1) 

Else 

Return False 

End If 

End Operator 




Public Overrides Function ToString() As String 

Return String.Format("H: {0:0.00} L: {1:0.00} S: {2:0.00}", Hue, Lightness, Saturation) 

End Function 


End Structure 
```
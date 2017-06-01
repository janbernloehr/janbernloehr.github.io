---
layout: post
title : "HLS Color Selector"
date : 03.07.2006 22:11:00
tags: [.NET, WPF]
---
{% include JB / setup %}

I searched the web for a while to find a WPF HLS Color selector like Interactive Designer is equipped with. 

![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/070406_0011_HLSColorS1.png) 

Since I didn’t find anything I thought about creating my own one. 

The most difficult task was to convert .NET Rgb Color Values to HLS values (the difference is explained [here](http://en.wikipedia.org/wiki/HLS_color_space)). In my opinion the HLS color system is superior to the RGB system. After hours of searching I found a VBA sample and ported it to WPF (the code can be found at the bottom of the article). 

The next thing to do was to create a new slider for the <strong>Hue</strong> and a triangle for <strong>Lightness</strong> and <strong>Saturation</strong> values. 

<strong>The Slider </strong>

It was easy to create a Color Slider since it is possible to override the default template. I created a gradient from hue 0 over hue 0.1, hue 0.2 … to hue 1.0 and put it into the background. At last I restyled the value selection thumb using Interactive Designer and the result looked like this. 

![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/070406_0011_HLSColorS2.png) 

<strong>The Triangle </strong>

The triangle was a bit more complex than the slider since I had to start from scratch. I created the triangle shape using Paths and Interactive Designer. The I assigned 3 backgrounds with different opacity masks to it. 

1.  <div>Background Black From lower left to upper right </div>

<div>Background White From upper left to lower right </div>

<div>Background Colored HLS(CurrentHue, 0.5, 1) from right to left </div>

To show the current selected Lightness and Saturation I added an ellipse. 

![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/070406_0011_HLSColorS3.png) 

The next step was to attach <em>MouseDown</em>, <em>MouseUp</em> and <em>MouseMove</em> event Handlers to the triangle. Whenever the mouse is moved over the triangle with the left button pressed the ellipse should follow. To ensure that the ellipse is moved correctly when you leave the triangle with the left button pressed I assigned <em>Mouse.Capture(element, Element)</em> to the element on <em>MouseDown</em> and <em>Mouse.Capture(element, None)</em> on <em>MouseUp</em>. 

Last but not least I had to write the code that generates the appropriate color from slider and ellipse position. The complete code can be found [here](http://www.dev-jc-vb.de/dev-jc-vb/blog/ColorSelector.zip). 

<strong>The Result </strong>

![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/070406_0011_HLSColorS4.png) 

In my opinion it was worth to spent some time to create this control. I hope that more people will begin building their own controls and make them available to the public. My control is not perfect and could be done in other ways. This is article is supposed to show only <u>one</u> possible way. 

<strong>Rgb <--> Hls Converter Code </strong>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New"><font size="2"><span style="COLOR: blue">Imports</span> System.Windows.Media </font></span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2"><span style="COLOR: blue">Public</span> <span style="COLOR: blue">Class</span> HlsConverter </font>

<font size="2"><span style="COLOR: blue">Public</span> <span style="COLOR: blue">Shared</span> <span style="COLOR: blue">Function</span> ConvertFromHls(<span style="COLOR: blue">ByVal</span> value <span style="COLOR: blue">As</span> HlsValue) <span style="COLOR: blue">As</span> Color </font>

<font size="2"><span style="COLOR: blue">Return</span> ConvertFromHls(value.Hue, value.Lightness, value.Saturation) </font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">Function </span></font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2"><span style="COLOR: blue">Public</span> <span style="COLOR: blue">Shared</span> <span style="COLOR: blue">Function</span> ConvertFromHls(<span style="COLOR: blue">ByVal</span> h <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double</span>, <span style="COLOR: blue">ByVal</span> l <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double</span>, <span style="COLOR: blue">ByVal</span> s <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double</span>) <span style="COLOR: blue">As</span> Color </font>

<font size="2"><span style="COLOR: blue">Dim</span> p1 <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>

<font size="2"><span style="COLOR: blue">Dim</span> p2 <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2"><span style="COLOR: blue">Dim</span> r <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>

<font size="2"><span style="COLOR: blue">Dim</span> g <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>

<font size="2"><span style="COLOR: blue">Dim</span> b <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New"><font size="2">h *= 360 </font></span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2"><span style="COLOR: blue">If</span> l <= 0.5 <span style="COLOR: blue">Then </span></font>

<font size="2">p2 = l * (1 + s) </font>

<span style="COLOR: blue"><font size="2">Else </font></span>

<font size="2">p2 = l + s - l * s </font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">If </span></font>

<font size="2">p1 = 2 * l - p2 </font>

<font size="2"><span style="COLOR: blue">If</span> s = 0 <span style="COLOR: blue">Then </span></font>

<font size="2">r = l </font>

<font size="2">g = l </font>

<font size="2">b = l </font>

<span style="COLOR: blue"><font size="2">Else </font></span>

<font size="2">r = QqhToRgb(p1, p2, h + 120) </font>

<font size="2">g = QqhToRgb(p1, p2, h) </font>

<font size="2">b = QqhToRgb(p1, p2, h - 120) </font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">If </span></font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2">r *= <span style="COLOR: blue">Byte</span>.MaxValue </font>

<font size="2">g *= <span style="COLOR: blue">Byte</span>.MaxValue </font>

<font size="2">b *= <span style="COLOR: blue">Byte</span>.MaxValue </font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2"><span style="COLOR: blue">Return</span> Color.FromRgb(<span style="COLOR: blue">CByte</span>(r), <span style="COLOR: blue">CByte</span>(g), <span style="COLOR: blue">CByte</span>(b)) </font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">Function </span></font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2"><span style="COLOR: blue">Public</span> <span style="COLOR: blue">Shared</span> <span style="COLOR: blue">Function</span> QqhToRgb(<span style="COLOR: blue">ByVal</span> q1 <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double</span>, <span style="COLOR: blue">ByVal</span> q2 <span style="COLOR: blue">As</span> _ </font>

<font size="2"><span style="COLOR: blue">Double</span>, <span style="COLOR: blue">ByVal</span> hue <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double</span>) <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>

<font size="2"><span style="COLOR: blue">If</span> hue > 360 <span style="COLOR: blue">Then </span></font>

<font size="2">hue = hue - 360 </font>

<font size="2"><span style="COLOR: blue">ElseIf</span> hue < 0 <span style="COLOR: blue">Then </span></font>

<font size="2">hue = hue + 360 </font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">If </span></font>

<font size="2"><span style="COLOR: blue">If</span> hue < 60 <span style="COLOR: blue">Then </span></font>

<font size="2">QqhToRgb = q1 + (q2 - q1) * hue / 60 </font>

<font size="2"><span style="COLOR: blue">ElseIf</span> hue < 180 <span style="COLOR: blue">Then </span></font>

<font size="2">QqhToRgb = q2 </font>

<font size="2"><span style="COLOR: blue">ElseIf</span> hue < 240 <span style="COLOR: blue">Then </span></font>

<font size="2">QqhToRgb = q1 + (q2 - q1) * (240 - hue) / 60 </font>

<span style="COLOR: blue"><font size="2">Else </font></span>

<font size="2">QqhToRgb = q1 </font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">If </span></font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">Function </span></font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2"><span style="COLOR: blue">Public</span> <span style="COLOR: blue">Shared</span> <span style="COLOR: blue">Function</span> ConvertToHls(<span style="COLOR: blue">ByVal</span> color <span style="COLOR: blue">As</span> Color) <span style="COLOR: blue">As</span> HlsValue </font>

<font size="2"><span style="COLOR: blue">Dim</span> max <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>

<font size="2"><span style="COLOR: blue">Dim</span> min <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>

<font size="2"><span style="COLOR: blue">Dim</span> diff <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>

<font size="2"><span style="COLOR: blue">Dim</span> r_dist <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>

<font size="2"><span style="COLOR: blue">Dim</span> g_dist <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>

<font size="2"><span style="COLOR: blue">Dim</span> b_dist <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2"><span style="COLOR: blue">Dim</span> r <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>

<font size="2"><span style="COLOR: blue">Dim</span> g <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>

<font size="2"><span style="COLOR: blue">Dim</span> b <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2"><span style="COLOR: blue">Dim</span> h <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>

<font size="2"><span style="COLOR: blue">Dim</span> l <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>

<font size="2"><span style="COLOR: blue">Dim</span> s <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2">r = color.R / <span style="COLOR: blue">Byte</span>.MaxValue </font>

<font size="2">g = color.G / <span style="COLOR: blue">Byte</span>.MaxValue </font>

<font size="2">b = color.B / <span style="COLOR: blue">Byte</span>.MaxValue </font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2">max = R </font>

<font size="2"><span style="COLOR: blue">If</span> max < G <span style="COLOR: blue">Then</span> max = G </font>

<font size="2"><span style="COLOR: blue">If</span> max < B <span style="COLOR: blue">Then</span> max = B </font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2">min = R </font>

<font size="2"><span style="COLOR: blue">If</span> min > G <span style="COLOR: blue">Then</span> min = G </font>

<font size="2"><span style="COLOR: blue">If</span> min > B <span style="COLOR: blue">Then</span> min = B </font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2">diff = max - min </font>

<font size="2">L = (max + min) / 2 </font>

<font size="2"><span style="COLOR: blue">If</span> Math.Abs(diff) < 0.00001 <span style="COLOR: blue">Then </span></font>

<font size="2">s = 0 </font>

<font size="2">h = 0 </font>

<span style="COLOR: blue"><font size="2">Else </font></span>

<font size="2"><span style="COLOR: blue">If</span> l <= 0.5 <span style="COLOR: blue">Then </span></font>

<font size="2">s = diff / (max + min) </font>

<span style="COLOR: blue"><font size="2">Else </font></span>

<font size="2">s = diff / (2 - max - min) </font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">If </span></font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2">r_dist = (max - r) / diff </font>

<font size="2">g_dist = (max - g) / diff </font>

<font size="2">b_dist = (max - b) / diff </font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2"><span style="COLOR: blue">If</span> r = max <span style="COLOR: blue">Then </span></font>

<font size="2">h = b_dist - g_dist </font>

<font size="2"><span style="COLOR: blue">ElseIf</span> g = max <span style="COLOR: blue">Then </span></font>

<font size="2">h = 2 + r_dist - b_dist </font>

<span style="COLOR: blue"><font size="2">Else </font></span>

<font size="2">h = 4 + g_dist - r_dist </font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">If </span></font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2">h = h * 60 </font>

<font size="2"><span style="COLOR: blue">If</span> h < 0 <span style="COLOR: blue">Then</span> h = h + 360 </font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">If </span></font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2">h /= 360 </font>

<font size="2"><span style="COLOR: blue">Return</span> <span style="COLOR: blue">New</span> HlsValue(h, l, s) </font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">Function </span></font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">Class </span></font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2"><span style="COLOR: blue">Public</span> <span style="COLOR: blue">Structure</span> HlsValue </font>

<font size="2"><span style="COLOR: blue">Private</span> mHue <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>

<font size="2"><span style="COLOR: blue">Private</span> mLightness <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>

<font size="2"><span style="COLOR: blue">Private</span> mSaturation <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2"><span style="COLOR: blue">Public</span> <span style="COLOR: blue">Sub</span> <span style="COLOR: blue">New</span>(<span style="COLOR: blue">ByVal</span> h <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double</span>, <span style="COLOR: blue">ByVal</span> l <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double</span>, <span style="COLOR: blue">ByVal</span> s <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double</span>) </font>

<font size="2">mHue = h </font>

<font size="2">mLightness = l </font>

<font size="2">mSaturation = s </font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">Sub </span></font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2"><span style="COLOR: blue">Public</span> <span style="COLOR: blue">Property</span> Hue() <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>

<span style="COLOR: blue"><font size="2">Get </font></span>

<font size="2"><span style="COLOR: blue">Return</span> mHue </font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">Get </span></font>

<font size="2"><span style="COLOR: blue">Set</span>(<span style="COLOR: blue">ByVal</span> value <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double</span>) </font>

<font size="2">mHue = value </font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">Set </span></font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">Property </span></font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2"><span style="COLOR: blue">Public</span> <span style="COLOR: blue">Property</span> Lightness() <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>

<span style="COLOR: blue"><font size="2">Get </font></span>

<font size="2"><span style="COLOR: blue">Return</span> mLightness </font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">Get </span></font>

<font size="2"><span style="COLOR: blue">Set</span>(<span style="COLOR: blue">ByVal</span> value <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double</span>) </font>

<font size="2">mLightness = value </font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">Set </span></font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">Property </span></font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2"><span style="COLOR: blue">Public</span> <span style="COLOR: blue">Property</span> Saturation() <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double </span></font>

<span style="COLOR: blue"><font size="2">Get </font></span>

<font size="2"><span style="COLOR: blue">Return</span> mSaturation </font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">Get </span></font>

<font size="2"><span style="COLOR: blue">Set</span>(<span style="COLOR: blue">ByVal</span> value <span style="COLOR: blue">As</span> <span style="COLOR: blue">Double</span>) </font>

<font size="2">mSaturation = value </font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">Set </span></font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">Property </span></font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2"><span style="COLOR: blue">Public</span> <span style="COLOR: blue">Shared</span> <span style="COLOR: blue">Operator</span> <>(<span style="COLOR: blue">ByVal</span> left <span style="COLOR: blue">As</span> HlsValue, <span style="COLOR: blue">ByVal</span> right <span style="COLOR: blue">As</span> HlsValue) <span style="COLOR: blue">As</span> <span style="COLOR: blue">Boolean </span></font>

<font size="2"><span style="COLOR: blue">Dim</span> bool1 <span style="COLOR: blue">As</span> <span style="COLOR: blue">Boolean </span></font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New"><font size="2">bool1 = left.Lightness <> right.Lightness <span style="COLOR: blue">Or</span> left.Saturation <> right.Saturation </font></span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2"><span style="COLOR: blue">If</span> <span style="COLOR: blue">Not</span> bool1 <span style="COLOR: blue">Then </span></font>

<font size="2"><span style="COLOR: blue">Return</span> left.Hue <> right.Hue <span style="COLOR: blue">And</span> (left.Hue > 0 <span style="COLOR: blue">And</span> right.Hue < 1) <span style="COLOR: blue">And</span> (left.Hue < 1 <span style="COLOR: blue">And</span> right.Hue > 0) </font>

<span style="COLOR: blue"><font size="2">Else </font></span>

<font size="2"><span style="COLOR: blue">Return</span> bool1 </font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">If </span></font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">Operator </span></font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2"><span style="COLOR: blue">Public</span> <span style="COLOR: blue">Shared</span> <span style="COLOR: blue">Operator</span> =(<span style="COLOR: blue">ByVal</span> left <span style="COLOR: blue">As</span> HlsValue, <span style="COLOR: blue">ByVal</span> right <span style="COLOR: blue">As</span> HlsValue) <span style="COLOR: blue">As</span> <span style="COLOR: blue">Boolean </span></font>

<font size="2"><span style="COLOR: blue">Dim</span> bool1 <span style="COLOR: blue">As</span> <span style="COLOR: blue">Boolean </span></font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New"><font size="2">bool1 = left.Lightness = right.Lightness <span style="COLOR: blue">And</span> left.Saturation = right.Saturation </font></span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2"><span style="COLOR: blue">If</span> bool1 <span style="COLOR: blue">Then </span></font>

<font size="2"><span style="COLOR: blue">Return</span> Math.Round(left.Hue, 2) = Math.Round(right.Hue, 2) <span style="COLOR: blue">Or</span> (Math.Round(left.Hue, 2) = 1 <span style="COLOR: blue">And</span> Math.Round(right.Hue, 2) = 0) <span style="COLOR: blue">Or</span> (Math.Round(left.Hue, 2) = 0 <span style="COLOR: blue">And</span> Math.Round(right.Hue, 2) = 1) </font>

<span style="COLOR: blue"><font size="2">Else </font></span>

<font size="2"><span style="COLOR: blue">Return</span> <span style="COLOR: blue">False </span></font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">If </span></font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">Operator </span></font>
</span>

<span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New">

<font size="2"><span style="COLOR: blue">Public</span> <span style="COLOR: blue">Overrides</span> <span style="COLOR: blue">Function</span> ToString() <span style="COLOR: blue">As</span> <span style="COLOR: blue">String </span></font>

<font size="2"><span style="COLOR: blue">Return</span> <span style="COLOR: blue">String</span>.Format(<span style="COLOR: maroon">"H: {0:0.00} L: {1:0.00} S: {2:0.00}"</span>, Hue, Lightness, Saturation) </font>

<font size="2"><span style="COLOR: blue">End</span> <span style="COLOR: blue">Function </span></font>
</span>

<font size="2"><span style="FONT-SIZE: 10px; FONT-FAMILY: Courier New"><span style="COLOR: blue">End Structure</span></span> </font>

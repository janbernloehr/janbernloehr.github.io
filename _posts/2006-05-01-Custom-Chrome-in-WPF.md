---
layout: post
title : "Custom Chrome in WPF"
date : 01.05.2006 12:50:00
tags: [.NET, WPF]
---
{% include JB / setup %}

UPDATE: As WPF evolvs everything becomes easier. An updated version (winfx beta 2) of the article can be found [here](/forums/blogs/janm/archive/2006/06/19).

Some of you may have wondered who the creators of [Microsoft Max](http://www.microsoft.com/max/) did the Chrome.

![Microsoft Max](http://www.microsoft.com/max/images/screen_sharing.jpg)

Although this breaks one of the [Top 10 Vista rules](http://msdn.microsoft.com/library/en-us/UxGuide/UXGuide/Resources/TopRules/T%20opRules.asp) I will show how to create a WPF window with custom chrome.

You can't get there by simply setting the border of a WPF Window to None because always a thick border remains. You have to do a work around and create an empty Win32 window and fill it with your own WPF controls.

You can use the <strong>HwndSource</strong> object to do this. This object is intended to place WPF controls into Win32 Applications but you can also use it to place a WPF control onto your Windows Desktop which is also a Win32 application. HwndSource will call the native function CreateWindowEx and create the window for you.

````
Dim params As HwndSourceParameters = New HwndSourceParameters("CustomChrome 1", 400, 300)  
Dim hwnd As HwndSource = New HwndSource(params)  
hwnd.RootVisual = New Page1
````

This code will create a default window for you. The <strong>HwndSourceParameters</strong>'s constructor sets the Name, Width and Height of the window to create. More parameters will be used later in this article.

HwndSource's <strong>RootVisual</strong> Property points to the WPF control being displayed by HwndSource.

I created a simple page with a green background:

![](http://www.dev-jc-vb.de/dev-jc-vb/Articles/Blog/WPF/CustomChrome1.PNG)

The Window has still the default Windows Chrome but it's very easy to get rid of it. The key is the HwndSourceParameters object passed to the HwndSource constuctor. The   
[WindowStyle](http://msdn.microsoft.com/library/en-us/winui/winui/WindowsUserInterface/Windowing/Windows/WindowReference/WindowStyles.asp) property lets you define how the window looks like.

````
 

Public Const WS_POPUP As Integer = &H80000000  
Public Const WS_CLIPCHILDREN As Integer = &H2000000  
Public Const WS_VISIBLE As Integer = &H10000000

...

Dim params As HwndSourceParameters = New HwndSourceParameters("CustomChrome 1", 400, 300)  
params.WindowStyle = WS_POPUP Or WS_CLIPCHILDREN Or WS_VISIBLE  
params.SetPosition(20, 20)Dim hwnd As HwndSource = New HwndSource(params)  
hwnd.RootVisual = New Page1


````  

Now the Chrome has gone but we aren't finished yet. As you can see, the Max Window  
has <u>round</u> corners ours not.

![](http://www.dev-jc-vb.de/dev-jc-vb/Articles/Blog/WPF/CustomChrome2.PNG)

To create a non rectangular Window you need a Region, which can be created by serval API functions. In this special case let's pick <strong>CreateRoundRectRgn</strong> which creates a rectangle with round corners.

````
Public Declare Auto Function CreateRoundRectRgn Lib "gdi32" Alias "CreateRoundRectRgn" (ByVal X1 As Integer, ByVal Y1 As Integer, ByVal X2 As Integer, ByVal Y2 As Integer, ByVal X3 As Integer, ByVal Y3 As Integer) As IntPtr
````

X1 and Y1 define the StartPoint of the region, X2 and Y2 the EndPoint. X3 and Y2  
define the radius of the edges.

Now you have to set the Window's region to the created one by calling <strong>SetWindowRgn</strong>.

````
Public Declare Auto Function SetWindowRgn Lib "user32.dll" (ByVal hWnd As IntPtr, ByVal hRdn As IntPtr, ByVal bRedraw As Boolean) As Integer
````

<strong>SetWinowRgn</strong> requires the Handle to the window whose window region is to be set.  
You can get this Handle from <strong>HwndSource.Handle</strong>.

````
Dim rgn As IntPtrrgn = CreateRoundRectRgn(0, 0, 400, 300, 10, 10)  
SetWindowRgn(hwnd.Handle, rgn, True)
````

The result should look like this:

![](http://www.dev-jc-vb.de/dev-jc-vb/Articles/Blog/WPF/CustomChrome3.PNG)

I created the Chrome with WPF Elements using Interactive Designer.

Last but not least you have to react on a closing of your window. The <strong>Disposed</strong> Event  
is raised when the window is either closed by ALT+F4 etc. or <strong>HwndSource.Dispose</strong> is called (Close Method).

````
AddHandler hwnd.Disposed, AddressOf hwnd_Disposed  

Private Sub hwnd_Disposed(ByVal sender As Object, ByVal e As EventArgs)  
MyApp.Current.Shutdown()  
End Sub
````

Creating a Window with custom chrome in WPF is very easy. You can define any shape you want by combining regions and use every effect and animation feature you like.

Our Window does not yet support <strong>Sizing </strong>and <strong>Moving</strong> but I will continue this tutorial in a few days ;-)

<strong>Read On:</strong>

*   [The hwnd interop white paper](http://blogs.msdn.com/nickkramer/archive/2005/07/18/440085.aspx) 
[Fun with window regions](http://www.flounder.com/setwindowrgn.htm)
440085.aspx) 
[Fun with window regions](http://www.flounder.com/setwindowrgn.htm)
ttp://www.flounder.com/setwindowrgn.htm)

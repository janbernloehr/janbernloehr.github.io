---
layout: post
title : "How to create a Microsoft Max style window in WPF"
date : 19.06.2006 22:12:00
tags: [.NET, WPF]
---
{% include JB / setup %}


When I first saw [Microsoft Max](http://www.microsoft.com/max/) I just though wow! how did they cheat ;) [Sorry for that] 

In fact WPF does really support such windows (even without losing hardware acceleration). This article will show you one way to achieve a Max-like window frame. 

## Get rid of the window frame 

First of all you have to know that WPF Windows are based on old fashioned Win32 window objects. The frame (even the modern vista frame) is rendered by GDI/GDI+. Only the client area is done by WPF itself.So all we have to do is to get rid of the default frame and replace it by an own one. Which frame GDI renders is defined by a [WindowStyle](http://msdn.microsoft.com/library/en-us/winui/winui/WindowsUserInterface/Windowing/Windows/WindowReference/WindowStyles.asp) and a WindowStyleEx property. You can easily set this style even if the window is already created using the API function [SetWindowLong](http://msdn.microsoft.com/library/en-us/winui/winui/windowsuserinterface/windowing/windowclasses/windowclassreference/windowclassfunctions/setwindowlong.asp)(hWnd, nIndex, dwNewLong). hWnd is the handle to the Win32 window, nIndex specifies which type of style you want to set and dwNewLong is the new style value. The question is where we can get the handle from since a Wpf Window does not expose its handle as a property. 

The namespace [System.Windows.Interop](http://windowssdk.msdn.microsoft.com/en-us/system.windows.interop(VS.80).aspx) contains a class called [WindowInteropHelper](http://windowssdk.msdn.microsoft.com/en-us/system.windows.interop.windowinterophelper(VS.80).aspx) which does the dirty work for us. We should call <em>SetWindowLong</em> before the window is shown (so we can’t use the Loaded event) and after the handle is created (so we can’t use the Initialized event). The solution is the [SourceInitialized](http://windowssdk.msdn.microsoft.com/en-us/system.windows.window.sourceinitialized(VS.80).aspx) event, which is called after the handle is created but before the window is actually shown. 

````VB.NET

Private Sub mWindow_SourceInitialized(ByVal sender As Object, ByVal e As System.EventArgs) Handles mWindow.SourceInitialized 

Dim wnd As Window 

Dim helper As WindowInteropHelper 




wnd = DirectCast(sender, Window) 

helper = New WindowInteropHelper(wnd) 




_Handle = helper.Handle 

_Source = HwndSource.FromHwnd(_Handle) 


_Source.AddHook(New HwndSourceHook(AddressOf WndProc)) 



Win32Methods.SetWindowLong(_Handle, -16, 0) 

End Sub 

Private Function WndProc(ByVal hwnd As IntPtr, ByVal msg As Integer, ByVal wParam As IntPtr, ByVal lParam As IntPtr, ByRef handled As Boolean) As IntPtr 

'TODO: process window messages here 

End Sub 

````

I created the helper first to get the handle. Then I also created a [HwndSource](http://windowssdk.msdn.microsoft.com/en-us/system.windows.interop.hwndsource(VS.80).aspx) object (found in System.Windows.Interop) which provides wpf-points to screen transforms and a hook. The hook method is called whenever the Win32 window receives a window message. Yes WPF Windows <u>use and support</u> window messages to interact with the desktop. At last set the window style to 0 which creates a plain window without frame. 

To get rid of the ugly white rectangle I also created a style using Expression Interactive Designer (you can download the complete source at the bottom of this article).

![No window frame](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/NoFrame.PNG)

The result looks nice but we still don’t have round border yet. 

## Make the window border round 

<em>Note: I don’t use window transparency here because this would WPF make to fall back to software rendering so: we again need some Windows API knowledge to understand the next steps. </em>

Every window is defined by and drawn to a region. In general all window regions are rectangular. In our case we will create a rectangular one with round corners using the API function [CreateRoundRectRgn](http://msdn.microsoft.com/library/en-us/gdi/regions_7wa6.asp)(X1, Y1, X2, Y2, X3, Y3). 
<em>

(X1, Y1) is the upper left point of the region (In general 0,0) 

(X2, Y2) is the lower right point of the region (In general Width, Height) 

X3 is the x radius of the corner circle. 

Y3 is the y radius of the corner circle. 
</em>

The created region can be applied by calling [SetWindowRgn](http://msdn.microsoft.com/library/en-us/gdi/pantdraw_2him.asp)(hWnd, hRdn, bRedraw). 


````VB.NET
Private Sub _UpdateCorners() 

Dim deviceSize As Point 


deviceSize = _Source.CompositionTarget.TransformToDevice.Transform(New Point(Window.ActualWidth, Window.ActualHeight)) 

Dim rges As IntPtr 

rges = Win32Methods.CreateRoundRectRgn(0, 0, CInt(deviceSize.X) + 1, CInt(deviceSize.Y) + , radiusx, radiusy) 



Win32Methods.SetWindowRgn(_Handle, rges, True) 

End Sub 
````

You have to update the window regions during <em>SourceInitialized</em> and every time the window is resized (<em>SizeChanged</em> event).

![Window with round borders](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/RoundBorders.PNG)

Now we have a good looking window with round corners but I’m not happy yet because I neither can move nor resize it. 

## Window Moving 

To make the window moveable you can use following source. 


````VB.NET
Private Sub Window1_MouseDown(ByVal sender As Object, ByVal e As System.Windows.Input.MouseButtonEventArgs) Handles Me.MouseDown 

If System.Windows.Input.Mouse.LeftButton = MouseButtonState.Pressed Then Me.DragMove() 

End Sub
````


Now you can move your window by pressing the left button of your mouse while over the window and then move your mouse. 

## Window Resizing 

Maybe you noticed the Sizing Controls section in my Window Template (colored in the picture below).

![Window with resize controls](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/SizingControls.PNG)

These controls can be used to make the window resizable. Unfortunately we can’t call something like DragSize to do this but it is really easy to achieve. 

You can send Windows a System Command using the API function [SendMessage](http://msdn.microsoft.com/library/en-us/winui/winui/windowsuserinterface/windowing/messagesandmessagequeues/messagesandmessagequeuesreference/messagesandmessagequeuesfunctions/sendmessage.asp). This makes window to resize the window while moving the mouse. 

````VB.NET
Public Sub DragSize(ByVal sizingAction As SizingAction) 

If System.Windows.Input.Mouse.LeftButton = Input.MouseButtonState.Pressed Then 

Win32Methods.SendMessage(_Handle, WindowMessage.WM_SYSCOMMAND, CType(SysCommand.SC_SIZE + sizingAction, IntPtr), IntPtr.Zero) 

Win32Methods.SendMessage(_Handle, 514, IntPtr.Zero, IntPtr.Zero) 

End If 

End Sub 

''' <summary> 

''' Maps Win32 Sizing Actions 

''' </summary> 

''' <remarks></remarks> 

Public Enum SizingAction 

North = 3 

South = 6 

East = 2 

West = 1 

NorthEast = 5 

NorthWest = 4 

SouthEast = 8 

SouthWest = 7 

End Enum 
````

The first parameter is the window handle, the second identifies this window message as a SystemCommand and the third parameter specifies in which ways you want to resize your window. This is important since every border does a different resize (e.g. The right border resize in/decreases the window’s width). The SizingAction enum allows you to easily specify at which place your resize border lives. 

DragSize(SizingAction.South) makes the window behave as if you are dragging it’s south border. 

In this sample I placed Line elements with a transparent background on the borders of the Window, which react to a MouseDown event and call DragSize.

## Summary 

I showed you one way to extend WPF windows. Consider that this article is breaking [rule 3 of the Top Vista rules](http://msdn.microsoft.com/library/en-us/uxguide/uxguide/Resources/TopRules/TopRules.asp) because of creating a custom window chrome. Although this is a very clean way to implement a custom chrome because we are not losing not a bit of the WPF functionality. 

You can download the complete Source [here](http://www.dev-jc-vb.de/dev-jc-vb/blog/WindowStyleExtender.zip). 

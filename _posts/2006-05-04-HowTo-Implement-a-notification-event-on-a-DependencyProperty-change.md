---
layout: post
title : "HowTo: Implement a notification event on a DependencyProperty change"
date : 04.05.2006 16:26:00
tags: [.net, wpf]
---
{% include JB/setup %}

The only way __DependencyProperties__ tell that something happend to them seems to be via a __binding__.

So I implemented an DependencyObject that binds to the the property to be watched.

````VB.NET
 

Public Class DependencyProperyChangedHelper(Of T)  
    Inherits DependencyObject

    Public Shared ReadOnly ValueProperty As DependencyProperty = DependencyProperty.Register("Value", GetType(T), GetType(DependencyProperyChangedHelper(Of T)))

    Public Sub New(ByVal d As DependencyObject, ByVal propName As String)  
        Dim bnd As New Binding(propName)  
        bnd.Source = d  
        bnd.Mode = BindingMode.OneWay

        System.Windows.Data.BindingOperations.SetBinding(Me, ValueProperty, bnd)  
    End Sub

    Public Property Value() As T  
        Get  
            Return CType(Me.GetValue(ValueProperty), T)  
        End Get  
        Set(ByVal value As T)  
            Me.SetValue(ValueProperty, value)  
        End Set  
    End Property  
End Class


```` 

You can e.g. assign a binding to a TextBox's TextProperty:

````VB.NET
Dim autoCompleteHelper As New DependencyProperyChangedHelper(Of String)(Me.txtNotify, "Text")
````

The constructor builds a Helper for a __String__ Property on __txtNotify__ named __Text__.

At the moment the Helper does not do anything else than apply the binding but when you take a closer look at the DependencyProperty.Register overloads you will notify that a __PropertyChangedCallback__ can be supplied:

````VB.NET
    Public Shared ReadOnly ValueProperty As DependencyProperty = DependencyProperty.Register("Value", GetType(T), GetType(DependencyProperyChangedHelper(Of T)), New FrameworkPropertyMetadata(New PropertyChangedCallback(AddressOf Value_Changed)))

    Private Shared Sub Value_Changed(ByVal d As DependencyObject, ByVal e As DependencyPropertyChangedEventArgs)  
        DirectCast(d, DependencyProperyChangedHelper(Of T)).OnValueChanged(e)  
    End Sub

    ' A Clr Event that is raied whenever the value changes  
    Public Event ValueChanged As DependencyPropertyChangedEventHandler

    ' A Helper Method to raise the event  
    Private Sub OnValueChanged(ByVal e As System.Windows.DependencyPropertyChangedEventArgs)  
        RaiseEvent ValueChanged(Me, e)  
    End Sub
````

From now on the helper object raises clr events whenever the property it is bound to changes.

__Complete Code:__

````VB.NET
 
Public Class DependencyProperyChangedHelper(Of T)  
    Inherits DependencyObject

    Private mTrackedObject As DependencyObject  
    Public Shared ReadOnly ValueProperty As DependencyProperty = DependencyProperty.Register("Value", GetType(T), GetType(DependencyProperyChangedHelper(Of T)), New FrameworkPropertyMetadata(New PropertyChangedCallback(AddressOf Value_Changed)))

    Public Sub New(ByVal d As DependencyObject, ByVal propName As String)  
        mTrackedObject = d

        Dim bnd As New Binding(propName)  
        bnd.Source = d  
        bnd.Mode = BindingMode.OneWay

        System.Windows.Data.BindingOperations.SetBinding(Me, ValueProperty, bnd)  
    End Sub

    Public ReadOnly Property TrackedObject() As DependencyObject  
        Get  
            Return mTrackedObject  
        End Get  
    End Property

    Public Property Value() As T  
        Get  
            Return CType(Me.GetValue(ValueProperty), T)  
        End Get  
        Set(ByVal value As T)  
            Me.SetValue(ValueProperty, value)  
        End Set  
    End Property

    Private Shared Sub Value_Changed(ByVal d As DependencyObject, ByVal e As DependencyPropertyChangedEventArgs)  
        DirectCast(d, DependencyProperyChangedHelper(Of T)).OnValueChanged(e)  
    End Sub

    Private Sub OnValueChanged(ByVal e As System.Windows.DependencyPropertyChangedEventArgs)  
        RaiseEvent ValueChanged(Me, e)  
    End Sub

    Public Event ValueChanged As DependencyPropertyChangedEventHandler  
End Class

````



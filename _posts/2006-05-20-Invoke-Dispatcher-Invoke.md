---
layout: post
title : "Invoke != Dispatcher.Invoke"
date : 20.05.2006 13:48:00
tags: [.net, wpf, threading]
---
{% include JB/setup %}

Sometimes applications need to do tasks asynchronously to keep the UI responsible. In .NET you can easily achieve this by using multithreading e.g. via the ThreadPool.  
Since methods created on a different thread than the owner thread of an Ui Control are not allowed to access the Ui objects you need to synchronise your worker threads with your Ui threads. To do this you can easily call Dispatcher.Invoke(methoddelegate, arguments) instead of Method(arguments) and the WPF Dispatcher does the synchronization for you.  
With all the ease comes a big performance impact when you call Dispatcher.Invoke. For example the Dispatcher does not check whether you are already on the Ui thread. It synchronizes the call in any way even though it wouldn’t be neccessary. Moreover you should avoid calling Dispatcher.Invoke frequently in a short amount of time.

**Test Configuration:**  
Dell Latitude D810  
1,73 Ghz Pentium M  
1 GB DDR 2 RAM  
Windows Server 2k3 SP1  
Visual Studio 2005 Professional; WinFX Feb 05 CTP

**Example 1**  
````VB.NET
Dim Coll1 As New System.Collections.ObjectModel.ObservableCollection(Of String)  

For i As Integer = 0 To 20000  
 Coll1.Add(System.Guid.NewGuid.ToString)  
Next
````

Dispatcher Thread / Threadpool Thread  
30 ms / crash

Objects are added without synchronization. Fast on the dispatcher thread, crash on any other thread.

**Example 2**  
````VB.NET
Private Delegate Sub AddDeleagte(ByVal item As String)  
…  
Dim Coll2 As New System.Collections.ObjectModel.ObservableCollection(Of String)  

For i As Integer = 0 To 20000  
 Dispatcher.Invoke(System.Windows.Threading.DispatcherPriority.Normal, New AddDeleagte(AddressOf Coll2.Add), System.Guid.NewGuid.ToString)  
Next
````

8200 ms / 8200 ms

Objects are added with synchronization. Very slow on the dispatcher thread, slow but safe on any other thread.

**Example 3**  
````VB.NET
Private Delegate Sub AddDeleagte(ByVal item As String)  
…  
Dim Coll3 As New System.Collections.ObjectModel.ObservableCollection(Of String)  

For i As Integer = 0 To 20000  
 If Not Me.Dispatcher.CheckAccess Then Me.Dispatcher.Invoke(System.Windows.Threading.DispatcherPriority.Normal, New AddDeleagte(AddressOf Coll3.Add), System.Guid.NewGuid.ToString) Else Coll3.Add(System.Guid.NewGuid.ToString)  
Next
````

30 ms / 8200 ms

Objects are added with synchronization when required. Fast on the dispatcher thread, slow but safe on any other thread.

Example 3 would be safe and fast on the dispatcher thread but maybe we could do more.  
If Example 3 was executed on any other thread than the dispatcher thread Dispatcher.Invoke would be called 20000 times. Since calling Dispatcher.Invoke produces a lot of overhead it slows down the application significantly.  
The solution is easy: Call a method synchronized that adds the 20000 elements.

**Example 4**

````VB.NET
 

Private Delegate Sub AddToListDelegate()

Private Sub AddToList()  
For i As Integer = 0 To ITEM_COUNT  
             Coll3.Add(System.Guid.NewGuid.ToString)  
Next  
End Sub

…

If Not Me.Dispatcher.CheckAccess Then Dispatcher.Invoke(System.Windows.Threading.DispatcherPriority.Normal, New AddToListDelegate(AddressOf AddToList)) Else AddToList()


````

30 ms / 31 ms

All objects would be added synchronized when required. Save and fast on any thread.

**Conclusion**  
- Consider whether executing methods asynchronously is useful.  
- Check your UI synchronization needs.  
- Try to avoid frequently UI synchronization. 
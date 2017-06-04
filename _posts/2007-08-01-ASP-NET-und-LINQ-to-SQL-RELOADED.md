---
layout: post
title : "ASP.NET und LINQ to SQL RELOADED!"
date : 01.08.2007 00:20:13
tags: [.net, asp.net, linq]
---
{% include JB/setup %}

Blicke ich zurück auf meinen [früheren Post](/2006/08/15/ASP-NET-und-LINQ-ein-fast-perfektes-Team), hat sich bei der Zusammenarbeit von ASP.NET und LINQ in der neusten Beta 2 von Visual Studio 2008 (Codename Orcas) viel getan.

ASP.NET Anwendungen können jetzt problemlos die neuen .NET 3.5 Assemblys einbinden und deren Features nutzen.

Für Webentwickler steht sogar die neue **LinqDataSource** bereit, die den Zugriff von allen Data-Bound-Controls wie GridView, ListView, DataRepeater etc. auf einen Linq to Sql DataContext gewährt und neben automatischem **Sorting** und **Paging** auch **Insert-**, **Upadte-** und **Delete-Querys** unterstützt.

Ist ein Linq-to-Sql DataContext im `AppCode` Folder oder in einer referenzierten Assembly vorhanden, kann man wie bei der Sql Data Source gewohnt mit dem Visual Designer arbeiten. Probleme bereiten noch `Where`-Conditions und komplexe Queries, doch in der **LinqDataSource** steckt mehr als Microsoft offen zugeben will.

Das **Selecting** Event der LinqDataSource bietet einem die Möglichkeit die vordefinierte Linq Query über den Parameter `e.Result` zu erstetzten. Dabei ist man nicht auf den Linq-to-Sql DataContext beschränkt, sondern kann jede beliebige Linq Query verwenden (z.B. Linq-to-Objects, Linq-to-Entities, Linq-to-Amazon, etc.) und das <u>ohne</u> den Verlust von Sorting und Paging.

**Hier eine kleine Demonstration**

ASP .NET Declaration 
````html
<asp:GridView ID="GridView1" runat="server" AllowPaging="True" AllowSorting="True" DataSourceID="PersonSource" />
<asp:LinqDataSource ID="PersonSource" runat="server" />
````

ASP .NET Code 
````vb
Partial Class _Default Inherits System.Web.UI.Page

    Protected Sub PersonSource_Selecting(ByVal sender As Object, ByVal e As     System.Web.UI.WebControls.LinqDataSourceSelectEventArgs) Handles PersonSource.Selecting
        Dim persons As Person() = New Person() {New Person With {.Name = "Jan", .Age = 19}, New Person With {.Name = "Albert", .Age = 23}, New Person With {.Name = "Robert", .Age = 61}}
        e.Result = From x In persons
                   Where x.Age > 20
    End Sub
End Class

Public Class Person
    Private _Name As String
    
    Public Property Name() As String
        Get
            Return _Name
        End Get
        Set(ByVal value As String)
            _Name = value
        End Set
    End Property
    
    Private _Age As Integer
    
    Public Property Age() As Integer
        Get
            Return _Age
        End Get
        Set(ByVal value As Integer)
            _Age = value
        End Set
    End Property
End Class
````

In diesem Beispiel wird ein einfaches GridView mit Sorting und Paging an eine LinqDataSource gebunden. Im Code wird eine Klasse Person definiert mit den Eigenschaften Name und Age. Im Selecting Event werden einige Person Objekte erstellt. Daraufhin werden mit einer Linq Query diejenigen Person Objekte herausgefiltert, deren Age Eigenschaft größer als 20 ist.

Um sich nun von der Flexibilität der LinqDataSource zu überzeugen muss man lediglich die Anwendung starten.

**Fazit**

Man sieht, dass das ASP.NET Team in den letzten Monaten nicht geschlafen hat, sondern neben der Erstellung der AJAX-Extension und [neuen WebControls](/2007/07/31/ASP-NET-ListView) auch noch die Linq Integration deutlich verbessert hat.

Zwar sind die Query Funktionen noch nicht sehr ausgereift, mit einem kleinen Workaround lässt sich dies aber spielend meistern.

Ab jetzt werde ich immer mehr Linq in meinen ASP.NET Projekten einsetzten!

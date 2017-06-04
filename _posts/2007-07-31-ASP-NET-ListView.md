---
layout: post
title : "ASP .NET ListView"
date : 31.07.2007 18:31:29
tags: [.net, asp.net]
---
{% include JB/setup %}

Mit der kommenden Visual Studio Version Visual Studio 2008 Codename: 'Orcas' wird das .NET Framework um viele neue Features erweitert.

Für ASP.NET gibt es neben den AJAX Erweiterungen nun auch ein neues Control namens **ListView**.

Es handelt sich dabei um eine Neuauflage des DataRepeaters, der schon seit .NET 1.0 enthalten ist. Das ListView ist aber wesentlich flexibler. Es erlaubt einem, für die Daten eine *Container Template* - sprich das drumherum - zu definieren und für die einzelnen Items *Item Templates*.

Beispiel einer ListView-Deklaration

````html
<asp:ListView ID="ListView1" runat="server" DataSourceID="SqlDataSource1">
<LayoutTemplate>
<table runat="server" border="0" style="">
    <thead runat="server">
       <tr runat="server" style=""> <th runat="server"> CompanyName</th> <th runat="server"> ContactName</th> </tr>
    </thead>
    <tbody id="itemContainer" runat="server"></tbody>
</table>
</LayoutTemplate>
<ItemTemplate>
<tr runat="server" style="">
    <td> <asp:Label ID="CompanyNameLabel" runat="server" Text='<%# Eval("CompanyName") %>' /> </td>
    <td> <asp:Label ID="ContactNameLabel" runat="server" Text='<%# Eval("ContactName") %>' /> </td>
</tr>
</ItemTemplate>
</asp:ListView>
````

Das im `<LayoutTemplate>` definierte html table dient als Container für die Daten. Wichtig ist, dass da `<tbody>` Element einen `runat="server"` tag und die Id **itemContainer** hat.

ASP.NET fügt später automatisch die Items in den itemContainer ein. Wie ein Item aussieht, lässt sich über das `<ItemTemplate>` definieren. In diesem Fall eine Zeile in der html table. Es stehen die übligen DataBinding Funktionen wie `<%# Eval("COLUMN_NAME") %>` und `<%# Bind("COLUMN_NAME") %>` zur Verfügung.

Das Ergebniss kann sich bereits sehen lassen:

[![image](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/ASP.NETListView_F8B1/image_thumb.png)](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/ASP.NETListView_F8B1/image.png) 

Neben der html table sind auch andere Elemente wie ein liste (`<ul>`) order ein dropdown (`<select>`) möglich.

Das ListView untersützt im Gegensatz zum DataRepeater auch noch Insert, Update und Delete Funktionen. Anders als beim GridView lässt sich hier das Template für eine ganze Zeile definieren und nicht nur für die einzelnen Zellen.

Aus meiner Sicht ersetzt das ListView die bisherigen ASP.NET DataControls in den meisten Funktionen, nur nach einem Automatischen Sorting feature suche ich vergeblich ...

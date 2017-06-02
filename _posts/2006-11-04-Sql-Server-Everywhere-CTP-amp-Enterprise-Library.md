---
layout: post
title : "Sql Server Everywhere CTP &amp; Enterprise Library"
date : 04.11.2006 14:01:00
tags: [.net, sql-server]
---
{% include JB/setup %}

___UPDATE: Mit dem Erscheinen des Sql Servers 2005 Compact Edition RC1 (ehemals Everywhere Edition) wird der SqlCe-Provider automatisch registriert, was einen zusätzlichen Eintrag in der web.config unnötig macht.___

_Die [Microsoft patterns & practices](http://msdn.microsoft.com/practices/) [Enterprise Library 2.0 January 2006](http://www.microsoft.com/downloads/details.aspx?FamilyId=5A14E870-406B-4F2A-B723-97BA84AE80B5&displaylang=en) setzt auf das Providermodell von ADO .NET 2.0. Für Sql Server, Ole Db, Oracle und auch den [SQL Server 2005 Everywhere Edition](http://www.microsoft.com/sql/CTP_sqlserver2005everywhereedition.mspx) stehen diese Provider zur Verfügung. Dieser Artikel soll zeigen, wie man sie auch einsetzt._

Mit dem DataAccess Application Block der Enterprise Library kann man vollkommen datenbankunabhängige Anwendungen entwerfen. Die Provider von ADO .NET 2.0 machen dies möglich, indem sie die sepeziellen Objekte wie z.b. SqlConnection und SqlCommand für ihre Datenbank erstellen und dem Entwickler allgemeine Objekte wie DbConnection und DbCommand zur Verfügung stellen.  
Durch eine kleine Änderung am ConnectionString kann so z.B. von Oracle auf Sql Server 2005 gewechselt werden.

````xml
<connectionStrings>  
<add name="AppDatabase" connectionString="Data Source=(local);Initial Catalog=dbname;User Id=user;Password=pwd;" __providerName="System.Data.SqlClient"__/>  
</connectionStrings>
````

Will man nun den Sql Server Everywhere mit __providerName="System.Data.SqlServerCe"__ einsetzten, stößt man zunächst auf die Fehlermeldung, dass kein Provider gefunden wird, obwohl sich in der Assembly befindet sich aber der xyz Provider befindet. Das Problem ist lediglich, dass dieser nicht ADO .NET registriert ist.

Die Registrierung ist jedoch mit einem weiteren Eintrag in der web.config ganz einfach zu erreichen:

````xml
<system.data>  
<DbProviderFactories>  
<add name="SQL Server 2005 Everywhere Data Provider"  
invariant="System.Data.SqlServerCe"  
description=".NET Framework Data Provider for Microsoft SQL Server 2005 Mobile Edition"  
type="System.Data.SqlServerCe.SqlCeProviderFactory,  
System.Data.SqlServerCe, Version=9.0.242.0, Culture=neutral,  
PublicKeyToken=89845dcd8080cc91" />  
</DbProviderFactories>  
</system.data>
````

Nun glückt die Instanzierung und die Enterprise Library kann in vollem Umfang eingesetzt werden.

__Zum Weiterlesen__

- [Sql Server Everywhere Edition](http://www.microsoft.com/sql/CTP_sqlserver2005everywhereedition.mspx)
- [Enterprise Library for .NET Framework 2.0](http://msdn.microsoft.com/library/?url=/library/en-us/dnpag2/html/EntLib2.asp)
- [ADO .NET 2.0](http://www.microsoft.com/germany/msdn/library/net/ADONET20.mspx?mfr=true)

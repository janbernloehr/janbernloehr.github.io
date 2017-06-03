---
layout: post
title : "LINQ To Everything"
date : 27.12.2007 12:10:50
tags: [.NET, LINQ, Visual Studio 2008]
---
{% include JB/setup %}

Das .NET Framework 3.5 bringt mit den Spracherweiterungen für C# 3.0 und VB 9.0 ein neues Feature namens Language INtegrated Queries (LINQ). Dadruch können beliebige Abfragen als streng typisierter Ausdruck in VB/C# etc. formuliert werden, dannach werden sie von einem Provider übersetzt gegebenenfalls optimiert und an die Datenquelle gesendet.

Das .NET Framework 3.5 bietet Unterstützung für die Datenquellen

*   Datenbanken wie Sql Server 2005 (Linq To Sql, Linq To Entities)
*   Xml (Linq To Xml)
*   .NET Objekte (Linq To Objects) 

Microsoft hat aber viel dafür getan, das LINQ möglichst offen und flexibel zu gestallten, damit man leicht eigene Provider erstellen kann. Und dies zeigt bereits eine enorme Wirkung, denn auf CodePlex gibt es bereits zahlreiche weitere Provider

*   Sharepoint (LINQ to Sharepoint [http://www.codeplex.com/LINQtoSharePoint](http://www.codeplex.com/LINQtoSharePoint))
*   Active Directory (LINQtoAD [http://www.codeplex.com/LINQtoAD](http://www.codeplex.com/LINQtoAD))
*   flickr (LINQ.flickr [http://www.codeplex.com/LINQFlickr](http://www.codeplex.com/LINQFlickr))
*   Microsoft Dynamics (LinqtoCRM [http://www.codeplex.com/LinqtoCRM](http://www.codeplex.com/LinqtoCRM))
*   Google (glinq [http://www.codeplex.com/glinq](http://www.codeplex.com/glinq)) 

Neben diesen Projekten will ich noch drei weitere erwähnen

Slinq erweitert LINQ um den Zugriff auf Stream Daten ([http://www.codeplex.com/Slinq](http://www.codeplex.com/Slinq))

InterLINQ implementiert einen n-Tier Ansatz für Linq To Sql ([http://www.codeplex.com/interlinq](http://www.codeplex.com/interlinq))

NLINQ hat sich zum Ziel gesetzt Linq für Visual Studio 2003 und 2005 zur Verfügung zu stellen ([http://www.codeplex.com/nlinq](http://www.codeplex.com/nlinq))

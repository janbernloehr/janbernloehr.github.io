---
layout: post
title : "Sql Server Everywhere Edition ist nun Sql Server Compact Edition"
date : 14.11.2006 23:41:17
tags: [.NET, Visual Studio 2005, Sql Server]
---
{% include JB / setup %}

Microsoft hat heute den Namen des bereits als Sql Server Everywhere Edition bekannten Sql Server abkömlings bekannt gegeben, der auch ohne Installation als Service läuft.

Die Compact Edition basiert auf der [Mobile Edition 2005](http://www.microsoft.com/sql/editions/sqlmobile/default.mspx) und besteht im Wesentlichen aus der Assembly **System.Data.SqlServerCe**, die in jedes .NET Projekt eingebunden werden kann. Die Daten werden in einer Datei mit der Endung **.sdf** abgelegt; der Zugriff erfolgt über die typischen ADO .NET Klassen mit dem Prefix **SqlCe** (SqlCeConnection, SqlCeCommand, SqlCeDataReader etc.) also im Gegensatz zu den Klassen des "großen" Bruders mit dem Prefix **Sql**.

Der Sql Server Compact Edition ist demnach eigentlich kein richtiger Sql "Server" mehr, da er nicht als Service für alle Applikationen zur Verfügung steht, sondern nur für die aktuelle Anwendung. Aus diesem Grund ist auch stets nur eine Verbindung zur Datenbank erlaubt, was aber für gewöhnliche Anwendungen ausreicht.

Jede Anwendung kann nun Daten im gewohnten Datenbankformat speichern, ohne eine Installation des Sql Servers auf dem Zielrechner zu erfordern. Das spart Zeit, da kein aufwendiges Speicherframework entwickelt werden muss, und bringt Performance, da der Sql Server Compact viel schneller ist als Daten, die aus einer Xml Datenbank geladen werden müssen. Außerdem ist die Compact Edition für kleinen Arbeitsspeicherverbrauch optimiert.

Bei der Erstellung einer ClickOnce Installation kann der Sql Server Compact mitausgewählt werden und ist damit auch automatisch bei jedem Start der Anwendung verfügbar.

Abgeschlossen wird die Compact Edition durch die **Sql Server Compact Edtion 2005 Tools for Visual Studio 2005**, durch die sich die Compact Edition nathlos in Visual Studio 2005 integriert und kompfortablen Zugriff ermöglicht.

**Momentan steht der Sql Server Compact Edition als RC1 zum Download**

Microsoft SQL Server 2005 Compact Edition RC1  
[http://www.microsoft.com/downloads/details.aspx?FamilyID=85e0c3ce-3fa1-453a-8ce9-af6ca20946c3&DisplayLang=en](http://www.microsoft.com/downloads/details.aspx?FamilyID=85e0c3ce-3fa1-453a-8ce9-af6ca20946c3&DisplayLang=en "http://www.microsoft.com/downloads/details.aspx?FamilyID=85e0c3ce-3fa1-453a-8ce9-af6ca20946c3&DisplayLang=en") 

SQL Server 2005 Compact Edition Books Online Community Technology Preview (CTP)  
[http://www.microsoft.com/downloads/details.aspx?FamilyID=e6bc81e8-175b-46ea-86a0-c9dacaa84c85&DisplayLang=en](http://www.microsoft.com/downloads/details.aspx?FamilyID=e6bc81e8-175b-46ea-86a0-c9dacaa84c85&DisplayLang=en "http://www.microsoft.com/downloads/details.aspx?FamilyID=e6bc81e8-175b-46ea-86a0-c9dacaa84c85&DisplayLang=en") 

Microsoft SQL Server 2005 Everywhere Edition Tools for Visual Studio 2005 Service Pack 1 Beta  
[http://www.microsoft.com/downloads/details.aspx?FamilyID=61289b5d-af86-45dd-8962-e7dcc5221796&displaylang=en](http://www.microsoft.com/downloads/details.aspx?FamilyID=61289b5d-af86-45dd-8962-e7dcc5221796&displaylang=en "http://www.microsoft.com/downloads/details.aspx?FamilyID=61289b5d-af86-45dd-8962-e7dcc5221796&displaylang=en")

**Weitere Informationen**

[http://www.microsoft.com/sql/editions/compact/default.mspx](http://www.microsoft.com/sql/editions/compact/default.mspx "http://www.microsoft.com/sql/editions/compact/default.mspx")

[http://www.microsoft.com/sql/editions/sqlmobile/default.mspx](http://www.microsoft.com/sql/editions/sqlmobile/default.mspx "http://www.microsoft.com/sql/editions/sqlmobile/default.mspx")

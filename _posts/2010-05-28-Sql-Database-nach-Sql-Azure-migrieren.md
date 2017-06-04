---
layout: post
title : "Sql Database nach Sql Azure migrieren"
date : 28.05.2010 11:51:39
tags: [azure, sql-server]
---
{% include JB/setup %}

## 1. Datenbank einrichten

Die Datenbank erstellt man über das WebInterface zu finden auf [http://windows.azure.com](http://windows.azure.com).

[![image](http://www.vb-magazin.de/forums/blogs/janm/image_thumb_7DC72601.png "image")](http://www.vb-magazin.de/forums/blogs/janm/image_27F6D3EA.png) 

Anschließend muss die Firewall so konfiguriert werden, dass man vom lokalen Computer Zugriff auf die Datenbank hat. Soll die Sql Azure Datenbank außerdem von einem Windows Azure Service verwendet werden, dann muss auch “Allow Microsoft Services” aktiviert werden.

[![image](http://www.vb-magazin.de/forums/blogs/janm/image_thumb_69CD936B.png "image")](http://www.vb-magazin.de/forums/blogs/janm/image_31FB4F48.png) 

## 1. Sql Server 2008 R2 CTP

Um bequem auf die Sql Azure Datenbanken zuzugreifen ist Sql Server 2008 R2 nötig, das Management Studio der “normalen” 2008er Version hat leider keinen Zugriff.

Die CTP steht hier zum Download (kostenlos)

[http://www.microsoft.com/sqlserver/2008/en/us/R2Downloads.aspx](http://www.microsoft.com/sqlserver/2008/en/us/R2Downloads.aspx)

## 2. Datenbank Skripte generieren

Der einfachste Weg, die lokale Datenbank mit oder ohne Daten in die Cloud zu bekommen ist mittels Sql Skripten. Sql 2008 R2 bringt dazu auch gleich die nötigen Wizards mit. Einfach die Datenbank auswählen –> Tasks –> Generate Scripts. Dann im Wizard Sql Azure auswählen. Das ist wichtig, da Sql Azure nicht alle Befehle des Sql Servers 2008 unterstützt, der Wizard generiert dann automatisch “bereinigte” Skripte.

[![image](http://www.vb-magazin.de/forums/blogs/janm/image_thumb_3F9DE583.png "image")](http://www.vb-magazin.de/forums/blogs/janm/image_1B590101.png) [![image](http://www.vb-magazin.de/forums/blogs/janm/image_thumb_04D63CAD.png "image")](http://www.vb-magazin.de/forums/blogs/janm/image_5EE08C56.png) 

Ich empfehle zunächst ein Skript für das Schema zu erstellen (Schema only) und dieses auszuführen. Ging alles glatt, kann man anschließend die Daten hinterherschicken (mittels Data only Skript).

Verwendet man eine ASP.NET 3.5 Membership Datenbank ist ein kleiner Fix bei den Stored Procedures notwendig. Ich habe folgenden verwendet (ohne Gewähr)

``` sql
CREATE PROCEDURE [aspnet_Membership_GetNumberOfUsersOnline]   
    @ApplicationName [nvarchar](256),   
    @MinutesSinceLastInActive [int],   
    @CurrentTimeUtc [datetime]   
WITH EXECUTE AS CALLER   
AS   
BEGIN   
    DECLARE @DateActive datetime   
    SELECT  @DateActive = DATEADD(minute,  -(@MinutesSinceLastInActive), @CurrentTimeUtc) 

    DECLARE @NumOnline int   
    SELECT  @NumOnline = COUNT(*)   
    FROM    dbo.aspnet_Users u,   
            dbo.aspnet_Applications a,   
            dbo.aspnet_Membership m   
    WHERE   u.ApplicationId = a.ApplicationId                  AND   
            LastActivityDate > @DateActive                     AND   
            a.LoweredApplicationName = LOWER(@ApplicationName) AND   
            u.UserId = m.UserId   
    RETURN(@NumOnline)   
END
```

## 3. User anlegen

Nachdem die Datenbankstruktur samt Daten nun in der Cloud sind, wollen wir wieder darauf zugreifen.

Dazu mit dem Management Studio in die master Datenbank von Sql Azure einloggen und einen login erstellen.

``` sql
CREATE LOGIN MyCloudUser WITH PASSWORD = 'Password';
```

Anschließend muss der login auf einen Datenbankuser gemapped werden. Dazu loggt man sich wieder in der Sql Azure Datenbank ein und führt folgenden Befehl aus.

``` sql
CREATE USER MyCloudUser FOR LOGIN MyCloudUser;
```

Die Rechteverwaltung in Sql Azure ist analog zu der im Sql Server. Für die meisten Anwendungen genügen Lese- und Schreibrechte, diese kann man so vergeben.

``` sql
EXEC sp_addrolemember N'db_datareader', N'MyCloudUser'
EXEC sp_addrolemember N'db_datawriter', N'MyCloudUser'
```

Sind außerdem Stored Procedures im Spiel, will man auch auf diese den Zugriff erlauben. Will man gleich alle Stored Procedures auf einmal freischalten, empfielht sich die Erstellung einer Rolle und mit den entsprechenden Rechten.

``` sql
CREATE ROLE db_executor   
GRANT EXECUTE TO db_executor
```

Nun müssen lediglich alle Benutzer, die alle Stored Procedures ausführen dürfen sollen zu dieser Rolle hinzugefügt werden

``` sql
EXEC sp_addrolemember N'db_executor', N'MyCloudUser'
```

!! Nur wenn es absolut unabdingbar ist, sollten dem Anwendungsbenutzer auch dbo-Rechte verliehen werden!!

``` sql
-- only if you need dbo access:
EXEC sp_addrolemember N'db_owner', N'MyCloudUser'
```

## 4. Anwendung testen

Jetzt ist die Sql Azure Datenbank bereit für den ersten Anwendungszugriff.

Hinweis: Damit Sql Azure Zugriff gewähren kann, muss aus dem connectionstring der ServerName **servername**.database.windows.net ausgelesen werden. Viele Provider senden, diesen jedoch nicht, daher sollte der Benutzername im Format `user@server` angegeben werden.

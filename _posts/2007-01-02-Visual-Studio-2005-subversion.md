---
layout: post
title : "Visual Studio 2005 + subversion"
date : 02.01.2007 04:45:00
tags: [.net, vs2005, svn]
---
{% include JB/setup %}

Nicht nur für Team Arbeit bietet sich ein SourceControl Provider an. Steht ein Projekt unter SourceControl, verfügt man über ein ständiges BackUp- und Versionierungssystem, durch das Änderungen schnell rückgängig gemacht aber auch gut nachvollzogen und dokumentiert werden können.

Der CVS Nachfolger subversion (im Folgenden SVN genannt) ist eine gute Alternative zu dem etwas in Vergessenheit geratenen Visual SourceSafe.  
Wie für SourceSafe existiert auch für SVN ein Visual Studio PlugIn, wodurch angenehmes Arbeiten ermöglicht wird.

Dieser Artikel soll zeigen, wie man subversion für die Verwendung mit Visual Studio 2005 konfiguriert.

__Vorbereitung__ 

SVN ist sehr flexibel und benötigt keine Installation im eigentlichen Sinne. Ob auf SVN von einem remote client oder lokal zugegriffen wird spielt lediglich für die Firewall eine Rolle.  
Hier wird eine lokale Installation im Verzeichnis `C:\SVN` erstellt. 

Zunächst muss SVN selbst heruntergeladen werden ([http://subversion.tigris.org/](http://subversion.tigris.org/)). Die Version 1.4.2 ist zurzeit aktuell. 

Um SVN komfortabel zu verwalten, empfiehlt sich ebenfalls die Installation von TortoiseSVN ([http://tortoisesvn.tigris.org/](http://tortoisesvn.tigris.org/)). 

Letztlich wird noch das Visual Studio Plug-In Ankh benötigt ([http://ankhsvn.tigris.org/](http://ankhsvn.tigris.org/)). 

__Konfiguration__

Das subversion .ZIP Archiv wird extrahiert und der Inhalt des bin Verzeichnisses nach `C:\SVN\bin` kopiert. 

SVN legt Daten in einem speziellen Verzeichnis, einem sog. repository ab. Nach der ToroiseSVN Installation kann ein solches repository im Explorer mit dem Kommando „create repository here“ eingerichtet werden (z.B. in `C:\SVN\repos\`).
![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualStudio2005subversion_42C7/01CreateRepository12.png) 

In diesem Verzeichnis wird nun die SVN Ordnerstruktur angelegt. Im Unterverzeichnis conf befindet sich die Datei svnserve.conf. Diese ermöglicht eine einfache Konfiguration des SVN repositorys.  
Entfernt man alle # vor einer Zeile, wird diese aktiviert. 

Passwortgeschützter Zugriff wird so aktiviert:

    password-db = passwd

Hier kann eine beliebige Bezeichnung gewählt werden:

    realm = My Local Repository

Bestimmt, dass nicht authentifizierte Benutzer keinen Zugriff haben: 

    anon-access = none

Bestimmt, dass authentifizierte Benutzer Schreibrechte haben. 

    auth-access = write

Für den passwortgeschützten Zugriff muss zusätzlich noch die Datei passwd bearbeitet werden, die die Benutzerkonten enthält. 

Das Schema der Datei lautet. 

    benutzer = passwort

Ist die Konfiguration abgeschlossen, kann nun der SVN „Server“ gestartet werden.  
Das dafür erforderliche Programm svnserve.exe befindet sich im bin Verzeichnis. Dieses Programm wird nun mit folgenden Parametern gestartet: 

    svnserve -d -r C:\SVN\repos

Dadurch wird das Repository C:\SVN\repos aktiviert. 

__Erster Test__

Mit ToroiseSVN lässt sich die Konfiguration sofort überprüfen. Dazu wird ein beliebiges leeres Verzeichnis benötigt (ACHTUNG: keines der verwendeten SVN Verzeichnisse). Mit einem Klick auf „svn checkout“ kann eine Verbindung zum repository hergestellt werden.  
![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualStudio2005subversion_42C7/02CheckOut016.png) 

Als Pfad ist nun `svn://localhost/` anzugeben.  
![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualStudio2005subversion_42C7/02CheckOut024.png) 

Bei aktiviertem Passwortschutz wird man nun aufgefordert, ein zuvor definiertes Benutzerkonto einzugeben. 

TortoiseSVN zeigt daraufhin eine Erfolgsmeldung an. Des Weiteren ist in diesem Verzeichnis ein versteckter Ordner .svn erstellt worden.  
![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualStudio2005subversion_42C7/02CheckOut034.png) 

SVN ist nun einsatzbereit! 

__Visual Studio 2005__

Nach der Ankh Installation müssen noch Programme für Diff und Merge Funktionen konfiguriert werden. Diese sind nicht in Ankh integriert, da es eine Vielzahl von bereits verfügbaren Tools gibt. Ist TortoiseSVN ebenfalls installiert, können die Tortoise Tools dafür festgelegt werden. 

Die Ankh Konfiguration kann mittels Tools\Ankh\Edit the Ankh configuration … geöffnet werden.  
![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualStudio2005subversion_42C7/03AnkhConfig014.png) 

Dort kann ein DiffExePath und ein MergeExePath festgelegt werden. Für Tortoise sind entsprechende Templates vorhanden, die mit der Tastenkombination Strg-T angezeigt werden.

Nun kann ein Projekt in Visual Studio mit „Datei\Add solution to Subversion repository …“ eingecheckt werden.  
![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualStudio2005subversion_42C7/03VsCheckIn4.png) 

Als url ist erneut `svn://localhost/` anzugeben.

Veränderte Dateien sind an einem roten M zu erkennen, unveränderte an einem grünen Haken.  
Änderungen an dem Projekt werden mit dem Commit Befehl an die SVN Datenbank übermittelt.  
![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualStudio2005subversion_42C7/04VsCommit4.png) 

Daraufhin erscheint ein Fenster, das die Auswahl der zu sendenden Dateien ermöglicht. 

Wurde das Projekt von außerhalb (z.B. von einem anderen Entwickler) verändert, kann man die aktuellste Version mit dem Update Befehl von der SVN Datenbank beziehen. 

SVN ist nun vollständig für die Verwendung mit Visual Studio 2005 konfiguriert. 

__SVN als Service__ 

Wird SVN auf einem Server aufgeführt, besteht die Möglichkeit svnserve auch als Windows Service zu starten. Mit folgendem Befhel kann man svnserve als Windows Service registrieren: 

    sc create svnserve binpath= "c:\SVN\bin\svnserve.exe --service -r c:\SVN\repos" displayname= "Subversion" depend= tcpip start= auto

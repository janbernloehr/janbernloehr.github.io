---
layout: post
title : "Visual Studio 2005 + subversion"
date : 02.01.2007 04:45:00
tags: [.NET, Visual Studio 2005]
---
{% include JB / setup %}

<em>Nicht nur für Team Arbeit bietet sich ein SourceControl Provider an. Steht ein Projekt unter SourceControl, verfügt man über ein ständiges BackUp- und Versionierungssystem, durch das Änderungen schnell rückgängig gemacht aber auch gut nachvollzogen und dokumentiert werden können.</em>

<em>Der CVS Nachfolger subversion (im Folgenden SVN genannt) ist eine gute Alternative zu dem etwas in Vergessenheit geratenen Visual SourceSafe.  
Wie für SourceSafe existiert auch für SVN ein Visual Studio PlugIn, wodurch angenehmes Arbeiten ermöglicht wird.</em> 

<em>Dieser Artikel soll zeigen, wie man subversion für die Verwendung mit Visual Studio 2005 konfiguriert.</em> 

<strong>Vorbereitung</strong> 

SVN ist sehr flexibel und benötigt keine Installation im eigentlichen Sinne. Ob auf SVN von einem remote client oder lokal zugegriffen wird spielt lediglich für die Firewall eine Rolle.  
Hier wird eine lokale Installation im Verzeichnis C:\SVN erstellt. 

Zunächst muss SVN selbst heruntergeladen werden ([http://subversion.tigris.org/](http://subversion.tigris.org/)). Die Version 1.4.2 ist zurzeit aktuell. 

Um SVN komfortabel zu verwalten, empfiehlt sich ebenfalls die Installation von TortoiseSVN ([http://tortoisesvn.tigris.org/](http://tortoisesvn.tigris.org/)). 

Letztlich wird noch das Visual Studio Plug-In Ankh benötigt ([http://ankhsvn.tigris.org/](http://ankhsvn.tigris.org/)). 

<b>Konfiguration</b> 

Das subversion .ZIP Archiv wird extrahiert und der Inhalt des bin Verzeichnisses nach C:\SVN\bin kopiert. 

SVN legt Daten in einem speziellen Verzeichnis, einem sog. repository ab. Nach der ToroiseSVN Installation kann ein solches repository im Explorer mit dem Kommando „create repository here“ eingerichtet werden (z.B. in C:\SVN\repos\).
![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualStudio2005subversion_42C7/01CreateRepository12.png) 

In diesem Verzeichnis wird nun die SVN Ordnerstruktur angelegt. Im Unterverzeichnis conf befindet sich die Datei svnserve.conf. Diese ermöglicht eine einfache Konfiguration des SVN repositorys.  
Entfernt man alle # vor einer Zeile, wird diese aktiviert. 

Passwortgeschützter Zugriff wird so aktiviert:

<div class="wlWriterSmartContent" id="57F11A72-B0E5-49c7-9094-E3A15BD5B5E7:b20d08dc-4ad3-4bf0-a571-4e024df7c4ef" contenteditable="false" style="PADDING-RIGHT: 0px; DISPLAY: inline; PADDING-LEFT: 0px; FLOAT: none; PADDING-BOTTOM: 0px; MARGIN: 0px; PADDING-TOP: 0px">

<div><span style="COLOR: #000000">password</span><span style="COLOR: #000000">-</span><span style="COLOR: #000000">db </span><span style="COLOR: #000000">=</span><span style="COLOR: #000000"> passwd
    </span></div>
</div>

Hier kann eine beliebige Bezeichnung gewählt werden:

<div class="wlWriterSmartContent" id="57F11A72-B0E5-49c7-9094-E3A15BD5B5E7:bfbae5df-db07-43d2-a283-5b9b7e83a004" contenteditable="false" style="PADDING-RIGHT: 0px; DISPLAY: inline; PADDING-LEFT: 0px; FLOAT: none; PADDING-BOTTOM: 0px; MARGIN: 0px; PADDING-TOP: 0px">

<div><span style="COLOR: #000000">realm </span><span style="COLOR: #000000">=</span><span style="COLOR: #000000"> My Local Repository</span></div>
</div>

Bestimmt, dass nicht authentifizierte Benutzer keinen Zugriff haben: 

<div class="wlWriterSmartContent" id="57F11A72-B0E5-49c7-9094-E3A15BD5B5E7:94a0220a-dc5c-4f9e-83e8-08706ac11452" contenteditable="false" style="PADDING-RIGHT: 0px; DISPLAY: inline; PADDING-LEFT: 0px; FLOAT: none; PADDING-BOTTOM: 0px; MARGIN: 0px; PADDING-TOP: 0px">

<div><span style="COLOR: #000000">anon</span><span style="COLOR: #000000">-</span><span style="COLOR: #000000">access </span><span style="COLOR: #000000">=</span><span style="COLOR: #000000"> none</span></div>
</div>

Bestimmt, dass authentifizierte Benutzer Schreibrechte haben. 

<div class="wlWriterSmartContent" id="57F11A72-B0E5-49c7-9094-E3A15BD5B5E7:e1bc9378-3e67-4053-9fca-301400b8206c" contenteditable="false" style="PADDING-RIGHT: 0px; DISPLAY: inline; PADDING-LEFT: 0px; FLOAT: none; PADDING-BOTTOM: 0px; MARGIN: 0px; PADDING-TOP: 0px">

<div><span style="COLOR: #000000">auth</span><span style="COLOR: #000000">-</span><span style="COLOR: #000000">access </span><span style="COLOR: #000000">=</span><span style="COLOR: #000000"> write</span></div>
</div>

Für den passwortgeschützten Zugriff muss zusätzlich noch die Datei passwd bearbeitet werden, die die Benutzerkonten enthält. 

Das Schema der Datei lautet. 

<div class="wlWriterSmartContent" id="57F11A72-B0E5-49c7-9094-E3A15BD5B5E7:c19efc1e-d897-4cdf-9983-3b2233ee6fba" contenteditable="false" style="PADDING-RIGHT: 0px; DISPLAY: inline; PADDING-LEFT: 0px; FLOAT: none; PADDING-BOTTOM: 0px; MARGIN: 0px; PADDING-TOP: 0px">

<div><span style="COLOR: #000000">benutzer </span><span style="COLOR: #000000">=</span><span style="COLOR: #000000"> passwort</span></div>
</div>

Ist die Konfiguration abgeschlossen, kann nun der SVN „Server“ gestartet werden.  
Das dafür erforderliche Programm svnserve.exe befindet sich im bin Verzeichnis. Dieses Programm wird nun mit folgenden Parametern gestartet: 

<div class="wlWriterSmartContent" id="57F11A72-B0E5-49c7-9094-E3A15BD5B5E6:a0b20b5d-d3e3-44d3-be0e-e537c63e50f8" contenteditable="false" style="PADDING-RIGHT: 0px; DISPLAY: inline; PADDING-LEFT: 0px; PADDING-BOTTOM: 0px; MARGIN: 0px; PADDING-TOP: 0px">

<div><span style="COLOR: #000000">svnserve </span><span style="COLOR: #000000">-</span><span style="COLOR: #000000">d </span><span style="COLOR: #000000">-</span><span style="COLOR: #000000">r C:\SVN\repos</span></div>
</div>

Dadurch wird das Repository C:\SVN\repos aktiviert. 

<b>Erster Test</b> 

Mit ToroiseSVN lässt sich die Konfiguration sofort überprüfen. Dazu wird ein beliebiges leeres Verzeichnis benötigt (ACHTUNG: keines der verwendeten SVN Verzeichnisse). Mit einem Klick auf „svn checkout“ kann eine Verbindung zum repository hergestellt werden.  
![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualStudio2005subversion_42C7/02CheckOut016.png) 

Als Pfad ist nun svn://localhost/ anzugeben.  
![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualStudio2005subversion_42C7/02CheckOut024.png) 

Bei aktiviertem Passwortschutz wird man nun aufgefordert, ein zuvor definiertes Benutzerkonto einzugeben. 

TortoiseSVN zeigt daraufhin eine Erfolgsmeldung an. Des Weiteren ist in diesem Verzeichnis ein versteckter Ordner .svn erstellt worden.  
![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualStudio2005subversion_42C7/02CheckOut034.png) 

SVN ist nun einsatzbereit! 

<b>Visual Studio 2005</b> 

Nach der Ankh Installation müssen noch Programme für Diff und Merge Funktionen konfiguriert werden. Diese sind nicht in Ankh integriert, da es eine Vielzahl von bereits verfügbaren Tools gibt. Ist TortoiseSVN ebenfalls installiert, können die Tortoise Tools dafür festgelegt werden. 

Die Ankh Konfiguration kann mittels Tools\Ankh\Edit the Ankh configuration … geöffnet werden.  
![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualStudio2005subversion_42C7/03AnkhConfig014.png) 

Dort kann ein DiffExePath und ein MergeExePath festgelegt werden. Für Tortoise sind entsprechende Templates vorhanden, die mit der Tastenkombination Strg-T angezeigt werden.

Nun kann ein Projekt in Visual Studio mit „Datei\Add solution to Subversion repository …“ eingecheckt werden.  
![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualStudio2005subversion_42C7/03VsCheckIn4.png) 

Als url ist erneut svn://localhost/ anzugeben.

Veränderte Dateien sind an einem roten M zu erkennen, unveränderte an einem grünen Haken.  
Änderungen an dem Projekt werden mit dem Commit Befehl an die SVN Datenbank übermittelt.  
![](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/VisualStudio2005subversion_42C7/04VsCommit4.png) 

Daraufhin erscheint ein Fenster, das die Auswahl der zu sendenden Dateien ermöglicht. 

Wurde das Projekt von außerhalb (z.B. von einem anderen Entwickler) verändert, kann man die aktuellste Version mit dem Update Befehl von der SVN Datenbank beziehen. 

SVN ist nun vollständig für die Verwendung mit Visual Studio 2005 konfiguriert. 

<strong>SVN als Service</strong> 

Wird SVN auf einem Server aufgeführt, besteht die Möglichkeit svnserve auch als Windows Service zu starten. Mit folgendem Befhel kann man svnserve als Windows Service registrieren: 

<div class="wlWriterSmartContent" id="57F11A72-B0E5-49c7-9094-E3A15BD5B5E7:e2e2486c-4691-4785-a631-8f9ff16e3f4d" contenteditable="false" style="PADDING-RIGHT: 0px; DISPLAY: inline; PADDING-LEFT: 0px; FLOAT: none; PADDING-BOTTOM: 0px; MARGIN: 0px; PADDING-TOP: 0px">

<div><span style="COLOR: #000000">sc create svnserve binpath</span><span style="COLOR: #000000">=</span><span style="COLOR: #000000"> </span><span style="COLOR: #000000">"</span><span style="COLOR: #000000">c:\SVN\bin\svnserve.exe --service -r c:\SVN\repos</span><span style="COLOR: #000000">"</span><span style="COLOR: #000000"> displayname</span><span style="COLOR: #000000">= </span><span style="COLOR: #000000">"</span><span style="COLOR: #000000">Subversion</span><span style="COLOR: #000000">"</span><span style="COLOR: #000000"> depend</span><span style="COLOR: #000000">=</span><span style="COLOR: #000000"> tcpip start</span><span style="COLOR: #000000">=</span><span style="COLOR: #000000"> auto
    </span></div>
</div>

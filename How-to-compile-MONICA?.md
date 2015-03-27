# Windows

##Notwendige Software

* VisualStudio (2013) Express
* Boost (Version 1.53.0)
* MySQLConnector C (6.0.2) 

**Hinweise:**

* Da die Datenbank, in der die EVA-Daten verwaltet werden, zu alt ist, muss der MySQL Connector C in der Version 6.0.2 verwendet werden. Bei der Verwendung von 6.1 erscheint eine Fehlermeldung und SQL-Abfragen an die EVA-Datenbank sind nicht möglich.

* MariaDB als Ersatz zum MySQL Connector C kann ebenfalls nicht verwendet werden, weil unsere Datenbank zu alt ist. Es erscheint eine irreführende Fehlermeldung: "Cannot connect twice. Already Connected". Dies ist dann der Hinweis, dass MariaDB nicht mit unser alten Datenbank kommunizieren kann.

* Wenn die Umstellung auf die Zentrale EVA-Datenbank aus Gießen erfolgt, kann diese Einschränkung obsolet sein, weil die neue Datenbank wahrscheinlich mit einem aktuelleren System umgesetzt wird.

### Notwendige Software um die EVA-Simulationsskripte zu nutzen

Um die EVA-Skripte zur Ausführung von MONICA zu nutzen, muss MONICA als Python-Bibliothek gebaut werden. Dies erfolgt durch das Compilieren von monica-swig.sln. Dazu ist die Installation folgender Softwarepakete erforderlich:

* Python (2.7.x)
* Numpy
* MysqlDb? 
* R
* swigwin 

Der Pfad zu R, Python und zur DLL vom MySQLConnector C muss in der PATH Umgebungsvariable gesetzt sein. Sonst erscheinen Fehler bei der Ausführung von MONICA aus den Python-SKripten heraus. 

Das swigwin-Verzeichnis muss in dem Verzeichnis abgespeichert werden, in dem sich auch die anderen Repositories befinden wie z.B: monica oder eva-project. 

Wird eine neuere swig Version als 2.10.0 verwendet so muss zusätzlich in der monica-swig.vcxproj Datei der Pfad zur swig.exe angepasst werden. Dazu muss die Datei monica-swig.vcxproj in einem Texteditor (z.B. notepad++) geöffnet werden und dann im Texteditor der Pfad angepasst werden.

Hinweis: MONICA muss in der swig-Variante als Release gebaut werden, da sonst der swig-Build-step nicht ausgeführt wird.

# Linux / Ubuntu

Diese Installation wurde unter Ubuntu 10.4 LTS getestet. Falls diese Dokumentation Fehler enthält, bitte selbständig im Wiki korrigieren.

## Notwendige Softwarepakete

* GCC: Der Compiler wird zum Übersetzen des Quellcodes benötigt; Installation: sudo apt-get install gcc build-essential 
* Git: Verteiltes Codeverwaltungssystem, wird zum aus-checken des Quellcodes benötigt; Installation: sudo apt-get install git-core 
* Qt4: Wird hauptsächlich als Makefile-Generator verwendet; MONICA ist nicht direkt abhängig von Qt; Installation: Von der Webseite  http://qt.nokia.com/downloads herunterladen und installieren (LGPL-Version) getestete Version: qtsdk-2010.02
* Boost: Bibliothek wird für die Übersetzung des Quellcodes benötigt; Download und Entpacken der Bibliothek (Version 1.53.0) ins übergeordnete Projektverzeichnis (../monica)
* MySQL: Zur Anbindung an diverse MySQL-Datenbanken mittels MONICA benötigt; Installation: sudo apt-get install libmysqlclient15-dev 

**Hinweise:**

Folgende Einträge müssen in der .bashrc gesetzt werden, damit qmake und die Qt-Bibliotheken korrekt gefunden werden können:

``export QTDIR=/home/user/pfad_zum_QT_installationsverzeichnis``

``export PATH=$QTDIR/bin:$QTDIR/qt/bin:$PATH``

``export LD_LIBRARY_PATH=$QTDIR/lib:$LD_LIBRARY_PATH``

    

## Optionale Software

* Eclipse: Freie Entwicklungsumgebung auch für C++-Entwicklung; Installation: siehe Installation von Eclipse 
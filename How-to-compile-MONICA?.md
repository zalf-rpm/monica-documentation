# Windows

##Notwendige Software

* VisualStudio (2013) Express
* Boost (Version 1.53.0)
* MySQLConnector C (6.0.2) 

**Hinweise:**

* Da die Datenbank, in der die EVA-Daten verwaltet werden, zu alt ist, muss der MySQL Connector C in der Version 6.0.2 verwendet werden. Bei der Verwendung von 6.1 erscheint eine Fehlermeldung und SQL-Abfragen an die EVA-Datenbank sind nicht möglich.

* MariaDB als Ersatz zum MySQL Connector C kann ebenfalls nicht verwendet werden, weil unsere Datenbank zu alt ist. Es erscheint eine irreführende Fehlermeldung: "Cannot connect twice. Already Connected". Dies ist dann der Hinweis, dass MariaDB nicht mit unser alten Datenbank kommunizieren kann.

* Wenn die Umstellung auf die Zentrale EVA-Datenbank aus Gießen erfolgt, kann diese Einschränkung obsolet sein, weil die neue Datenbank wahrscheinlich mit einem aktuelleren System umgesetzt wird.

## Notwendige Software um die EVA-Simulationsskripte zu nutzen

Um die EVA-Skripte zur Ausführung von MONICA zu nutzen, muss MONICA als Python-Bibliothek gebaut werden. Dies erfolgt durch das Compilieren von monica-swig.sln. Dazu ist die Installation folgender Softwarepakete erforderlich:

* Python (2.7.x)
* Numpy
* MysqlDb? 
* R
* swigwin 

Der Pfad zu R, Python und zur DLL vom MySQLConnector C muss in der PATH Umgebungsvariable gesetzt sein. Sonst erscheinen Fehler bei der Ausführung von MONICA aus den Python-SKripten heraus. 
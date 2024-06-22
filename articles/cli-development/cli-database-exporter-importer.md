<!-- Filename: J4.x:CLI_Database_Exporter_Importer / Display title:  CLI Datenbank Export / Import -->

Joomla!  4.0

## Über

Vor einem Joomla! Update oder [Installation einer
Drittanbietererweiterung](https://docs.joomla.org/J4.x:CLI_Update "Special:MyLanguage/J4.x:CLI Update")
wird ein Backup der Internetseite dringend empfohlen. Die Joomla! 4.x
Konsole bietet Kommandos für den Export (Backup) und Import
(Wiederherstellung) der Joomla! Datenbank. Hinweis: Das Dateisystem wird
dabei nicht gesichert, dies sollte separat erfolgen.

## Voraussetzungen

Um diese Kommandos verwenden zu können, benötigst du / benötigen Sie
einen sicheren Konsolenzugang (SSH) zum Host auf dem das PHP CLI
(Kommandozeileninterface) installiert ist. Bedenken Sie / Bedenke, dass
ein grundlegendes Wissen über die Verwendung von Konsolenkommandos nötig
ist.

## Anleitung

Bitte am Hostrechner anmelden und das Wurzelverzeichnis der Website
aufrufen. Es wird empfohlen das Joomla! „tmp“-Verzeichnis im zu
verwenden, um über Lese-/Schreibrechte zu verfügen.

- Alle verfügbaren Kommandos der Joomla! Konsole auflisten:
  `php cli/joomla.php list`
- Die Datenbank in ein Verzeichnis exportieren:
  `php cli/joomla.php database:export --all --folder `
- Die Datenbank aus einem Verzeichnis importieren:
  `php cli/joomla.php database:import --all --folder `

Alternativ kann man auch:

- Die Datenbank als .zip-Datei exportieren:  
  `php cli/joomla.php database:export --all --zip`
- Eine Tabelle exportieren:  
  `php cli/joomla.php database:export --table `
- Eine Tabelle als .zip-Datei exportieren:  
  `php cli/joomla.php database:export --table --zip`
- Eine Tabelle importieren:  
  `php cli/joomla.php database:import --table `
- Falls Hilfe benötigt wird:  
  `php cli/joomla.php database:export --help`
  `php cli/joomla.php database:import --help`

## Sichern und Wiederherstellen

Um ein vollständiges Backup (mit Ordnern, Dateien und Datenbank) der
Site zu erstellen, können diese Befehle ausgeführt werden:

1.  Das Joomla-Root-Verzeichnis archivieren:  
    `tar --exclude='./tmp/joomla_bak.*' -zcvf tmp/joomla_bak.tgz . > tmp/joomla_bak.log`
2.  Die gesamte Joomla-Datenbank exportieren:  
    `php cli/joomla.php database:export --all --folder tmp/db_bak`

Und zum Wiederherstellen diese Befehle ausführen:

1.  Die gesamte Joomla-Datenbank importieren:  
    `php cli/joomla.php database:import --all --folder tmp/db_bak`
2.  Die Archiv-Datei entpacken:  
    `tar --recursive-unlink -xvf tmp/joomla_bak.tgz .`

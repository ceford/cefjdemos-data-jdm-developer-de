<!-- Filename: J4.x:Developer:_Required_Software / Display title: Joomla installieren -->

## Zusammenfassung

Dies ist nur eine kurze Zusammenfassung der beteiligten Schritte:

- **Laden Sie** die neueste Vollversion von der [Joomla Downloads](https://downloads.joomla.org/) Seite herunter.
- **Speichern** Sie die heruntergeladene Datei in Ihrem Dokumenten-Root oder einem Unterordner des Roots.
- **Entpacken** Sie die heruntergeladene Datei an Ort und Stelle. Einige Betriebssysteme erstellen einen Ordner mit demselben Namen wie die heruntergeladene Zip-Datei, abzüglich des Zip. Das ist gut - Sie können den Ordner einfach umbenennen und bei Bedarf verschieben. Andere Betriebssysteme entpacken die Dateien und Ordner direkt an Ort und Stelle. Das ist schlecht, wenn Sie die Datei in Ihrem Root-Ordner abgelegt haben und dort bereits Ordner mit anderen Seiten existieren. Sie müssen einen kurzen Ordnernamen erstellen und alle neu extrahierten Dateien und Ordner dorthin verschieben. Das Ziel ist es, mit einem Ordner zu enden, auf den Sie über Ihren Webbrowser über localhost/j4test zugreifen können.
- **Installieren** Sie, indem Sie Ihren Browser auf localhost/j4test richten und die Joomla-Installationsformulare ausfüllen.

## Konfiguration

Einige Vorschläge:

- **Globale Konfiguration / Site / Cookie / Cookie-Pfad** auf den Ordner der Ihre Joomla-Installation enthält, gesetzt (/j4test).
- **Globale Konfiguration / System / Debug / Debug-System** auf Ja gesetzt.
- **Globale Konfiguration / System / Sitzungslaufzeit** auf 60 gesetzt - ansonsten werden Sie beim Nachdenken ausgeloggt.
- **Globale Konfiguration / Server / Server / Fehlerberichterstattung** auf Maximum gesetzt.
- **Plugin System - Debug / Plugin / Assets aktualisieren** auf Nein gesetzt, es sei denn, Sie debuggen CSS oder JavaScript.

Das ist alles. Wenn Joomla funktioniert, sind Sie bereit, es für die Entwicklung von Erweiterungen zu verwenden.

## Weitere Beiträge

Sie können beliebig viele Seiten auf einem einzigen Computer installieren. Je nachdem, wie Sie Ihren Server eingerichtet haben, kann dies über verschiedene Unterordner sein, auf die Sie über localhost/test1, localhost/test2 und so weiter zugreifen, oder über virtuelle Hosts, auf die Sie über test1.localhost, test2.localhost und so weiter zugreifen. Auf jeden Fall können Sie in wenigen Minuten eine neue Datenbank und eine neue saubere Joomla-Seite haben. Wählen Sie aussagekräftige kurze Namen! *test1* und *test2* werden bald verwirrend.

## Installation für Tests

Es gibt ein anderes Verfahren, wenn du bei der Joomla-Testung helfen möchtest. In diesem Fall solltest du **git** installieren und den Anweisungen im [GitHub Repository](https://github.com/joomla/joomla-cms) folgen.

- In GitHub die Joomla-Branch auswählen, an der Sie arbeiten möchten. Das ändert die weiter unten auf der Seite angezeigten Anweisungen.
- Auf Ihrem Laptop oder Desktop den Anweisungen ab *Repository klonen:* folgen.
- Das Installationsverzeichnis am Ende der Joomla-Installation nicht löschen!
- Den Klon von joomla-cms in etwas anderes umbenennen, damit Sie alles später erneut durchführen können.
- Den Joomla Patchtester installieren.

*Übersetzt von openai.com*


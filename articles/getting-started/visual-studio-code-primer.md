<!-- Filename: Visual_Studio_Code_Primer / Display title: Visual Studio Code -->

## VS Code - Eine beliebte kostenlose IDE

Von [Wikipedia](https://de.wikipedia.org/wiki/Visual_Studio_Code):

> Visual Studio Code, auch allgemein als VS Code bezeichnet, ist ein
> Quellcode-Editor von Microsoft für Windows, Linux und macOS.
> Zu den Funktionen gehören Unterstützung für Debugging, Syntaxhervorhebung,
> intelligente Code-Vervollständigung, Snippets, Code-Refaktorisierung und integriertes
> Git. Benutzer können das Theme, die Tastenkombinationen, die Einstellungen ändern und
> Erweiterungen installieren, die zusätzliche Funktionalität hinzufügen.

## Installation

Die Standardseite der [VS Code](https://code.visualstudio.com/) Website hat eine Dropdown-Liste für jede unterstützte Plattform. Es ist wahrscheinlich, dass Ihre Plattform vorausgewählt ist. Also laden Sie herunter und installieren Sie, und Sie sind startklar.

### Erste Schritte

Die VS Code *Loslegen*-Seite enthält einige *Start*-Elemente, eine Liste von *Kürzlich* verwendeten Elementen und eine kurze Liste von *Durchgängen*. Wenn Sie völlig neu in VS Code sind, wird empfohlen, diese anzusehen. Sie dauern nur ein paar Minuten.

Die VS Code-Dokumentation ist im Menü *Hilfe / Dokumentation* verfügbar. Die Einführungsvideos sind es wert, angesehen zu werden. Jedes dauert 2 bis 6 Minuten und bietet eine ausgezeichnete Einführung in die VS Code-Funktionen:

[VS Code Videos](https://code.visualstudio.com/docs/getstarted/introvideos)

Die offizielle Dokumentation ist der richtige Ort, wenn Sie spezifische Informationen nachschlagen möchten.

### VS Code-Erweiterungen

VS Code kann für jede Art von Text verwendet werden, einschließlich einer Vielzahl von Programmiersprachen. Es funktioniert mit JavaScript, ohne Erweiterungen hinzuzufügen. Andere Sprachen werden anhand des Kontexts erkannt, sodass Sie voraussichtlich aufgefordert werden, ein PHP-Supportpaket zu installieren, wenn Sie beginnen, PHP-Code zu erstellen und zu speichern.

Klicken Sie auf das Symbol *Erweiterungen* in der linken *Aktivitätsleiste*, um zu sehen, was Sie installiert haben und was empfohlen wird. Sie benötigen die PHP-Debug-Erweiterung!

### Das VS Code-Bildschirm-Layout

Einige Begriffe, die in den nachfolgenden Anweisungen verwendet werden:

- **Aktivitätsleiste:** die schmale Leiste auf der linken Seite des Bildschirms. Wählen Sie ein beliebiges Symbol aus, um die primäre Seitenleiste zu öffnen oder zu schließen.
- **Primäre Seitenleiste:** zeigt bei geöffneter Leiste Details der ausgewählten Aktivität an.
- **Statusleiste:** am unteren Rand des Bildschirms. Sie zeigt an, was vor sich geht.
- **Panel:** ein Bereich unterhalb der Texteditoren, um andere Informationen anzuzeigen.

Wählen Sie ein Layout-Symbol oben rechts aus, um eines dieser Elemente zu öffnen oder zu schließen.

## Codierung einer Joomla-Erweiterung

Um eine Erweiterung zu erstellen, ist es Ihr Ziel, eine ZIP-Datei zu erstellen, die Sie in einer funktionierenden Joomla-Website installieren können. Sie benötigen also einen Ordner, um Ihren Code zu speichern. Dieser sollte sich in Ihrem persönlichen Dateibereich auf Ihrem Laptop oder Desktop-Computer befinden, der für die lokale Entwicklung verwendet wird. Er sollte nicht in Ihrem Website-Baum sein. Zum Beispiel könnten Sie *~/jextensions* verwenden, um Unterordner für verschiedene Erweiterungen zu enthalten. Ich benutze *~/git*, weil es kurz und einfach zu buchstabieren ist, obwohl es potenziell verwirrend ist, da jedes Unterverzeichnis ein separates git-Repository verwendet.

### Beispielcode

Wenn Sie einige Beispielcodes zum Bearbeiten wünschen, gibt es eine Erweiterung auf GitHub mit dem Namen *mod_debugme*. Wie der Name schon andeutet, handelt es sich um ein Modul mit einigen Fehlern. Zusätzlich zum Modulcode gibt es eine *build.xml*-Datei, die eine Möglichkeit veranschaulicht, den Build-Prozess für Tests zu automatisieren und eine Zip-Datei zu erstellen.

Das Modul ist so konzipiert, dass es die nächsten paar (standardmäßig 3) Ereignisse (Geburtstage) aus einer Liste anzeigt, die in einer Datenbanktabelle gespeichert ist. Man könnte sich vorstellen, dass dies auf einer Büro- oder Familienseite in Erwartung von Kuchen verwendet wird.

Es ist möglicherweise am besten, mit Git-Befehlen über die Befehlszeile zu beginnen. Erstelle zuerst einen Ordner für deinen Code und klone dann das entfernte Repository:

```sh
    mkdir ~/git
    cd ~/git
    git clone https://github.com/ceford/j4xdemos-mod-debugme
```

Die Antwort sollte nur wenige Sekunden dauern:

```sh
    Cloning into 'j4xdemos-mod-debugme'...
    remote: Enumerating objects: 23, done.
    remote: Counting objects: 100% (23/23), done.
    remote: Compressing objects: 100% (16/16), done.
    remote: Total 23 (delta 3), reused 23 (delta 3), pack-reused 0
    Unpacking objects: 100% (23/23), done.
```

Nehmen Sie sich einen Moment Zeit, um den Inhalt des Ordners anzusehen:

```sh
    cd j4xdemos-mod-debugme
    ls -al
    total 16
    drwxr-xr-x   6 ceford  staff   192  2 Sep 17:48 .
    drwxr-xr-x   3 ceford  staff    96  2 Sep 17:48 ..
    drwxr-xr-x  12 ceford  staff   384  2 Sep 17:48 .git
    -rw-r--r--   1 ceford  staff  1402  2 Sep 17:48 README.md
    -rw-r--r--   1 ceford  staff   927  2 Sep 17:48 build.xml
    drwxr-xr-x   8 ceford  staff   256  2 Sep 17:48 mod_debugme
```

Der *.git*-Ordner enthält Informationen über das Repository. Die *README.md*-Datei ist ein Markdowndokument, das dieses Repository beschreibt. Die *build.xml*-Datei ist eine Datei, die verwendet wird, um die Erweiterung mit einem externen Tool, Phing - später beschrieben, zu erstellen. Der *mod_debugme*-Ordner enthält den Code der Erweiterung.

### In Joomla installieren

Komprimieren Sie den Erweiterungsordner, um eine installierbare ZIP-Datei zu erstellen:

```sh
    zip -r mod_debugme.zip mod_debugme
```

Sie können nun die ZIP-Datei auf der Joomla-Seite installieren, die Sie zum Testen verwenden. Nach der Installation müssen Sie ein Site-Modul erstellen und es einer Modulposition zuweisen. Da es sich um ein kaputtes Modul handelt, könnten Sie es einer Position auf *Allen Seiten* zuweisen, während Sie daran arbeiten; oder Sie könnten es einer Position auf einer einzelnen Seite zuweisen; oder Sie könnten es in einem Beitrag positionieren, der einen eigenen Menüpunkt hat.

Nach der Installation die Zip-Datei löschen.

### Debugmodus aktivieren

In der globalen Konfiguration von Joomla setzen Sie *Debug-System* auf *Ja* und *Fehlerberichterstattung* auf *Maximum*.

Wenn Sie eine Seite mit dem fehlerhaften Modul öffnen, sehen Sie eine Stapelverfolgung, die Ihnen zeigt, wo ein Fehler ausgelöst wurde.

![Stack-Trace](../../../en/images/getting-started/vscode-primer-stack-trace.png)

Manchmal befindet sich der Programmierfehler in der ersten Zeile der Stacktrace. Andernfalls, wenn der Fehler im Bibliothekscode ausgelöst wird, zum Beispiel durch das Übergeben ungültiger Daten an eine Datenbankfunktion, kann der Programmierfehler weiter unten in der Liste der Funktionsaufrufe liegen.

## Erweiterungsordner in VS Code öffnen

In VS Code verwenden Sie den Menüpunkt Datei / Ordner öffnen, um den Ordner zu finden und zu öffnen, der Ihre lokale Kopie des *mod_debugme*-Erweiterungscodes enthält. Sie sollten etwas Ähnliches wie das Folgende sehen:

![VS Code Bildschirm](../../../en/images/getting-started/vscode-primer-screen.png)

Möglicherweise können Sie das Problem alleine durch Lesen des Codes diagnostizieren. Im Fall des Fehlers *Klasse "DebugHelper" nicht gefunden* werden Sie sehen, dass eine *use*-Anweisung ein paar Zeilen weiter oben auskommentiert wurde. Das Vergessen, eine *use*-Anweisung einzufügen, ist ein häufiger Fehler während der anfänglichen Entwicklung!

Beheben Sie das Problem und komprimieren und installieren Sie dann das Modul erneut. Dieser Schritt wird etwas mühsam, wenn Sie mehrere Probleme haben. An dieser Stelle sind Build-Tools nützlich.

## Das Phing-Build-Tool

Phing ist ein Befehlszeilen-Tool, das für alle Plattformen verfügbar ist und zur Erstellung von Softwarepaketen verwendet wird. Die Anweisungen werden in einer xml-Datei gespeichert, die standardmäßig build.xml genannt wird. Bei der Arbeit mit Erweiterungscode kann es für zwei Dinge verwendet werden:

- Kopiere die geänderten Dateien aus deinem Erweiterungsquellordner an die richtigen Stellen in deinem Website-Ordner.
- Erzeuge eine neue ZIP-Datei für neue Installationen.

Laden Sie [Phing](https://www.phing.info/) herunter und installieren Sie es. Andere Build-Tools sind verfügbar! Sie könnten Phing in Ihrem eigenen Bin-Verzeichnis oder in einem System-Bin-Verzeichnis installieren. Sie müssen sich den Pfad zu Ihrem Phing-Code notieren. In diesem Beispiel ist es *~/bin/phing-latest.phar*. Sie können es von der Kommandozeile aus ausprobieren, nachdem Sie in das Verzeichnis mit Ihrem Erweiterungscode gewechselt sind:

```sh
    cd ~/git/j4xdemos-mod-debugme
    php ~/bin/phing-latest.phar
```

Antwort:

```sh
    Buildfile: /Users/ceford/git/j4xdemos-mod-debugme/build.xml

    mod_debugme > main:
          ... Any copied files will be mentioned here
          [zip] Building zip: /Users/ceford/zips/mod_debugme.zip

    BUILD FINISHED

    Total time: 0.0863 seconds
```

## VS Code-Aufgaben

Um Phing innerhalb von VS Code auszuführen, müssen Sie eine *tasks.json* Datei im *.vscode* Ordner im Stammverzeichnis des *j4xdemos-mod-debugme* Ordners erstellen. Falls letzterer nicht existiert, erstellen Sie ihn zuerst. Erstellen Sie dann die *tasks.json* Datei und geben Sie den folgenden Code ein:

```json
    {
        // See https://go.microsoft.com/fwlink/?LinkId=733558
        // for the documentation about the tasks.json format
        "version": "2.0.0",
        "tasks": [
          {
            "label": "Build mod_debugme",
            "type": "shell",
            "command": "php ~/bin/phing-latest.phar",
            "windows": {
              "command": "php ~/bin/phing-latest.phar"
            },
            "group": "build",
            "presentation": {
              "reveal": "always",
              "panel": "shared"
            }
          }
        ]
    }
```

Windows-Nutzer müssen den Windows-spezifischen Befehl korrigieren. Sie können nun die Erweiterung über das Menü *Terminal / Build-Aufgabe ausführen* erstellen. Das Ergebnis des Befehls sollte im Terminal-Panel unterhalb des Bearbeitungsbereichs erscheinen.

```sh
      *  Executing task: php ~/bin/phing-latest.phar

    Buildfile: /Users/ceford/git/gitdemo/j4xdemos-mod-debugme/build.xml

    mod_debugme > main:

          [zip] Nothing to do: /Users/ceford/zips/mod_debugme.zip is up to date.

    BUILD FINISHED

    Total time: 0.1031 seconds

     *  Terminal will be reused by tasks, press any key to close it.
```

Es können unverständliche Fehlermeldungen auftreten. Die wahrscheinlichste Ursache ist, dass ungültige Pfade zu Ordnern in der *build.xml* Datei angegeben sind oder ein Ordner nicht erstellt wurde. Nur eine weitere Art von Problem zum Debuggen!

## Debuggen

Sie sollten in der Lage sein, den ersten Fehler durch Code-Inspektion zu beheben. Kompliziertere Probleme erfordern, dass Sie den Code mit dem Debugger durchgehen. Das ermöglicht es Ihnen, Variablen zu überprüfen, um zu sehen, ob sie die erwarteten Werte enthalten, zum Beispiel beim Übergeben von Argumenten an Bibliotheksfunktionen.

### *php.ini* Einstellungen

Um das Debugging mit Xdebug einzurichten, müssen Sie einige Einträge am Anfang Ihrer *php.ini* Datei vornehmen.

```sh
    zend_extension="xdebug.so"
    xdebug.mode="debug"
    xdebug.client_port=9003
    xdebug.start_with_request=yes
    xdebug.log_level=0
```

Nach dem Speichern aller Änderungen starten Sie Ihren Apache-Server neu.

### Website-Fenster hinzufügen

Ihr Erweiterungsordner enthält nur wenige Dateien, die ***Quellen*** der in Ihrer Website installierten Dateien. Das Debuggen zur Laufzeit erfordert das Setzen von Haltepunkten in Ihren ***Website***-Dateien, daher benötigen Sie Zugriff auf diese Dateien. Sie könnten das Menü *Datei / Ordner zum Arbeitsbereich hinzufügen...* verwenden, um den Website-Ordner zu Ihrem Arbeitsbereich hinzuzufügen. Es besteht jedoch eine sehr große Wahrscheinlichkeit, dass Sie am Ende Änderungen an den Website-Dateien anstelle der Quelldateien vornehmen. Daher ist es wahrscheinlich am besten, ein separates VS Code-Fenster zum Debuggen zu öffnen.

- **Neues Fenster öffnen:** Wählen Sie im VS Code-Menü *Datei / Neues Fenster* und wählen Sie den Ordner aus, der Ihre Joomla-Website enthält.
- **Ordner öffnen:** Wählen Sie im neu geöffneten Fenster *Datei / Ordner öffnen...* aus dem VS Code-Menü. Suchen Sie Ihren Website-Ordner und wählen Sie ihn aus. Sie sollten eine Liste aller Dateien Ihrer Joomla-Website in der primären Seitenleiste sehen.

### Startkonfiguration

Um das Debuggen in VS Code tatsächlich zum Laufen zu bringen, benötigen Sie eine Startkonfiguration. Erstellen Sie im Stammverzeichnis Ihrer Website einen Ordner namens *.vscode* (beachten Sie den führenden Punkt), der eine Datei namens *launch.json* mit folgendem Inhalt enthält:

```json
    {
        "configurations": [
            {
                "name": "Listen for XDebug",
                "type": "php",
                "request": "launch",
                "hostname": "0.0.0.0",
                "port": 9003,
                "stopOnEntry": true,
                "pathMappings": {
                    "/Users/ceford/Sites/j421rc2": "${workspaceFolder}"
                }
            }
        ]
    }
```

Erinnern Sie sich daran, das Element pathMappings in diesem Beispiel durch die tatsächlichen pathMappings auf Ihrer eigenen Website zu ersetzen. Das Element stopOnEntry wird die Ausführung an der allerersten Zeile des ausgeführten PHP-Codes anhalten.

### Debug *mod_debugme*

Jetzt sind Sie bereit, die Fehler im installierten Modul zu finden und zu beheben.

- **Modulcode finden:** Finden Sie den ersten Fehler in Zeile 16 von JROOT/modules/mod_debugme/mod_debugme.php.
- **Haltepunkt setzen:** Klicken Sie in den Raum links von der Zahl 16. Ein heller roter Punkt erscheint beim Schweben und wird voll rot, nachdem Sie geklickt haben, um anzuzeigen, dass ein Haltepunkt gesetzt wurde.
- **Debuggen starten:** Wählen Sie im VS Code-Menü *Run / Start Debugging* aus. Laden Sie Ihre Seite in Ihrem Browser neu. Ihr VS Code-Fenster sollte mit dem Code, der an der ersten Zeile der Datei *index.php* der Seite gestoppt ist, wieder angezeigt werden. Am oberen Rand des Bildschirms befinden sich einige Symbole zur Steuerung des Debug-Prozesses. Diese sollten selbsterklärend sein. Falls nicht, sehen Sie im VS Code Hilfe / Dokumentation nach.
- **Fortsetzen:** Wählen Sie die Fortsetzen-Schaltfläche - der Code läuft bis zu Ihrem ersten Haltepunkt. Untersuchen Sie den Code, um das Problem zu sehen.
- **Schweben:** Wenn Sie über eine Variable schweben, der ein Wert zugewiesen wurde, erscheint eine kleine Tooltip, die die Attribute dieser Variablen zusammenfasst. Es gibt keinen Tooltip für Variablen, denen keine Werte zugewiesen wurden.
- **Variablen:** Die linke Spalte enthält mehr Informationen über den Zustand des Codes am Haltepunkt. Es gibt zu viele, um sie hier alle zu behandeln. Erkunden Sie sie nach Bedarf!
- **Debugging stoppen:** Es ist wahrscheinlich am besten, das Fortsetzen-Symbol auszuwählen, andernfalls wird die Webseite leer geliefert. Ansonsten könnten Sie die Stop-Schaltfläche oder das Menü Run / Stop Debugging verwenden.

### Einen Fehler beheben

**Erinnere dich:** Behebe den Fehler nicht im Website-Code! Behebe ihn im Quellcode!

Korrigieren Sie den Quellcode und verwenden Sie dann *Terminal / Run Build Task...*.

Neustart-Debug.

### Tipps

Einige weniger offensichtliche Probleme:

- Sie beheben den ersten Fehler, aber es stürzt weiterhin an dieser Zeile ab. Schauen Sie in mod_debugme.xml nach, wo der src der Namespaced-Klassen definiert ist. Wenn es in der Quelle behoben ist, müssen Sie die ZIP-Datei neu installieren oder die Datei *administrator/cache/autoload_psr4.php* löschen. Wenn diese fehlt, erstellt Joomla diese Datei aus den Manifest-Dateien neu. Aber wenn ein falscher oder fehlender Eintrag vorhanden ist, wird das Problem nicht behoben, bis die Erweiterung neu installiert wird.
- Manchmal müssen Sie einen Haltepunkt einige Zeilen vor der Zeile setzen, in der der Fehler auftritt, insbesondere wenn Sie die an Funktionsaufrufe übergebenen Werte überprüfen möchten.
- Tabelle *xxx.yyy\\debugme* existiert nicht. Ah ja, der Code zum Erstellen einer Tabelle bei der Installation und zum Entfernen bei der Deinstallation wurde nie erstellt. Sie müssen eine SQL-Abfrage in phpMyAdmin mit dem Inhalt der Datei *mod\\debugme.sql* ausführen. Denken Sie daran, *\#\\* in den Tabellennamen durch Ihr Datenbankpräfix zu ersetzen. Und wenn es immer noch fehlschlägt, überprüfen Sie den Tabellennamen im Code.

## Screenshot

Wenn alles behoben ist, sehen Sie möglicherweise Folgendes:

![Seitenansicht des funktionierenden, debugged Moduls](../../../en/images/getting-started/vscode-primer-debugme-fixed.png)

Kuchentage?

## Literaturverzeichnis

Aus der Joomla! Dokumentation: [Visual Studio Code](https://docs.joomla.org/Visual_Studio_Code), das auch die Konfiguration anderer Werkzeuge abdeckt, zum Beispiel CodeSniffer und PHPUnit.

*Übersetzt von openai.com*

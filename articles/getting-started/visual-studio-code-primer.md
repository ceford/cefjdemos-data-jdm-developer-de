<!-- Filename: Visual_Studio_Code_Primer / Display title: Einführung in Visual Studio-Code -->

## VS Code - Eine beliebte kostenlose IDE

Von <a href="https://en.wikipedia.org/wiki/Visual_Studio_Code"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">Wikipedia</a>:

> Visual Studio Code, allgemein auch als VS Code bezeichnet, ist ein
> Quellcode-Editor von Microsoft für Windows, Linux und macOS. Zu den
> Funktionen gehören Unterstützung für Debugging, Syntaxhervorhebung,
> intelligente Codevervollständigung, Snippets, Code-Refactoring und
> eingebettetes Git. Benutzer können das Design, Tastaturkürzel und
> Einstellungen ändern und Erweiterungen installieren, die zusätzliche
> Funktionen hinzufügen.

## Installation

Die Standardseite der Website
<a href="https://code.visualstudio.com/" class="external text"
target="_blank" rel="nofollow noreferrer noopener">VS Code</a> enthält
eine Dropdown-Liste für jede unterstützte Plattform. Die Chancen stehen
gut, dass Ihre Plattform vorausgewählt ist. Also herunterladen und
installieren und schon kann es losgehen.

### Erste Schritte

Die Seite „Get Started“ von VS Code enthält einige „Start“-Elemente,
eine Liste mit „Recent“-Elementen und eine kurze Liste mit
„Walkthroughs“. Wenn VS Code völlig neu für Sie ist, sollten Sie sich
diese ansehen. Es dauert nur wenige Minuten.

Die VS-Code-Dokumentation ist über das Menü „Hilfe → Dokumentation“
verfügbar. Die Einführungsvideos sind sehr sehenswert. Jede dauert 2 bis
6 Minuten und bietet eine hervorragende Einführung in die Funktionen von
VS Code

<a href="https://code.visualstudio.com/docs/getstarted/introvideos"
class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://code.visualstudio.com/docs/getstarted/introvideos</a>

Die offizielle Dokumentation ist die Adresse, wenn Sie bestimmte
Informationen nachschlagen möchten.

### VS-Code-Erweiterungen

VS-Code kann für jede Art von Text verwendet werden, einschließlich
einer Vielzahl von Programmiersprachen. Es funktioniert mit JavaScript,
ohne Erweiterungen hinzuzufügen. Andere Sprachen werden vom Kontext
erkannt. Wenn Sie also mit dem Erstellen und Speichern von PHP-Code
beginnen, werden Sie wahrscheinlich aufgefordert, ein PHP-Support-Paket
zu installieren.

Klicken Sie auf das Symbol „Erweiterungen“ in der linken
„Aktivitätsleiste“, um zu sehen, was Sie installiert haben und was
empfohlen wird. Sie benötigen die PHP-Debug-Erweiterung!

### Das VS-Code-Bildschirmlayout

Einige Begriffe, die in nachfolgenden Anweisungen verwendet werden:

- **Aktivitätsleiste:** die schmale Leiste links auf dem Bildschirm.
  Wählen Sie ein beliebiges Symbol aus, um die primäre Seitenleiste zu
  öffnen oder zu schließen.
- **Primäre Seitenleiste:** zeigt im geöffneten Zustand Details der
  ausgewählten Aktivität an.
- **Statusleiste:** am unteren Rand des Bildschirms. Es zeigt, was los
  ist.
- **Panel:** ein Bereich unter den Texteditoren, um andere Informationen
  anzuzeigen.

Wählen Sie oben rechts ein Layout-Symbol aus, um eines dieser Elemente
zu öffnen oder zu schließen.

## Codieren einer Joomla-Erweiterung

Um eine Erweiterung zu erstellen, besteht Ihr Ziel darin, eine ZIP-Datei
zu erstellen, die Sie auf einer funktionierenden Joomla-Site
installieren können. Sie benötigen also einen Ordner für Ihren Code.
Dies sollte sich in Ihrem persönlichen Dateibereich auf Ihrem Laptop
oder Desktop-Computer befinden, der für die lokale Entwicklung verwendet
wird. Es sollte sich nicht in Ihrem Website-Baum befinden.
Beispielsweise könnten Sie „~/jextensions“ verwenden, um Unterordner für
verschiedene Erweiterungen zu enthalten. Ich verwende „~/git“, weil es
kurz und einfach zu buchstabieren ist, obwohl es möglicherweise
verwirrend ist, weil jeder Unterordner ein separates Git-Repository
verwendet.

### Beispielcode

Wenn Sie an Beispielcode arbeiten möchten, steht auf GitHub eine
Erweiterung mit dem Namen „mod_debugme“ zur Verfügung. Wie der Name
schon sagt, handelt es sich um ein Modul mit einigen Fehlern. Zusätzlich
zum Modulcode gibt es eine *build.xml*-Datei, um eine Möglichkeit zum
Automatisieren des Bauens zum Testen und Erstellen einer ZIP-Datei zu
veranschaulichen.

Das Modul ist so konzipiert, dass es die nächsten paar (standardmäßig 3)
Ereignisse (Geburtstage) aus einer in einer Datenbanktabelle
gespeicherten Liste anzeigt. Sie können sich vorstellen, dass dies in
einem Büro oder einer Familienseite in der Erwartung von Kuchen
verwendet wird.

Es ist möglicherweise am besten, mit Git-Befehlen von der Befehlszeile
aus loszulegen. Erstellen Sie zuerst einen Ordner für Ihren Code und
klonen Sie dann das Remote-Repository:

```bash
    mkdir ~/git
    cd ~/git
    git clone https://github.com/ceford/j4xdemos-mod-debugme
```

Die Antwort sollte nur wenige Sekunden dauern:

```bash
    Cloning into 'j4xdemos-mod-debugme'...
    remote: Enumerating objects: 23, done.
    remote: Counting objects: 100% (23/23), done.
    remote: Compressing objects: 100% (16/16), done.
    remote: Total 23 (delta 3), reused 23 (delta 3), pack-reused 0
    Unpacking objects: 100% (23/23), done.
```

Sie sollten sich einen Moment Zeit nehmen, um sich den Inhalt des
Ordners anzusehen:

```bash
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

Der Ordner „.git“ enthält Informationen über das Repo. Die Datei
„README.md“ ist ein Markdown-Dokument, das dieses Repo beschreibt. Die
Datei „build.xml“ ist eine Datei, die verwendet wird, um die Erweiterung
mit einem externen Tool, Phing, zu erstellen, das später beschrieben
wird. Der Ordner „mod_debugme“ enthält den Code der Erweiterung.

Komprimieren Sie den Erweiterungsordner, um eine installierbare
ZIP-Datei zu erstellen:

```bash
    zip -r mod_debugme.zip mod_debugme
```

Sie können die ZIP-Datei jetzt auf der Joomla-Site installieren, die Sie
zum Testen verwenden. Nach der Installation müssen Sie ein Standortmodul
erstellen und es einer Modulposition zuweisen. Da es sich um ein
defektes Modul handelt, könnten Sie es einer Position auf „Alle Seiten“
zuweisen, während Sie daran arbeiten; oder Sie könnten es einer Position
auf einer einzelnen Seite zuweisen; oder Sie könnten es in einem Artikel
positionieren, der einen eigenen Menüpunkt hat.

Löschen Sie nach der Installation die ZIP-Datei.

### Aktivieren Sie den Debug-Modus

Stellen Sie in Joomlas Global Configuration „Debug System“ auf „Yes“ und
„Error Reporting“ auf „Maximum“.

Wenn Sie eine Seite öffnen, die das fehlerhafte Modul enthält, sehen Sie
einen Stack-Trace, der Ihnen mitteilt, wo ein Fehler ausgelöst wurde.

<img
src="https://docs.joomla.org/images/thumb/3/3f/J4.x-vscode-primer-stack-trace-en.png/800px-J4.x-vscode-primer-stack-trace-en.png"
class="thumbborder" decoding="async"
srcset="https://docs.joomla.org/images/thumb/3/3f/J4.x-vscode-primer-stack-trace-en.png/1200px-J4.x-vscode-primer-stack-trace-en.png 1.5x, https://docs.joomla.org/images/3/3f/J4.x-vscode-primer-stack-trace-en.png 2x"
data-file-width="1331" data-file-height="850" width="800" height="511"
alt="J4.x-vscode-primer-stack-trace-en.png" />

Manchmal befindet sich der Codierungsfehler in der ersten Zeile des
Stack-Trace. Andernfalls, wenn der Fehler im Bibliothekscode ausgelöst
wird, beispielsweise durch die Übergabe ungültiger Daten an eine
Datenbankfunktion, kann der Codierungsfehler weiter unten in der Liste
der Funktionsaufrufe stehen.

## Erweiterungsordner in VS Code öffnen

Verwenden Sie in VS Code das Menüelement Datei / Ordner öffnen, um den
Ordner zu suchen und zu öffnen, der Ihre lokale Kopie des
Erweiterungscodes „mod_debugme“ enthält. Sie sollten etwas Ähnliches wie
das Folgende sehen:

<img
src="https://docs.joomla.org/images/thumb/3/39/J4.x-vscode-primer-screen.png/800px-J4.x-vscode-primer-screen.png"
class="thumbborder" decoding="async"
srcset="https://docs.joomla.org/images/thumb/3/39/J4.x-vscode-primer-screen.png/1200px-J4.x-vscode-primer-screen.png 1.5x, https://docs.joomla.org/images/3/39/J4.x-vscode-primer-screen.png 2x"
data-file-width="1437" data-file-height="813" width="800" height="453"
alt="J4.x-vscode-primer-screen.png" />

Möglicherweise können Sie das Problem diagnostizieren, indem Sie einfach
den Code lesen. Im Fall des Fehlers *Class "DebugHelper" not found*
sehen Sie, dass ein paar Zeilen zuvor eine *use*-Anweisung
auskommentiert wurde. Das Vergessen, eine „use“-Anweisung einzufügen,
ist ein häufiger Fehler während der anfänglichen Entwicklung!

Beheben Sie dieses Problem und komprimieren und installieren Sie das
Modul erneut. Dieser Schritt wird etwas mühsam, wenn Sie mehrere
Probleme haben. Hier kommen Build-Tools ins Spiel.

## Phing

Phing ist ein Befehlszeilentool, das für alle Plattformen verfügbar ist
und zum Erstellen von Softwarepaketen mithilfe von Anweisungen verwendet
wird, die in einer XML-Datei mit dem standardmäßigen Namen build.xml
gespeichert sind. Für die Arbeit mit Erweiterungscode kann es für zwei
Dinge verwendet werden:

- Kopieren Sie geänderte Dateien aus Ihrem Erweiterungsquellordner an
  die richtigen Stellen in Ihrem Websiteordner.
- Generieren Sie eine neue ZIP-Datei für neue Installationen.

Laden Sie
<a href="https://www.phing.info/Phing" class="external autonumber"
target="_blank" rel="nofollow noreferrer noopener">[1]</a> herunter und
installieren Sie es. Andere Build-Tools sind verfügbar! Sie können Phing
in Ihrem eigenen bin-Ordner oder in einem bin-Ordner des Systems
installieren. Sie müssen den Pfad zu Ihrem Phing-Code notieren. In
diesem Beispiel ist es *~/bin/phing-latest.phar*. Sie können es von der
Befehlszeile aus ausprobieren, nachdem Sie in den Ordner mit Ihrem
Erweiterungscode gewechselt sind:

```bash
    cd ~/git/j4xdemos-mod-debugme
    php ~/bin/phing-latest.phar
```

Antwort:

```bash
    Buildfile: /Users/ceford/git/j4xdemos-mod-debugme/build.xml

    mod_debugme > main:
          ... Any copied files will be mentioned here
          [zip] Building zip: /Users/ceford/zips/mod_debugme.zip

    BUILD FINISHED

    Total time: 0.0863 seconds
```

## VS-Code-Aufgaben

Um Phing innerhalb von VS Code auszuführen, müssen Sie eine
„tasks.json“-Datei im Ordner „.vscode“ im Stammverzeichnis des Ordners
„j4xdemos-mod-debugme“ erstellen. Wenn letzteres nicht vorhanden ist,
erstellen Sie es zuerst. Erstellen Sie dann die Datei „tasks.json“ und
geben Sie den folgenden Code ein:

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

Windows-Benutzer müssen den Windows-spezifischen Befehl korrigieren. Sie
können nun die Erweiterung über das Menü „Terminal / Run Build Task“
erstellen. Das Ergebnis des Befehls sollte im Terminalbereich unter dem
Bearbeitungsbereich erscheinen.

```bash
      *  Executing task: php ~/bin/phing-latest.phar

    Buildfile: /Users/ceford/git/gitdemo/j4xdemos-mod-debugme/build.xml

    mod_debugme > main:

          [zip] Nothing to do: /Users/ceford/zips/mod_debugme.zip is up to date.

    BUILD FINISHED

    Total time: 0.1031 seconds

     *  Terminal will be reused by tasks, press any key to close it.
```

Es kann zu unverständlichen Fehlermeldungen kommen. Die
wahrscheinlichste Ursache sind ungültige Pfade zu Ordnern in der Datei
„build.xml“ oder ein Ordner wurde nicht erstellt. Nur eine weitere Art
von Problem zum Debuggen!

## Fehlersuche

Sie sollten in der Lage sein, den ersten Fehler durch Codeinspektion zu
beheben. Kompliziertere Probleme erfordern das schrittweise Durchlaufen
des Codes mit dem Debugger. Auf diese Weise können Sie Variablen
untersuchen, um zu sehen, ob sie Werte enthalten, die Sie erwarten,
beispielsweise wenn Sie Argumente an Bibliotheksfunktionen übergeben.

### *php.ini* Einstellungen

Um das Debugging mit Xdebug einzurichten, müssen Sie einige Einträge
oben in Ihrer *php.ini*-Datei vornehmen.

```ini
    zend_extension="xdebug.so"
    xdebug.mode="debug"
    xdebug.client_port=9003
    xdebug.start_with_request = yes
```

Nachdem Sie alle Änderungen gespeichert haben, starten Sie Ihren
Apache-Server neu.

### Website-Fenster hinzufügen

Ihr Erweiterungsordner enthält nur wenige Dateien, die ***Quellen*** der
auf Ihrer Website installierten Dateien. Das Debuggen zur Laufzeit
umfasst das Setzen von Breakpoints in Ihren ***site***-Dateien, sodass
Sie Zugriff auf diese Dateien benötigen. Sie können das Menü "Datei /
Ordner zum Arbeitsbereich hinzufügen ..." verwenden, um den Site-Ordner
zu Ihrem Arbeitsbereich hinzuzufügen. Es besteht jedoch eine sehr gute
Chance, dass Sie am Ende Änderungen an Site-Dateien anstelle von
Quelldateien vornehmen. Daher ist es wahrscheinlich am besten, ein
separates VS-Code-Fenster zum Debuggen zu öffnen.

- **Neues Fenster öffnen:** Wählen Sie im VS-Code-Menü *Datei / Neues
  Fenster* und wählen Sie den Ordner mit Ihrer Joomla-Website aus.
- **Ordner öffnen:** Wählen Sie im neu geöffneten Fenster *Datei /
  Ordner öffnen...* aus dem VS-Code-Menü. Suchen Sie Ihren
  Website-Ordner und wählen Sie ihn aus. Sie sollten eine Liste aller
  Dateien auf Ihrer Joomla-Website in der primären Seitenleiste sehen.

### Konfiguration starten

Damit das Debuggen tatsächlich in VS Code funktioniert, benötigen Sie
eine Startkonfiguration. Erstellen Sie im Stammverzeichnis Ihrer Website
einen Ordner mit dem Namen „.vscode“ (beachten Sie die führende
Haltestelle), der eine Datei mit dem Namen „launch.json“ mit folgendem
Inhalt enthält:

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

Denken Sie daran, das pathMappings-Element in diesem Beispiel durch die
tatsächlichen pathMappings auf Ihrer eigenen Website zu ersetzen. Das
Element stopOnEntry unterbricht die Ausführung in der allerersten Zeile
des ausgeführten PHP-Codes.

### *mod_debugme* debuggen

Jetzt sind Sie bereit, die Fehler im installierten Modul zu finden und
zu beheben

- **Modulcode finden:** Finden Sie den ersten Fehler in Zeile 16 von
  JROOT/modules/mod_debugme/mod_debugme.php.
- **Unterbrechungspunkt setzen:** Klicken Sie auf das Feld links neben
  der Zahl 16. Beim Bewegen des Mauszeigers erscheint ein blassroter
  Fleck, der nach dem Klicken vollständig rot wird, um anzuzeigen, dass
  ein Unterbrechungspunkt festgelegt wurde.
- **Debug starten:** Wählen Sie im VS-Code-Menü *Ausführen / Debuggen
  starten*. Laden Sie in Ihrem Browser Ihre Seite neu. Ihr
  VS-Code-Fenster sollte wieder erscheinen, wobei der Code in der ersten
  Zeile der Datei „index.php“ der Website angehalten wird. Am oberen
  Rand des Bildschirms befinden sich einige Symbole zur Steuerung des
  Debug-Prozesses. Sie sollten selbsterklärend sein. Wenn nicht,
  schlagen Sie in der Hilfe/Dokumentation zu VS Code nach.
- **Fortfahren:** Wählen Sie die Schaltfläche „Weiter“ – der Code wird
  bis zu Ihrem ersten Haltepunkt ausgeführt. Untersuchen Sie den Code,
  um zu sehen, was das Problem ist.
- **Hover:** Wenn Sie den Mauszeiger über eine Variable bewegen, der ein
  Wert zugewiesen wurde, erscheint ein kleiner Tooltip, der die
  Variablenattribute zusammenfasst. Für Variablen, denen keine Werte
  zugewiesen wurden, gibt es keinen Tooltip.
- **Variablen:** Die linke Spalte enthält weitere Informationen über den
  Status des Codes am Haltepunkt. Es gibt zu viele, um sie hier zu
  behandeln. Erkunden Sie sie nach Bedarf!
- **Stop Debugging:** Es ist wahrscheinlich am besten, das
  Fortfahren-Symbol zu wählen, da sonst die Webseite leer geliefert
  wird. Andernfalls können Sie die Schaltfläche Stop oder das Menü Run /
  Stop Debugging verwenden.

### Fehler beheben

**Denken Sie daran:** Beheben Sie nicht den Fehler im Website-Code!
Korrigieren Sie es im Quellcode!

Korrigieren Sie den Quellcode und verwenden Sie dann *Terminal / Run
Build Task...*.

Debuggen neu starten.

### Tipps

Ein paar nicht so offensichtliche Probleme:

- Sie beheben den ersten Fehler, aber es stürzt immer noch in dieser
  Zeile ab. Schauen Sie in mod_debugme.xml nach, um zu sehen, wo die
  Quelle von Namespace-Klassen definiert ist. Wenn es in der Quelle
  behoben ist, müssen Sie die ZIP-Datei neu installieren oder
  „administrator/cache/autoload_psr4.php“ löschen. Wenn nicht vorhanden,
  erstellt Joomla diese Datei aus Manifestdateien neu. Aber wenn es
  einen falschen oder fehlenden Eintrag hat, wird es nicht behoben, bis
  die Erweiterung neu installiert wird.
- Manchmal müssen Sie einige Zeilen vor der Zeile, in der der Fehler
  auftritt, einen Haltepunkt setzen, insbesondere wenn Sie Werte
  überprüfen möchten, die an Funktionsaufrufe übergeben werden.
- Tabelle *xxx.yyy\\debugme* existiert nicht. Ah ja, der Code zum
  Erstellen einer Tabelle bei der Installation und zum Entfernen bei der
  Deinstallation wurde nie erstellt. Sie müssen eine SQL-Abfrage in
  phpMyAdmin ausführen, die den Inhalt der Datei „mod\\debugme.sql“
  verwendet. Denken Sie daran, *\#\\* in den Tabellennamen für Ihr
  Datenbankpräfix zu ändern. Und wenn es immer noch fehlschlägt,
  überprüfen Sie den Tabellennamen im Code.

## Screenshot

Wenn alles behoben ist, sehen Sie möglicherweise Folgendes:

<img
src="https://docs.joomla.org/images/1/1b/J4.x-vscode-primer-debugme-fixed-en.png"
class="thumbborder" decoding="async" data-file-width="318"
data-file-height="157" width="318" height="157"
alt="J4.x-vscode-primer-debugme-fixed-en.png" />

Kuchentage?

## Referenzen

Von Joomla! Dokumentation: [Visual Studio
Code](https://docs.joomla.org/Visual_Studio_Code "Visual Studio Code")
behandelt auch die Konfiguration anderer Tools, zum Beispiel CodeSniffer
und PHPUnit.

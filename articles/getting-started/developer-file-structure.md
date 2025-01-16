<!-- Filename: J4.x:Developer:_File_Structure / Display title: Beispiel-Dateistruktur -->

## Einführung

Als Einzelperson, die mit der Entwicklung von Joomla-Erweiterungen auf einem persönlichen Computer, Laptop oder Desktop beginnt, müssen Sie eine geeignete Dateistruktur für Ihren Erweiterungscode einrichten. Der Standort Ihrer Joomla-Website ist durch Ihr Betriebssystem und die Apache-Installation vorgegeben. Der Standort des von Ihnen erstellten Codes liegt in Ihrer Hand.

## Test-Website-Standort

In diesem Beispiel befinden sich Joomla-Testseiten in Unterordnern des Dokumentenstamms. Dies ermöglicht die Erstellung vieler Websites für verschiedene Projekte, ohne dass virtuelle Hosts erstellt werden müssen. Einige Testseiten könnten überhaupt nicht mit Joomla in Verbindung stehen. Der Stamm der Website für verschiedene Plattformen:

- Mac: /Benutzer/benutzername/Sites
- Linux: /home/benutzername/public_html
- Windows: ...

Dies ist ein Screenshot eines Teils einer Site-Ordnerliste, die eine Auswahl vieler Testseiten zeigt:

![mehrere Websites auf Mac](../../../en/images/getting-started/developer-file-structure-mac-sites.png)

Jeder wird über seinen Unterordnernamen aufgerufen. Beispiele:

- `http://localhost/j4test`
- `http://localhost/j520`

Es gibt Umstände, unter denen Sie es vorziehen könnten, separate virtuelle Sites zu erstellen. Das wird hier nicht behandelt.

## Websitedateistruktur testen

Wenn Sie dies noch nicht getan haben, müssen Sie sich mit der Struktur einer Joomla-Website vertraut machen. Die folgende Abbildung zeigt einen typischen Joomla-Datei- und Ordnerbaum, bei dem der Administrator-Ordner erweitert ist, um seinen Inhalt anzuzeigen.

![joomla-Dateistruktur mit ausgeklapptem Administrator](../../../en/images/getting-started/developer-file-structure-mac-joomla.png)

Hier wird der funktionierende Code installiert. Der Quellcode befindet sich woanders.

## Speicherort des Entwicklererweiterungscodes

Der Speicherort Ihres Erweiterungscodes ist eine persönliche Wahl. Ich mag es, meinen Erweiterungscode in einer Dateistruktur zu halten, die zur Erstellung einer installierbaren Zip-Datei geeignet ist. Der Basisordner meines Baumes ist /Users/username/git, weil ich git buchstabieren kann und ich git für die Versionskontrolle verwende. Sie müssen das nicht tun - git wird in einem separaten Tutorial behandelt. Mein übergeordneter git-Ordner enthält viele Unterordner, von denen jeder separate git-Ordner für die Versionskontrolle verwenden kann. Hier ist ein Screenshot, der eine Teilliste von Projekten zeigt:

![joomla Dateistruktur Projektordner](../../../en/images/getting-started/developer-file-structure-mac-project-folders.png)

Beachten Sie, dass einige der Ordnernamen mit `j4xdemos` beginnen, was ich als ersten Teil des Namespaces für meine Projekte übernommen habe, die für Joomla 4 Tutorialzwecke erstellt wurden. Es ist nicht notwendig als Teil des Ordnernamens, aber es ist etwas, worüber man nachdenken sollte: Der erste Teil Ihres Namespaces sollte etwas Einzigartiges für Sie oder Ihre Organisation sein. Ich habe seitdem `cefjdemos` als mein Namespace-Präfix übernommen, da es persönlicher ist und nicht spezifisch für eine Joomla-Version.

### Projektordnerstruktur

Am Beispiel von j4xdemos-com-mywalks befindet sich der gesamte Code, der in die Erweiterung kommt, im Ordner com_mywalks. Wenn dieser komprimiert wird, wird die com_mywalks.zip-Datei erstellt, die zur Installation in Joomla verwendet wird. Keine der anderen Dateien im Root-Verzeichnis sind in der Zip-Datei enthalten, sie können jedoch im GitHub-Repository enthalten sein. Zum Beispiel ist README.md eine Markdown-Format-Textdatei, die eine kurze Beschreibung der Erweiterung enthält. Der Ordnername j4xdemos-com-mywalks ist nicht bedeutend. Er wird nur verwendet, um die Ordner alphabetisch zu sortieren.

Im folgenden Beispiel wurde der Ordner j4xdemos-com-mywalks in VSCodium geöffnet, um die Projektcode-Struktur anzuzeigen. Die Datei mywalks.xml ist eine Manifestdatei, die Joomla anweist, was wohin installiert werden soll. Die Ordner admin und site enthalten Code, der in administrator/components/com_mywalks und components/com_mywalks eingefügt wird.

![Projektordner geöffnet in VSCodium](../../../en/images/getting-started/developer-file-structure-mac-vscodium.png)

Es sollte offensichtlich sein, dass selbst eine kleine Komponente ziemlich viele Ordner und Dateien benötigt. Es gibt Erweiterungs-Vorlagenerstellungstools, mit denen man schnell eine Grundstrukturkomponente erstellen kann. Diese werden an anderer Stelle behandelt. ToDo

Bereit, etwas Code zu erstellen?

*Übersetzt von openai.com*

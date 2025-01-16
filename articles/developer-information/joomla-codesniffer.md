<!-- Filename: Joomla_CodeSniffer / Display title: Kodierungsstandards -->

<div class="alert alert-warning">
Der letzte Teil dieses Beitrags muss aktualisiert werden!
</div>

## Ein Überblick über KI

Codierungsstandards sind wichtig für die Softwareentwicklung, weil sie:

- **Verbessere die Code-Qualität**
  - Kodierungsstandards helfen sicherzustellen, dass der Code zuverlässig, sicher und gefahrlos ist. Sie können auch dazu beitragen, Leistungsprobleme und Sicherheitsbedenken zu reduzieren, die durch schlechte Kodierungspraktiken entstehen könnten.
- **Code lesbarer und wartbarer machen**
  - Kodierungsstandards helfen, den Code leichter verständlich, lesbar und wartbar zu machen. Dies kann es auch neuen Entwicklern erleichtern, mit dem Code zu arbeiten.
- **Entwicklung beschleunigen**
  - Kodierungsstandards können Entwicklern helfen, häufige Fehler zu vermeiden, die den Kodierprozess verlangsamen können.
- **Zusammenarbeit verbessern**
  - Kodierungsstandards können die Zusammenarbeit unter Entwicklern erleichtern, selbst in großen Teams.
- **Konsistenz sicherstellen**
  - Kodierungsstandards helfen sicherzustellen, dass der Code über Projekte hinweg konsistent ist. Dies kann es Entwicklern erleichtern, gemeinsam an denselben Projekten zu arbeiten.
- **Skalierbarkeit verbessern**
  - Kodierungsstandards können sicherstellen, dass der Code skalierbar bleibt, ohne unüberschaubar zu werden.
- **Klare Kriterien für Codeüberprüfungen bieten**
  - Kodierungsstandards können klare Kriterien für Codeüberprüfungen bieten, was zu effektiverem Feedback führen kann.

Codierungsstandards umfassen oft Regeln für Einrückungen, Zeilenlänge, Klammerplatzierung und Abstände.

Joomla verwendet den [PSR-12](https://www.php-fig.org/psr/psr-12/) Kodierungsstandard. Sie können diesen Kodierungsstandard in Ihrer IDE aktivieren und Hinweise erhalten, wenn Sie den Kodierungsstandard nicht befolgen, oder auch eine automatische Korrektur verwenden. Wir empfehlen, diesen Standard zu befolgen, wenn Sie Ihre eigenen Erweiterungen entwickeln, um kompatibel mit dem Core zu bleiben und sicherzustellen, dass Ihr Code hoffentlich mit zukünftigen Versionen funktioniert.

## PHP-Code-Sniffer

Bitte konsultieren Sie die aktuelle [PHP_CodeSniffer](https://github.com/PHPCSStandards/PHP_CodeSniffer/) Quelle für Informationen zu diesem Dienstprogramm und wie man es installiert. Beachten Sie, dass die ursprüngliche Quelle verlassen wurde, aber möglicherweise von Suchmaschinen aufgelistet wird. Es gibt zwei Skripte:

- **phpcs** erkennt Verstöße gegen einen Kodierungsstandard und erstellt einen Bericht.
- **phpcbf** versucht, Verstöße gegen Kodierungsstandards zu korrigieren und erstellt einen Bericht darüber, was es getan hat.

Du kannst die einzelnen Skripte installieren oder Composer verwenden, um sie zu installieren. Du findest sie auch in einem Joomla-Clone, der sich in libraries/vendor/bin befindet, zusammen mit einigen anderen nützlichen Dienstprogrammen.

## Testen von PHP Code Sniffer

Wenn Sie eine globale Installation in Ihrem `$PATH` durchführen, können Sie den Code Sniffer in der Befehlszeile im Stammverzeichnis eines Projekts ausführen. Versuchen Sie dies, um eine Liste der Codierungsstandards anzuzeigen:

```sh
phpcs -i
The installed coding standards are MySource, PEAR, PSR1, PSR2, PSR12, Squiz and Zend
```

Als Beispiel wurde der folgende Befehl in einem älteren Projektordner verwendet, der den früheren benutzerdefinierten Joomla-Codierungsstandard verwendete. Unter anderem wurden Tabs anstelle von Leerzeichen für das Layout verwendet. Viele ähnliche Zeilen wurden aus Gründen der Kürze unten weggelassen.

```sh
phpcs --standard=PSR12 .

FILE: /Users/ceford/git/j4xdemos-plg-toc/j4xdemostoc/j4xdemostoc.php
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
FOUND 132 ERRORS AND 3 WARNINGS AFFECTING 116 LINES
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   1 | WARNING | [ ] A file should declare new symbols (classes, functions, constants, etc.) and cause no other side effects, or it should execute logic with side effects, but should not do both. The
     |         |     first symbol is defined on line 22 and the first side effect is on line 10.
   1 | ERROR   | [x] Header blocks must be separated by a single blank line
  22 | ERROR   | [ ] Each class must be in a namespace of at least one level (a top-level vendor name)
  24 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
  25 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
  26 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
  ...
  42 | ERROR   | [x] Expected 1 space after closing parenthesis; found newline
...
  48 | ERROR   | [x] No space found after comma in argument list
  48 | ERROR   | [x] Expected 1 space after closing parenthesis; found newline
...
  67 | ERROR   | [x] Expected 0 spaces before closing parenthesis; 1 found
...
  75 | ERROR   | [x] Space after opening parenthesis of function call prohibited
  75 | ERROR   | [x] Expected 0 spaces before closing parenthesis; 1 found
...
 138 | WARNING | [ ] Line exceeds 120 characters; contains 168 characters
 139 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
 139 | WARNING | [ ] Line exceeds 120 characters; contains 141 characters
...
 156 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
 157 | ERROR   | [x] Expected 1 newline at end of file; 0 found
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
PHPCBF CAN FIX THE 131 MARKED SNIFF VIOLATIONS AUTOMATICALLY
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Time: 123ms; Memory: 8MB
```

Das vorherige Beispielprojekt enthielt eine einzelne PHP-Datei und keine CSS- oder JS-Dateien. phpcs erstellt einen Bericht für jede PHP-, CSS- und JS-Datei in einem Projekt. Hier sind einige Beispiele für CSS- und JS-Dateien:

```sh
FILE: /Users/ceford/git/j4x-demos-com-mediacat/com_mediacat/media/css/mediacat.css
----------------------------------------------------------------------------------
FOUND 33 ERRORS AFFECTING 32 LINES
----------------------------------------------------------------------------------
  3 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
...
 51 | ERROR | [x] Whitespace found at end of line
...
 79 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
----------------------------------------------------------------------------------
PHPCBF CAN FIX THE 33 MARKED SNIFF VIOLATIONS AUTOMATICALLY
----------------------------------------------------------------------------------

FILE: /Users/ceford/git/j4x-demos-com-mediacat/com_mediacat/media/css/file-icon-classic.min.css
-----------------------------------------------------------------------------------------------
FOUND 0 ERRORS AND 1 WARNING AFFECTING 1 LINE
-----------------------------------------------------------------------------------------------
 1 | WARNING | File appears to be minified and cannot be processed
-----------------------------------------------------------------------------------------------

FILE: /Users/ceford/git/j4x-demos-com-mediacat/com_mediacat/media/js/mediacat-site.js
-------------------------------------------------------------------------------------
FOUND 38 ERRORS AFFECTING 30 LINES
-------------------------------------------------------------------------------------
  3 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
  5 | ERROR | [x] Expected 1 space after FUNCTION keyword; 0 found
  6 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
...
 13 | ERROR | [x] Opening brace should be on a new line
...
 29 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
 29 | ERROR | [x] Expected at least 1 space before "+"; 0 found
 29 | ERROR | [x] Expected at least 1 space after "+"; 0 found
...
 36 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
-------------------------------------------------------------------------------------
PHPCBF CAN FIX THE 38 MARKED SNIFF VIOLATIONS AUTOMATICALLY
-------------------------------------------------------------------------------------
```

## Befehlsvariationen

Sie können Hilfe zu phpcs-Befehlen erhalten:

```sh
phpcs --help
```

### Eine oder mehrere Dateien ausschließen

Verwenden Sie eine durch Kommas getrennte Liste von Dateimustern, um Dateien von der Code-Stil-Validierung auszuschließen. Beispiel

```php
phpcs --standard=PSR12 --ignore='libraries/*' .
```

### Eine oder mehrere Regeln ausschließen

Joomla erlaubt längere Zeilen als der PSR-Standard, 560 anstatt 120. Der folgende Befehl kann verwendet werden, um die Warnungen für lange Zeilen zu unterdrücken:

```sh
phpcs --standard=PSR12 --ignore='libraries/*' --exclude=Generic.Files.LineLength .
```

Sie können die Regel, die verletzt wird, mit diesem Befehl finden:

```sh
phpcs -s yourfile.php
```

### Joomla-Ausnahmen

Für die Entwicklung einer Erweiterung mit einer IDE können Sie sich entscheiden, den PSR12-Standard ohne Ausnahmen zu verwenden. Joomla hat ein [benutzerdefiniertes Regelwerk](https://github.com/joomla/joomla-cms/blob/5.2-dev/ruleset.xml), das viele Ausnahmen erlaubt. Es wird zur Validierung der gesamten Joomla-Installation während der Systemtests verwendet.

## Behebung von Verstößen

Die einzelne PHP-Datei, die `FOUND 132 ERRORS AND 3 WARNINGS AFFECTING 116 LINES` wie oben gezeigt, kann größtenteils wie folgt behoben werden:

```sh
phpcbf --standard=PSR12 .

PHPCBF RESULT SUMMARY
-----------------------------------------------------------------------------------
FILE                                                               FIXED  REMAINING
-----------------------------------------------------------------------------------
/Users/ceford/git/j4xdemos-plg-toc/j4xdemostoc/j4xdemostoc.php     131    4
-----------------------------------------------------------------------------------
A TOTAL OF 131 ERRORS WERE FIXED IN 1 FILE
-----------------------------------------------------------------------------------

Time: 232ms; Memory: 8MB
```

Erneutes Ausführen des phpcs-Dienstprogramms zeigt, welche Probleme noch bestehen:

```sh
phpcs --standard=PSR12 .

FILE: /Users/ceford/git/j4xdemos-plg-toc/j4xdemostoc/j4xdemostoc.php
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
FOUND 1 ERROR AND 3 WARNINGS AFFECTING 4 LINES
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   1 | WARNING | A file should declare new symbols (classes, functions, constants, etc.) and cause no other side effects, or it should execute logic with side effects, but should not do both. The first
     |         | symbol is defined on line 23 and the first side effect is on line 11.
  23 | ERROR   | Each class must be in a namespace of at least one level (a top-level vendor name)
 132 | WARNING | Line exceeds 120 characters; contains 168 characters
 133 | WARNING | Line exceeds 120 characters; contains 141 characters
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Time: 127ms; Memory: 8MB
```

Schlussfolgerung: Eine Erweiterung, die für eine frühere Joomla-Version codiert wurde und einen anderen Kodierstandard verwendet, benötigt nun einige Arbeit!

## Installation in IDEs

Die meisten integrierten Entwicklungsumgebungen können den Code-Sniffer verwenden, während Sie arbeiten oder wenn Sie eine Datei speichern.

### Installation in VSCode

In VSCode (und/oder VScodium) das Symbol Erweiterungen auswählen, nach PHP_CodeSniffer suchen und installieren. Es benötigt eine Konfiguration:

1. In **Einstellungen** finde PHP_CodeSniffer
2. Setze das Feld **PHP Code Sniffer → Ausführen:** so, dass es zum Speicherort deines phpcs-Binaries passt.
3. Wähle in der Liste **PHP Code Sniffer: Standard** die Option **PSR12** aus.

Schließen und Sie sind bereit für den Einsatz.

Einige der oben gezeigten Probleme können mit PSR1-Direktiven behoben werden.

```php
<?php

/**
 * @package     Whatever
 *
 * @phpcs:disable PSR1.Classes.ClassDeclaration.MissingNamespace
 */

use joomla\CMS\...

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects
```

Andere benötigen Überlegung!

Nach der Installation in VSCode werden Verstöße gegen den Code-Stil erkannt und mit einer roten Wellenlinie unterstrichen. Bewegen Sie den Mauszeiger über das Problem, um eine Meldung und eine mögliche Lösung zu sehen, wie in diesem Beispiel:

```txt
Header blocks must be separated by a single blank line
PHP_CodeSniffer(PSR12.Files.FileHeader.SpacingAfterBlock)
```

<div class="alert alert-warning">
Die folgenden Abschnitte müssen aktualisiert werden!
</div>

### Installation in PhpStorm

Code Sniffer wird in PhpStorm standardmäßig unterstützt. Gehen Sie zu den Einstellungen und unter **Editor → Inspektionen** sehen Sie die Liste der Sniffer, die Sie installiert haben.

#### Pfad zum Code Sniffer festlegen

1. Öffne Einstellungen (CTRL-ALT-S / CMD-,)
2. Gehe zu Sprachen & Frameworks
3. Klicke auf PHP
4. Klicke auf Qualitätstools
5. Klicke auf den Pfeil des Dropdown-Menüs bei PHP cs fixer
6. Die Konfiguration ist standardmäßig auf Lokal eingestellt
7. Klicke auf die 3 Punkte dahinter, um den Konfigurationsbildschirm zu öffnen
8. Die erste Option ist der PHP Code Sniffer (phpcs) Pfad
9. Klicke auf die 3 Punkte hinter dem Pfad, um den Speicherort der phpcs-Datei auszuwählen. Siehe oben, wo phpcs auf deiner Seite installiert sein könnte
10. Klicke auf Validieren, um sicherzustellen, dass der Pfad korrekt ist und phpcs funktioniert
11. Klicke auf OK

#### Aktivieren des Code-Stils

1. Öffnen Sie Einstellungen (STRG-ALT-S / CMD-,)
2. Gehen Sie zum Editor
3. Klicken Sie auf Inspektionen
4. In der Liste, gehen Sie zu PHP
5. Klicken Sie auf PHP Code Sniffer Validierung
6. Klicken Sie auf das Kontrollkästchen dahinter, um es zu aktivieren
7. Klicken Sie auf die Schaltfläche Neu laden (2 Pfeile), um ein erneutes Laden der Regeln von der Festplatte zu erzwingen
8. Joomla sollte jetzt in der Liste verfügbar sein. Siehe folgendes Bild.
9. Klicken Sie auf OK

![CodeSniffer in PHPStorm](../../../en/images/getting-started/core-phpstorm-code-sniffer.png)

### Installation in Netbeans

Netbeans hat die Sniffer-Funktionalität in das Kernsystem integriert.

1. Starten Sie Ihre Netbeans IDE
2. Öffnen Sie **Werkzeuge → Optionen → PHP → Code-Analyse → Code Sniffer**
3. Sie müssen den Pfad zur *phpcs.bat* aus dem installierten PHP_CodeSniffer PEAR-Paket festlegen
    - Für Unix-basierte Systeme ist der Pfad etwa /usr/bin/phpcs
    - In XAMPP (Windows) finden Sie die Datei im PHP-Stammordner (z.B. C:\xampp\php\phpcs.bat)
4. Wählen Sie als "Standard-Standard" "Joomla", um den Joomla!-Standard zu verwenden
5. Jetzt können Sie auf *OK* klicken, um mit dem Sniffing zu beginnen
6. Öffnen Sie im oberen Menü **Quelle → Untersuchen...**
7. Viel Spaß

### Installation in Eclipse

1. **Hilfe → Neue Software installieren...**
2. **Arbeiten mit** Füllen Sie eine der Update-Site-URLs ein.
3. Wählen Sie die gewünschten Werkzeuge aus.
4. Starten Sie Eclipse neu.
5. **Fenster → Einstellungen**
6. **PHP-Werkzeuge → PHP CodeSniffer**

![Eclipse PTI Einstellungen](../../../en/images/getting-started/core-eclipse-pti-settings.png)

Du kannst jetzt nach Verstößen gegen gängige Standards im Code schnüffeln.

![Codesniffer in Eclipse](../../../en/images/getting-started/core-eclipse-pti.png)

### Installation in Geany

- Öffnen Sie eine PHP-Datei. (Andernfalls ist das Build-Menü nicht zugänglich.)
- Wählen Sie im oberen Menü **Erstellen → Build-Befehle festlegen**.
- Wählen Sie das zweite Feld und benennen Sie es nach Wunsch. Geben Sie diesen Code im Befehl ein:<br>
  `phpcs --standard=Joomla "%f" | sed -e 's/^/%f |/' | egrep 'WARNING|ERROR'`
- Geben Sie diesen Code im Feld Fehlerregulärer Ausdruck ein: `(.+) [|]\s+([0-9]+)`
- Wählen Sie OK.
- Wenn das Nachrichtenfenster nicht geöffnet ist, zeigen Sie es an, indem Sie es im oberen Menü Ansicht auswählen.
- Beim Anzeigen einer PHP-Datei drücken Sie F9, um die gefundenen Fehler zu sehen.

### Installation in Atom

- Installieren Sie phpcs und die Joomla-Standards wie oben beschrieben.
- Installieren Sie in **Atom → Einstellungen → Installieren** **linter-phpcs** und alle erforderlichen Abhängigkeiten.
- Passen Sie in **Atom → Einstellungen → Pakete → linter-phpcs → Einstellungen** Folgendes an:
  - **Ausführbarer Pfad** zum Pfad Ihres phpcs
  - **Kodierungsstandard oder Konfigurationsdatei**: PSR12
  - **Tabulatorbreite**: 4

*Übersetzt von openai.com*

<!-- Filename: J4.x:Developer:_Eclipse_PDT / Display title: Eclipse PDT -->

## Einführung

Als PHP-Entwickler benötigen Sie einige Werkzeuge, die Ihnen bei Ihrer Arbeit helfen. Eclipse ist eine IDE (Integrierte Entwicklungsumgebung), die für alle Arten von Projekten in allen möglichen Programmiersprachen verwendet werden kann. Eclipse PDT (PHP Developer Tools) enthält die Erweiterungen, die für die PHP-Entwicklung erforderlich sind. Einige der auf der Startseite erwähnten Funktionen sind:

- Syntaxhervorhebung
- Syntaxvalidierung
- Inhaltsunterstützung
- Codenavigation
- PHP-Debugging (Zend Debugger / Xdebug)
- PHP-Profiling (Zend Debugger / Xdebug)

Fügen Sie dazu die Fähigkeit hinzu, Dateien dorthin zu kopieren, wo sie in Ihrer lokalen Test-Website sein müssen, und ein Zip-Datei zu erstellen, es gibt genug Material, um jahrelang damit zu arbeiten. Es dauert Zeit, zu lernen, wie man Eclipse PDT benutzt, einen Tag oder so, aber es lohnt sich. Es gibt andere IDEs und Code-Editoren, die verfügbar sind!

### Alternativen

**IDEs** verfügen über alle Funktionen, die benötigt werden, um PHP-Projekte zu erstellen. Plattformübergreifende Beispiele, die von einigen Joomla-Entwicklern verwendet werden, sind:

- **JetBrains PhpStorm** Kommerziell, plattformübergreifend.
- **Apache NetBeans** Kostenlos, Open Source, plattformübergreifend.
- **Visual Studio Code** Kostenlos, plattformübergreifend.

**PHP-Editoren** sind gut zum Bearbeiten von Code, fehlen jedoch einige der Raffinessen, die für große Projekte benötigt werden. Beispiele umfassen:

- **Notepad++** Kostenlos, nur für Windows.

## Eclipse PDT installieren

Gehen Sie zur [Eclipse PDT](https://www.eclipse.org/pdt/) Webseite und laden Sie die für Ihre Plattform (Linux, Mac oder Windows) verfügbare Version herunter.

Befolgen Sie die Installationsanweisungen für Ihre Plattform.

## Der Eclipse-Arbeitsbereich

Es gibt mindestens drei verschiedene Orte, an denen sich Ihr Projektcode befinden wird:

- **Quellcode-Dateien des Projekts** sind diejenigen, die Sie selbst erstellen und bearbeiten. Sie sollten nicht in Ihrem Web-Baum sein. Sie sollten sich in Ihrem persönlichen Speicherbereich befinden. Beispiele, wobei jedes Projekt in einem separaten Unterordner ist:
  - /Users/username/git (Mac)
  - /home/username/git (Linux)
  - ... (Windows)
- **Web-Baum**-Standort hängt von Ihrer Software-Stack-Installation ab. Sie können Ihren eigenen Speicherbereich verwenden. Beispiele:
  - /Users/username/Sites/j4test (Mac)
  - /home/username/public_html/j4test (Linux)
  - ... (Windows).
- **Eclipse Workspace** ist, wo Eclipse Informationen über einzelne Projekte speichert. Der Standardstandort ist die Wurzel Ihres eigenen Speicherbereichs. Beispiele:
  - /Users/username/eclipse-workspace (Mac)
  - /home/username/eclipse-workspace (Linux)
  - ... (Windows).

Bereit, Eclipse zu starten?

## Eclipse starten

Nach dem Start werden Sie aufgefordert, verschiedene Einstellungen zu bestätigen. An einem bestimmten Punkt werden Sie aufgefordert, einen Arbeitsbereich auszuwählen. Der Standard ist /home/username/eclipse-workspace, aber Sie möchten für jedes Projekt einen Unterordner erstellen, also wählen Sie die Schaltfläche Durchsuchen und dann die Schaltfläche Neuer Ordner aus. Im untenstehenden Beispiel habe ich einen Ordner namens j4tutorials erstellt.

![Wählen Sie den Eclipse-Arbeitsbereichsstandort](../../../en/images/getting-started/eclipse-pdt-choose-workspace.png)

Wählen Sie die Schaltfläche **Starten**. Falls etwas nicht stimmt, können Sie später unter Datei / Arbeitsbereich wechseln die Arbeitsfläche erneut auswählen. Nicht verwendete Arbeitsbereiche können Sie später ebenfalls löschen.

Beim ersten Start wird Ihnen ein Begrüßungsbildschirm angezeigt. Sie können diesen Bildschirm auch über die Menüoptionen **Hilfe / Willkommen** aufrufen.

![Eclipse-Willkommensbildschirm](../../../en/images/getting-started/eclipse-pdt-welcome.png)

Beginnen Sie mit der Überprüfung der IDE-Konfigurationseinstellungen. Sie können alle geeigneten Einstellungen abhaken oder mit einem Kreuz markieren, oder, wenn Sie unsicher sind, das Häkchen entfernen oder das Kreuz aufheben oder weitermachen. Sie können die Einstellungen auch später erneut ändern.

## Erstelle ein neues PHP-Projekt

Das erste Projekt, das hinzugefügt werden soll, ist der Testcode der Website. Sie benötigen dies, um zu überprüfen, ob Ihre eigenen Dateien an die richtigen Stellen kopiert werden und für späteres Debugging. Wählen Sie den Punkt im Begrüßungsbildschirm aus. Wenn Sie bereits zum Eclipse-Arbeitsbereich weitergegangen sind, wählen Sie Erstellt ein PHP-Projekt im Projekt-Explorer. Im Formular Neues PHP-Projekt:

- Geben Sie einen geeigneten Projektnamen ein. Es sollte ein kurzes Wort sein, das im Projekt-Explorer erscheint.
- Wählen Sie die Option **Projekt an bestehendem Ort erstellen (aus bestehender Quelle)**.
- **Durchsuchen** Sie den Speicherort Ihrer Joomla-Test-Website und wählen Sie diesen aus.
- Wählen Sie **Fertigstellen**.

![Eclipse neues PHP-Projekt](../../../en/images/getting-started/eclipse-pdt-new-project.png)

Ihre Website-Dateien werden dem Projekt-Explorer hinzugefügt. Es kann ein oder zwei Minuten dauern, bis Eclipse diesen Vorgang abgeschlossen hat, da im Hintergrund einige Vorbereitungen getroffen werden. Sie können das Projekt auswählen, um die erste Ordnerebene zu erweitern.

Zwei Dateien werden zu Ihrem Site-Ordner hinzugefügt: .buildpath und .project. Da sie mit einem Punkt (.) beginnen, sehen Sie sie möglicherweise nicht und müssen sich deshalb keine Sorgen machen. Es handelt sich um XML-Dateien, die Informationen enthalten, die von Eclipse verwendet werden.

## Die PHP-Perspektive

Die Sammlung von Panels im Eclipse-Bildschirm wird als Perspektive bezeichnet. Die beiden am häufigsten verwendeten sind die PHP-Perspektive und die Debug-Perspektive. Sie können zwischen ihnen wechseln, indem Sie die Symbole oben rechts in der Symbolleiste verwenden. Sie können auch das Menü verwenden: Fenster / Perspektive / Perspektive öffnen / PHP oder Debug.

Die folgenden Abbildungen zeigen die PHP-Perspektive mit ein paar geöffneten Bearbeitungsformularen.

![Eclipse php Perspektive](../../../en/images/getting-started/eclipse-pdt-php-perspective.png)

Die Eclipse-Panels:

- Das linke Panel ist der Projektexplorer, in dem Sie durch Ihre Dateistruktur navigieren können.
- Im oberen mittleren Panel erscheinen geöffnete Texteditoren.
- Das obere rechte Panel zeigt normalerweise eine Gliederung des ausgewählten Bearbeitungspanels. Es kann auch andere Informationen anzeigen.
- Das untere rechte Panel zeigt eine von mehreren Ansichten an, die über das Menü Fenster / Ansicht anzeigen ausgewählt werden können.

Die Ansicht „Probleme“ zeigt Fehler (100 von 791 Beiträge) und Warnungen (100 von 11629). Das scheint viel zu sein, ist aber nichts, worüber man sich Sorgen machen muss.

## Ein neues Hello Universe PHP-Projekt

Sie sind nun bereit, ein neues PHP-Projekt für die Erweiterung zu erstellen, die Sie entwickeln möchten. Zur Veranschaulichung können Sie mit einer einfachen Komponente namens com_hellouniverse beginnen. Für mich wird der Hello Universe Code in /Users/username/git/j4xtutorials-com-hellouniverse gespeichert sein und ich werde j4xtutorials als meine Namensraumzugehörigkeit verwenden (das erste Wort im Komponentennamensraum).

Im Eclipse-Menü wählen Sie Datei / Neu / PHP-Projekt. Das Formular ist das oben dargestellte. Meine Daten:

- Projektname: HelloUniverse
- Inhalte: Wählen Sie Projekt an vorhandenem Ort erstellen (aus vorhandener Quelle).
- Durchsuchen: Navigieren Sie zu /Home/username/git und wählen Sie die Schaltfläche Neuer Ordner.
- Geben Sie im Dialog Neuer Ordner j4xtutorials-com-hellouniverse ein und drücken Sie Erstellen.
- Öffnen Sie den leeren Ordner und wählen Sie Öffnen.
- Das ausgewählte Verzeichnis sollte lauten: /Users/username/git/j4xtutorials-com-hellouniverse
- Wählen Sie Abschließen.

Sie werden jetzt das neue Projekt im Project Explorer zusammen mit Ihrem Website-Projekt sehen. Das neue Projekt enthält zwei Elemente, die beide mit PHP-Unterstützung zusammenhängen. Es gibt einige von Eclipse verwendete versteckte Elemente: .settings (Ordner), .buildpath und .project.

## com_hellouniverse-Ordner hinzufügen

Mit dem HelloUniverse-Projekt ausgewählt:

- Wählen Sie den Menüpunkt Datei aus oder klicken Sie mit der rechten Maustaste auf den Projektnamen.
- Wählen Sie die Menüeinträge Neu / Ordner aus.
- Geben Sie im Formular Neuer Ordner im Feld Ordnername: com_hellouniverse ein.
- Wählen Sie Fertigstellen.

Der Ordner com_hellouniverse ist der Ort, an dem Ihr Erweiterungscode abgelegt wird. Alles, was sich in diesem Ordner befindet, wird in einer Zip-Datei enden, die Sie zur Installation in Joomla verwenden können. Alles, was sich nicht in diesem Ordner befindet, wird für andere Zwecke verwendet. Zwei häufige Dateien auf dieser Ebene:

- README.md ist eine im Markdown formatierte Textdatei, die die Komponente beschreibt. Solche Dateien werden häufig in Verbindung mit remote-Code-Speicherung verwendet, zum Beispiel auf Github (mehr dazu an anderer Stelle).
- build.xml ist eine Datei, die Anweisungen enthält, was zu tun ist, nachdem Sie Änderungen am Code vorgenommen haben. Am häufigsten werden Sie geänderte Dateien an die richtigen Stellen in Ihrem Website-Baum kopieren und die ZIP-Datei neu generieren wollen. Dann können Sie bei den meisten Änderungen einfach die Webseite, an der Sie arbeiten, neu laden. Sie müssen die Komponente nicht neu installieren. Dies spart enorm viel Zeit!

## Admin-, Site- und Medienordner hinzufügen

- Wählen Sie den Ordner com_hellouniverse aus und verwenden Sie die Menüoptionen Datei / Neu / Ordner, um einen neuen Ordner mit dem Namen admin zu erstellen.
- Erneut, dieses Mal erstellen Sie einen neuen Ordner mit dem Namen media. ACHTUNG: Stellen Sie sicher, dass der übergeordnete Ordner com_hellouniverse und nicht HelloUniverse ist.
- Erneut, dieses Mal erstellen Sie einen neuen Ordner mit dem Namen site. Wenn er am falschen Ort erstellt wurde, kann er an die richtige Stelle verschoben werden.

## build.xml

Der folgende Code enthält ein Beispiel für eine build.xml-Datei, die im Stammverzeichnis Ihres HelloUniverse-Projekts eingefügt werden soll.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="hellouniverse" basedir="." default="main">
    <property file=".project" />

    <!-- Source and Destinations -->

    <property name="package"  value="${phing.project.name}" override="true" />
    <property name="srcdir" value="${project.basedir}/com_hellouniverse" override="true" />
    <property name="sitedir" value="/Users/username/Sites/j4tutorials"  override="true" />
    <property name="zipsdir" value="/Users/username/zips"  override="true" />

    <!-- Filesets -->

    <fileset dir="${srcdir}/admin" id="adminfiles">
        <include name="**" />
    </fileset>
    <fileset dir="${srcdir}/media" id="mediafiles">
        <include name="**" />
    </fileset>
    <fileset dir="${srcdir}/site" id="sitefiles">
        <include name="**" />
    </fileset>
    <fileset dir="${srcdir}" id="allfiles">
        <include name="**" />
    </fileset>

    <!-- Targets -->

    <target name="main" description="main target">
        <copy todir="${sitedir}/administrator/components/com_hellouniverse">
            <fileset refid="adminfiles" />
        </copy>
        <copy todir="${sitedir}/media/com_hellouniverse">
            <fileset refid="mediafiles" />
        </copy>
        <copy todir="${sitedir}/components/com_hellouniverse">
            <fileset refid="sitefiles" />
        </copy>
        <zip destfile="${zipsdir}/com_hellouniverse.zip">
            <fileset refid="allfiles" />
        </zip>
    </target>
</project>
```

Einige Erklärungen:

- **srcdir** ist der Speicherort Ihres Quellcodes im Dateisystem, von dem Dateien kopiert werden.
- **sitedir** ist der Speicherort Ihrer Testseite im Dateisystem, wohin die Dateien kopiert werden.
- **zipsdir** ist ein Speicherort für die Zip-Datei - ich finde es praktisch, die Zip-Dateien von verschiedenen Projekten zusammen in einem Zips-Ordner aufzubewahren.
- **Dateisätze** sind Quellgruppen, die an verschiedene Ziele kopiert werden sollen. bedeutet alle Ordner und Dateien.
- **Ziele** sind die Zielorte und was dorthin kopiert werden muss.

Erstellen Sie eine build.xml-Datei mit dem obigen Code:

- Wählen Sie Datei / Neu / XML-Datei aus dem Eclipse-Menü.
- Stellen Sie sicher, dass HelloUniverse als übergeordnetes Element ausgewählt ist, und geben Sie den Namen build.xml ein (genau so).
- Wählen Sie Fertigstellen
- Kopieren Sie im Quelltext-Ansicht (unten links auswählen) den obigen Code und fügen Sie ihn anstelle der bereits vorhandenen Zeile ein.
- Speichern.

Um dies zum Laufen zu bringen, benötigen Sie ein Build-Werkzeug: Phing.

## Das Phing-Build-Tool¶

Gehen Sie zur [Phing](https://www.phing.info/) Website und laden Sie das Phar-Archiv herunter. Sie können diesen [direkten Link](https://www.phing.info/get/phing-latest.phar) verwenden. Speichern Sie die Datei in einem phing-Ordner in Ihrem persönlichen bin-Ordner /Users/username/bin/phing. Ändern Sie die Dateiberechtigungen auf 755, damit sie von der Kommandozeile aus ausgeführt werden kann. Um zu testen, öffnen Sie ein Terminal, wechseln Sie zu Ihrem bin/phing-Ordner und geben Sie ./phing-latest.phar -v ein, um die Versionsnummer zu sehen. Merken Sie sich den vollständigen Pfad zur Datei:

- /Users/username/bin/phing/phing-latest.phar

Du wirst es später brauchen.

## Eclipse-Einstellungen - Builder

Hier konfigurieren Sie Phing, um Ihr Projekt zu erstellen, wann immer Sie Codeänderungen vornehmen.

- Im Projekt-Explorer wählen Sie das Projekt HelloUniverse aus.
- Wählen Sie die Menüeinträge Datei / Eigenschaften aus.
- Im Eigenschafts-Dialog wählen Sie Ersteller und dann die Schaltfläche Neu aus.
- Im Dialogfeld Konfigurationstyp wählen Sie Programm und dann OK.
- Im Dialogfenster Eigenschaften der Startkonfiguration bearbeiten geben Sie einen geeigneten Titel ein: Build HelloUniverse.
- Im Feld Standort geben Sie den Pfad zum Phing-Programm ein oder verwenden Sie die Schaltfläche Durchsuchen im Dateisystem, um danach zu suchen: /Users/ceford/bin/phing/phing-latest.phar
- Für das Feld Arbeitsverzeichnis wählen Sie die Schaltfläche Durchsuchen im Arbeitsbereich und dann HelloUniverse aus.
- Klicken Sie auf OK, dann auf Übernehmen und Schließen.

Um zu testen: wählen Sie die Menüeinträge Projekt / Projekt erstellen. Die Konsolenansicht unten auf dem Bildschirm zeigt den Fortschritt des Builds an. In diesem Stadium wird wahrscheinlich BUILD FAILED in Rot angezeigt. Das wird erwartet - die Komponentenordner werden bei der Installation der Zip-Datei erstellt. Sie existieren noch nicht, da es keinen Code gibt. Aber es zeigt, dass das Phing-Build-Tool funktioniert.

## So debuggen Sie

Wenn etwas schiefgeht, möchten Sie möglicherweise den Code Zeile für Zeile durchgehen, um zu sehen, was tatsächlich passiert. Wenn Sie das Debug-System auf Ja und die Fehlerberichterstattung auf Maximum in der Joomla Global Configuration eingestellt haben, haben Sie möglicherweise eine Stack-Trace, um zu zeigen, wo ein Fehler im Code auftritt. Oder Sie sehen möglicherweise etwas Ungewöhnliches in der Ausgabe und haben eine gute Vorstellung davon, wo der Fehler liegen könnte. In beiden Fällen müssen Sie Breakpoints setzen, an denen die Ausführung pausiert, um Ihnen zu ermöglichen, die Werte von Variablen zu sehen und den Code Zeile für Zeile durchzugehen.

### Einen Breakpoint setzen

Haltepunkte müssen im Website-Projekt J4Tutorials gesetzt werden. Als Beispiel erweitern Sie im Projekt-Explorer-Panel das J4Tutorials-Projekt und finden administrator / components / com_users / src / Model und doppelklicken Sie auf UsersModel.php, um es in einem Bearbeitungsfenster zu öffnen. Gehen Sie zu Zeile 87 und doppelklicken Sie auf die Nummer 87. Ein kleiner blauer Marker erscheint, um anzuzeigen, dass ein Haltepunkt gesetzt wurde. Die Code-Ausführung sollte hier stoppen, wenn Sie über die Menüeinträge Administrator / Benutzer / Verwalten die Liste der Benutzer anzeigen.

### Eine Debug-Konfiguration erstellen

Aus dem oberen Menü wählen Sie Ausführen / Debug-Konfigurationen. Geben Sie im Formular einen erkennbaren Titel ein, zum Beispiel Debug Admin, um später aus einer Drop-down-Liste auswählen zu können. Stellen Sie sicher, dass die ausgewählte Datei /J4Tutorials/administrator/index.php ist. Wählen Sie im Formular im Bereich "Common" Debug aus der Anzeige im Favoriten-Menü. Überprüfen Sie, ob der Debugger auf Xdebug eingestellt ist. Wählen Sie Übernehmen und Schließen.

Sie können nun Debug-Admin aus der Debug-Dropdown-Liste der Symbolleiste auswählen, dem Symbol mit dem Bug-Symbol.

![Eclipse-Debug-Tools](../../../en/images/getting-started/eclipse-pdt-debug-tools.png)

Debug-Symbole, zum Anzeigen der Beschriftungen darüberfahren:

- **Alle Haltepunkte überspringen** Ein Schalter, um Haltepunkte außer der ersten Zeile von index.php zu überspringen.
- **Fortsetzen** Normale Ausführung bis zum nächsten Haltepunkt fortsetzen.
- **Beenden** Debugging stoppen (aber Sie müssen auch das Xdebug-Cookie entfernen).
- **Hereinspringen** Bei einer Zeile, die einen Funktionsaufruf enthält, in diese Funktion hineinspringen.
- **Darüber springen** Bei einer Zeile, die einen Funktionsaufruf enthält, nicht in diese Funktion hineingehen.
- **Herausspringen** In einer Funktion das Anhalten bei jeder Zeile überspringen und zum Aufrufer zurückkehren.

### Debug-Prozedur

Beim Starten einer Debug-Sitzung pausiert die Ausführung an der ersten Zeile von administrator/index.php. Das ist die Home-Dashboard-Seite. Sie müssen das Resume-Symbol (gelber Balken und grünes rechtes Dreieck) auswählen. Nach einer Verzögerung von etwa einer Sekunde gibt es mehr Aktivität und mehr pausierte Remote-Starts im linken Debug-Admin-Panel. Dies ist die Startseite, die Ajax verwendet, um aktualisierte Informationen abzurufen. Klicken Sie einfach weiter auf Resume, bis die Aktivität gestoppt hat. Wählen Sie dann in Ihrem Browser das Benutzer-Element aus dem Home-Dashboard aus. Eclipse wird aktiv und stoppt die Ausführung an dem von Ihnen gesetzten Haltepunkt.

![Eclipse PHP-Debug-Perspektive](../../../en/images/getting-started/eclipse-pdt-debug-perspective.png)

Merkmale zu beachten:

- Die aktuelle Codezeile ist mit einem blassgrünen Hintergrund markiert.
- Das linke Panel zeigt einen Stack-Trace - die vorherigen Funktionsaufrufe, die zur aktuellen Funktion führten. Wählen Sie ein Element aus, um den übergeordneten Code zu sehen.
- Das rechte Panel kann entweder aktive Variablen oder Haltepunkte anzeigen. Objekte nach Bedarf erweitern, um Details zu sehen.

### Einige Probleme

- Anhalten an Haltepunkten schlägt fehl: Dies geschieht, wenn Sie ein anderes PHP-Programm wie phpMyAdmin öffnen. Um dies zu beheben:
  - Gehen Sie zu Ausführen / Debug-Konfigurationen und wählen Sie Ihr Debug-Admin-Element aus.
  - Wählen Sie im Debugger-Tab Konfigurieren...
  - Wählen Sie in der Pfadzuordnung alle Pfade aus, die nicht mit Ihrer Test-Website zusammenhängen, und entfernen Sie diese.
  - Wählen Sie Fertigstellen - möglicherweise müssen Sie die Debug-Sitzung beenden und neu laden.
- Debugging wird nach Auswahl der Beenden-Schaltfläche fortgesetzt. Um dies zu beheben:
  - Verwenden Sie die Entwickler-Tools Ihres Browsers, um ein Inspektionsfenster zu öffnen.
  - Finden Sie die Cookies, die Ihr Browser verwendet (Speicher in Firefox).
  - Wählen Sie das XDebug-Cookie aus und löschen Sie es.

## Verwendung von Git

Git ist ein Versionskontrollsystem, das häufig zur Verwaltung von Texten jeglicher Art verwendet wird, hauptsächlich Code, aber es kann alles Mögliche sein. Es arbeitet über eine Kommandozeile, wird jedoch normalerweise von grafischen Tools wie Eclipse aufgerufen. Wenn Sie mehr über Git lesen möchten, besuchen Sie die [Git-Website](https://git-scm.com/).

Wenn Sie Git verwenden möchten, müssen Sie es für Ihre Plattform installieren. Wählen Sie dann in Eclipse Ihr HelloUniverse-Projekt, klicken Sie mit der rechten Maustaste darauf und wählen Sie Team / Projekt freigeben. Wählen Sie im Dialogfeld Projekt freigeben die Option **Repository im übergeordneten Ordner des Projekts verwenden oder erstellen** und wählen Sie das HelloUniverse-Projekt aus der Liste. Es gibt ein Feld, das angibt, wo das Repository erstellt wird (/Users/ceford/git/j4xtutorials-com-hellouniverse) - wählen Sie die benachbarte Schaltfläche Repository erstellen. Klicken Sie schließlich auf die Schaltfläche Fertig stellen.

![Git-Repository konfigurieren](../../../en/images/getting-started/eclipse-pdt-configure-git.png)

Sie werden feststellen, dass einige neue Dekorationen neben den Elementen in der Projekt-Explorer-Ansicht erschienen sind:

- Das \> Zeichen zeigt an, dass es einen Inhalt gibt, der geändert, aber nicht im Repository gesichert wurde.
- Das ? Zeichen zeigt an, dass dieses Element nicht zur Liste der zu verfolgenden Elemente hinzugefügt wurde.

![Git-Markierungen](../../../en/images/getting-started/eclipse-pdt-git-markers.png)

Sie sind nun für die Versionskontrolle eingerichtet. Klicken Sie mit der rechten Maustaste auf das Projekt und sehen Sie sich das Menü Team an. Viel zu viel, um es hier abzudecken!

## Schließlich

Bereit, mit der Arbeit an Ihrem eigenen Code zu beginnen?

*Übersetzt von openai.com*

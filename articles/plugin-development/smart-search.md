<!-- Filename: Creating_a_Smart_Search_plug-in / Display title: Beispiel: Intelligente Suche -->

## Einführung

Die Smart Search-Plugins befinden sich in der Gruppe `finder` (in plugins/finder/xxxx). Die Kodierung der Finder-Plugins hat sich in Joomla 4 und 5 erheblich verändert. Um ein neues Finder-Plugin zu erstellen, ist es wahrscheinlich am besten, ein vorhandenes Core-Plugin zu kopieren und seinen Inhalt so zu ändern, dass es dem neuen Zweck entspricht.

Dieser Beitrag beschreibt ein Finder-Plugin, das zur Unterstützung der *Jdocmanual*-Erweiterung entwickelt wurde. Der zu indizierende Inhalt befindet sich in der Tabelle `#__jdm_articles`. Der Code ist auf [GitHub](https://github.com/ceford/cefjdemos-plg-finder-jdocmanual) zu finden.

## IDE-Einrichtung - VSCode oder VSCodium

In meiner lokalen Entwicklungsumgebung befindet sich der Quellcode dieser Erweiterung in ~/git/cefjdemos-plg-jdm-finder. Dieser Ordnername hat keine Bedeutung; es ist nur meine Art, meine eigenen Erweiterungen zu ordnen. Die schematische Struktur des Ordners ist wie folgt:

```
cefjdemos-plg-finder-jdocmanual
|-- .vscode (folder for build settings)
|-- plg_finder_jdocmanual (folder compressed to create an installable zip file)
    |-- language/en-GB/ (language folder, kept with the extension code)
        |-- plg_finder_jdocmanual.ini
        |-- plg_finder_jdocmanual.sys.ini
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Extension (folder for extension specific code)
            |-- Jdocmanual.php (the extension class code)
    |-- jdocmanual.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
```

In VSCodium sieht es so aus:

![Plugin-Entwicklungs-Dateistruktur in vscodium](../../../en/images/plugins/jdocmanual-vscodium.png)

## Den Code anpassen

Nachdem mit einem grundlegenden Finder-Plugin begonnen wurde, besteht die erste Aufgabe darin, alle Verweise auf das ursprüngliche Plugin in geeignete Werte für das neue Plugin zu ändern. In diesem Fall gibt es 63 Instanzen des Wortes `jdocmanual` in 7 Dateien. Es besteht eine gute Chance, dass bei der ersten Durchsicht einige übersehen werden und das Plugin nicht funktioniert.

## Erweiterung erstellen

Es gibt zwei Teile beim Erstellen. Ich habe eine build.xml-Datei, die Anweisungen für [`phing`](https://www.phing.info/) enthält, ein Build-Tool für PHP-Projekte. Es kann von VSCode/VSCodium oder Eclipse oder jeder anderen IDE aufgerufen werden.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="jdocmanual" basedir="." default="main">

    <property name="joomladir" value="/Users/ceford/public_html/jdm3"  override="true" />
    <property name="sitecomdir" value="${joomladir}/components" override="true" />
    <property name="admincomdir" value="${joomladir}/administrator/components" override="true" />
    <property name="adminmediadir" value="${joomladir}/media/com_jdocmanual" override="true" />
    <property name="plgdir" value="${joomladir}/plugins/finder/jdocmanual" override="true" />
    <property name="srcdir" value="${project.basedir}" override="true" />

    <property name="zipsdir" value="/Users/ceford/git/zips/cefjdemos"  override="true" />

    <!-- Fileset for plugin files -->
    <fileset dir="./plg_finder_jdocmanual" id="plgfiles">
        <include name="**" />
    </fileset>

    <!-- fileset for zip -->
    <fileset dir="./plg_finder_jdocmanual" id="plgfiles">
        <include name="**" />
    </fileset>

    <!-- ============================================  -->
    <!-- (DEFAULT) Target: main                        -->
    <!-- ============================================  -->
    <target name="main" description="main target">

        <copy todir="${plgdir}">
            <fileset refid="plgfiles" />
        </copy>

        <zip destfile="${zipsdir}/plg_finder_jdocmanual.zip">
            <fileset refid="plgfiles" />
        </zip>

    </target>
</project>
```

Der zweite Teil ist eine `tasks.json`-Datei im .vscode-Ordner. Sie teilt VSCode mit, wo sich die Phing-Datei befindet.

```json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
      {
        "label": "Build plg_finder_jdocmanual",
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

In VSCode/VScodium wähle ich **Terminal / Build-Aufgabe ausführen...** aus der Menüleiste und wähle die entsprechende Aufgabe.

## Die Erweiterung installieren

Nach dem Build muss die ZIP-Datei der Erweiterung wie jede andere Erweiterung installiert werden. Sie muss aktiviert und alle Optionen müssen eingestellt werden. Jdocmanual hat drei Optionen für *Taxonomien zum Indizieren*:

- **Handbuch** eines oder alle verfügbaren Handbücher (Benutzer, Entwickler, ...)
- **Sprache** Suchvorgänge sind auf Beiträge in derselben Sprache wie die Seitensprache beschränkt.
- **Typ** eine oder alle Arten von Inhalten (Jdocmanual, Inhalt, ...)

Im Fall der Jdocmanual-Website sind andere Finder-Plugins deaktiviert, da es keine weiteren Inhalte gibt, die indexiert werden sollen.

Nach der ersten Installation reicht es normalerweise aus, die Build-Aufgabe nach einer Korrektur des Codes erneut auszuführen. Die Installation der ZIP-Datei ist nur erforderlich, wenn es Änderungen an der manifest.xml-Datei gibt.

## Inhalte indizieren

Jdocmanual ist so eingerichtet, dass es seine Beiträge indiziert, wenn dies von einem Super User angefordert wird. Dies unterscheidet sich vom normalen Inhalts-Plugin, das Inhalte jedes Mal indiziert, wenn ein Beitrag gespeichert wird. Ein erster Durchlauf dauert einige Minuten, um die ~5000 Jdocmanual-Beiträge in 8 Sprachen zu indizieren.

## Ein Smart-Suchmodul erstellen

Aus dem **Inhalt / Website-Module** wählen Sie **Neu** und installieren Sie ein neues **Smart Search**-Modul. Passen Sie die Einstellungen an, um ein geeignetes Erscheinungsbild zu erhalten.

## Testen

Letztendlich sollte Ihr benutzerdefiniertes Smart-Suche-Plugin funktionieren. Dies ist eine Beispiel-Ergebnisseite für Jdocmanual, die nach einem Begriff auf dieser Seite sucht. Die Ergebnisseite lässt das Suchformular in der Titelleiste aus, da es im Seiteninhalt vorhanden ist.

![Intelligentes Suchergebnis](../../../en/images/plugins/jdocmanual-search-result.png)

Ein Hinweis: Das System - Joomla Accessibility Checker-Plugin zeigt, dass es 3 Fehler im Zusammenhang mit dem *Suchbegriffe* Dateneingabeformular gibt. Das benötigt eine Kernkorrektur oder ein Override.

*Übersetzt von openai.com*

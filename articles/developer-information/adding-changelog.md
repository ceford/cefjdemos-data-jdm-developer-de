<!-- Filename: Adding_changelog_to_your_manifest_file / Display title: Hinzufügen eines Änderungsprotokolls -->

Seit Joomla 4.0 können Erweiterungsentwickler die Fähigkeit von Joomla nutzen, eine Änderungsdatei zu lesen und eine visuelle Darstellung der Änderungen zu geben. Wenn eine bestimmte Version nicht in der Änderungsdatei gefunden wird, wird der Änderungsprotokoll-Button nicht angezeigt.

Die Änderungen in einem Release werden in dieser Weise präsentiert:

![changelog modal view](../../../en/images/developer-information/adding-changelog-example-1.png)

Das Änderungsprotokoll wird an zwei verschiedenen Orten verwendet.

## Ansicht aktualisieren

Der Installationsprogramm zeigt das Änderungsprotokoll der Version an, die installiert werden kann, falls verfügbar.

![changelog installer view](../../../en/images/developer-information/adding-changelog-update-view.png)

Durch Klicken auf die Schaltfläche Changelog wird das Änderungsprotokoll der neuen verfügbaren Version angezeigt.

## Ansicht verwalten

Der Erweiterungsmanager zeigt das Änderungsprotokoll der derzeit installierten Erweiterung an, falls verfügbar.

![changelog installer view](../../../en/images/developer-information/adding-changelog-extension-view.png)

Durch Klicken auf die Versionsnummer hier wird das Änderungsprotokoll der aktuell installierten Version angezeigt.

## Tag changelogurl zu den Manifestdateien hinzufügen

Der erste Schritt besteht darin, Ihre Manifestdateien zu aktualisieren, die Joomla mitteilen, wo sich die Changelog-Details befinden. Fügen Sie den folgenden Knoten zu Ihren Manifest-XML-Dateien hinzu:

```
<changelogurl>https://example.com/updates/changelog.xml</changelogurl>
```

Bitte beachten: Die URL im changelogurl-Tag darf keine Leerzeichen oder Zeilenumbrüche davor oder danach haben. Siehe Codebeispiele.

### Servermanifest aktualisieren

Sehen Sie sich dieses Beispiel für eine Aktualisierungs-Server-Manifestdatei an, die Joomla über ein Update einer Komponente namens "com_lists" informiert. So werden Sie die Changelog-Schaltfläche in der Aktualisierungsansicht sehen.

```
<?xml version="1.0" encoding="utf-8"?>
<updates>
 <update>
  <name>Student List</name>
  <description>List of students</description>
  <element>com_lists</element>
  <type>component</type>
  <version>4.0.0</version>

  <changelogurl>https://example.com/updates/changelog.xml</changelogurl>

  <tags>
   <tag>stable</tag>
  </tags>
  <maintainer>Example Miller</maintainer>
  <maintainerurl>https://example.com/</maintainerurl>
  <section>Updates</section>
  <targetplatform name="joomla" version="4.?" />
  <client>1</client>
  <folder></folder>
 </update>
</updates>
```

### Erweiterungsmanifest

Fügen Sie zusätzlich das changelogurl-Tag zur Erweiterungsmanifest-XML hinzu. Somit wird die Erweiterungsversion in der Verwaltungsansicht mit den Änderungsprotokollen verlinkt.

```
<?xml version="1.0" encoding="utf-8"?>
<extension type="component" method="upgrade">
    <name>COM_LISTS</name>

... Other stuff ...

    <changelogurl>https://example.com/updates/changelog.xml</changelogurl>

    <updateservers>
        <server type="extension" name="My Extension's Updates">https://example.com/lists-updates.xml</server>
    </updateservers>
</extension>
```

## Änderungsprotokolldatei erstellen

Die Änderungsprotokolldatei muss die folgenden 3 Knoten haben:

* Element
* Typ
* Version

Diese Informationen werden verwendet, um das korrekte Änderungsprotokoll für eine bestimmte Erweiterung zu identifizieren.

Ein Versionsknoten innerhalb eines beliebigen Changelog-Knotens ist immer obligatorisch. Ansonsten wird eine Fehlermeldung wie SyntaxError: JSON.parse: unerwartetes Zeichen in Zeile 1 Spalte 1 der JSON-Daten angezeigt.

```
<element>com_lists</element>
<type>component</type>
<version>4.0.0</version>
```

Weiterhin wird das Changelog mit einem oder mehreren Änderungstypen gefüllt. Die folgenden Änderungstypen werden unterstützt:

* Sicherheit: Alle Sicherheitsprobleme, die behoben wurden
* Fehlerbehebung: Alle behobenen Fehler
* Sprache: Dies betrifft Sprachänderungen
* Ergänzung: Alle neuen Funktionen, die hinzugefügt wurden
* Änderung: Alle Änderungen
* Entfernen: Alle entfernten Funktionen
* Hinweis: Zusätzliche Informationen, um den Benutzer zu informieren

Jeder Knoten kann so oft wie nötig wiederholt werden.

Der Format des Textes kann einfacher Text oder HTML sein, aber im Fall von HTML muss er in CDATA-Tags eingeschlossen werden, wie im Beispiel gezeigt.

```
<changelogs>
    <changelog>
        <element>com_lists</element>
        <type>component</type>
        <version>4.0.0</version>
        <security>
            <item>Item A</item>
            <item><![CDATA[<h2>You MUST replace this file</h2>]]></item>
        </security>
        <fix>
            <item>Item A</item>
            <item>Item b</item>
        </fix>
        <language>
            <item>Item A</item>
            <item>Item b</item>
        </language>
        <addition>
            <item>Item A</item>
            <item>Item b</item>
        </addition>
        <change>
            <item>Item A</item>
            <item>Item b</item>
        </change>
        <remove>
            <item>Item A</item>
            <item>Item b</item>
        </remove>
        <note>
            <item>Item A</item>
            <item>Item b</item>
        </note>
</changelog>
<changelog>
    <element>com_lists</element>
    <type>component</type>
    <version>0.0.2</version>
    <security>
        <item>Big issue</item>
    </security>
</changelog>
</changelogs>
```

Diese Datei enthält 2 Änderungsprotokolle:

* Version 0.0.2 (zum Testen der Verwaltungsansicht)
* Version 4.0.0 (zum Testen der Aktualisierungsansicht)

Ein Änderungsprotokoll kann so viele Versionen haben, wie benötigt.

*Übersetzt von openai.com*

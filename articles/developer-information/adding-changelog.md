<!-- Filename: Adding_changelog_to_your_manifest_file / Display title: Changelog zur Manifest-Datei hinzufügen -->

Seit Joomla 4.0 können Extension-Entwickler die Fähigkeit von Joomla nutzen, eine Changelog-Datei zu lesen und eine visuelle Darstellung des Changelogs zu geben. Wenn eine bestimmte Version nicht im Changelog gefunden wird, wird die Schaltfläche Changelog nicht angezeigt.

Die Änderungen in einer Version werden so dargestellt:

<img alt="Changelog modal" src="https://docs.joomla.org/images/thumb/7/7a/Changelog_modal-en.png/700px-Changelog_modal-en.png" decoding="async" width="700" height="461" srcset="https://docs.joomla.org/images/thumb/7/7a/Changelog_modal-en.png/1050px-Changelog_modal-en.png 1.5x, https://docs.joomla.org/images/thumb/7/7a/Changelog_modal-en.png/1400px-Changelog_modal-en.png 2x" data-file-width="1618" data-file-height="1066">

Das Changelog wird an zwei verschiedenen Stellen verwendet.

## Update Ansicht

Das Installationsprogramm zeigt das Changelog der Version an, die installiert werden kann, falls verfügbar.

<img alt="Changelog button on the Update View" src="https://docs.joomla.org/images/thumb/7/79/Update_view_changelog_button-en.png/700px-Update_view_changelog_button-en.png" decoding="async" width="700" height="152" srcset="https://docs.joomla.org/images/thumb/7/79/Update_view_changelog_button-en.png/1050px-Update_view_changelog_button-en.png 1.5x, https://docs.joomla.org/images/7/79/Update_view_changelog_button-en.png 2x" data-file-width="1282" data-file-height="278">

Durch klicken auf die AAAA-Schaltfläche wird das Änderungsprotokoll der neu verfügbaren Version angezeigt.

## Manager-Ansicht

Der Erweiterungs-Manager zeigt das Änderungsprotokoll der aktuell installierten Erweiterung an, falls verfügbar.

<img alt="Version number is a link to the changelog modal" src="https://docs.joomla.org/images/thumb/4/4b/Manage_view_changelog_link-en.png/700px-Manage_view_changelog_link-en.png" decoding="async" width="700" height="81" srcset="https://docs.joomla.org/images/thumb/4/4b/Manage_view_changelog_link-en.png/1050px-Manage_view_changelog_link-en.png 1.5x, https://docs.joomla.org/images/4/4b/Manage_view_changelog_link-en.png 2x" data-file-width="1274" data-file-height="148">

Durch klicken auf die Versionsnummer wird das Änderungsprotokoll der aktuell installierten Version angezeigt.

## Ein Changelog-URL-Tag in Manifest-Dateien einfügen

Der erste Schritt besteht darin, Ihre Manifestdateien zu aktualisieren, die Joomla mitteilen, wo die Changelog-Details zu finden sind. Fügen Sie Ihren Manifest-XML-Dateien den folgenden Node hinzu:

```
<changelogurl>https://example.com/updates/changelog.xml</changelogurl>
```

Bitte beachten: Die im changelogurl-Tag eingetragene URL darf davor und danach keine Leerzeichen oder Zeilenumbrüche enthalten. Siehe Code-Beispiele.

### Update-Server Manifest-Datei

Dieses Beispiel zeigt eine Manifestdatei für den Update-Server, die Joomla über ein Update einer Komponente namens "com_lists" informiert. Dadurch sehen Sie in der Update-Ansicht die Schaltfläche Changelog.

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

### Erweiterung-Manifest-Datei

Fügen Sie zusätzlich das Changelog-URL-Tag zur „Extension Manifest XML“ hinzu. Somit wird die Erweiterungsversion mit den Änderungsprotokollen in der Manager-Ansicht verknüpft.

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
## Changelog-Datei erstellen

Die Changelog-Datei muss die folgenden 3 Nodes enthalten:

* element
* type
* version

Diese Informationen werden verwendet, um das richtige Changelog für eine bestimmte Erweiterung zu identifizieren.

Ein version-Node innerhalb eines changelog-Node ist immer erforderlich. Andernfalls erhalten Sie eine Fehlermeldung wie SyntaxError: JSON.parse: unerwartetes Zeichen in Zeile 1, Spalte 1 der JSON-Daten.

```
<element>com_lists</element>
<type>component</type>
<version>4.0.0</version>
```

Weiter wird das Changelog mit einer oder mehreren Änderungsarten gefüllt. Die folgenden Änderungs-Typen werden unterstützt:

* security: Etwaige Sicherheitsprobleme, die behoben wurden
* fix: Fehler, die behoben wurden
* language: Dieser gilt für Sprachänderungen
* addition: Neu hinzugefügte Features
* change: Andere Änderungen
* remove: Aus der Erweiterung entfernte Features
* note: Ergänzende Informationen für den Benutzer

Jeder Node kann mehrfach verwendet werden, falls notwendig.

Das Format des Texts kann reiner Text oder HTML sein. Bei HTML muss der Text, wie im Beispiel gezeigt, in CDATA-Tags eingeschlossen sein.

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

Diese Datei enthält 2 Changelogs:

* Version 0.0.2 (zum Testen der Manager-Ansicht)
* Version 4.0.0 (zum Testen der Update-Ansicht)

Ein Changelog kann beliebig viele Versionen enthalten.

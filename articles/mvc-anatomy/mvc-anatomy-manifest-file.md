<!-- Filename: J4.x:MVC_Anatomy:_Manifest_File / Display title: MVC Anatomie: Manifestdatei -->

## Metadaten

Die Komponentendatei muss manifest.xml oder componentname.xml genannt werden, in diesem Fall countrybase.xml. Beachten Sie, dass der com_ Teil nicht enthalten ist. Der erste Teil der Datei enthält Metadaten:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="component" method="upgrade">
    <name>com_countrybase</name>
    <author>Clifford E Ford</author>
    <authorEmail>john.doe@example.com</authorEmail>
    <authorUrl>example.com</authorUrl>
    <creationDate>2025-01-02</creationDate>
    <copyright>(C) 2025 Clifford E Ford</copyright>
    <license>GNU General Public License version 3 or later; see LICENSE.txt</license>
    <version>0.0.1</version>
    <description>COM_COUNTRYBASE_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Component\Countrybase</namespace>
```

Der größte Teil der Metadaten ist selbsterklärend. Zu beachtende Punkte:

- Der Erweiterungstyp in diesem Fall ist **Komponente**. Es gibt andere Erweiterungstypen, wie Modul oder Plugin.
- Die Methode kann Installation oder Upgrade sein. Letzteres ermöglicht eine Neuinstallation nach einem Code-Update.
- Die Versionsnummer ist wichtig! Sie sollte es unmöglich machen, eine ältere Version erneut zu installieren. Und sie wird verwendet, um zu steuern, ob Datenbankaktualisierungen erforderlich sind.
- Der Namespace-Pfad src hat drei Teile:
  - Der erste Teil ist ein vom Anbieter definierter Präfix, also Joomla für von Joomla bereitgestellten Code, Acme für Code vom Drittanbieter-Erweiterungslieferanten Acme und in diesem Fall Cefjdemos, ein Name, der für Joomla-Code-Demonstrationen gewählt wurde.
  - Der zweite Teil gibt den Typ der Erweiterung an. Es könnte Komponente oder Modul oder Plugin oder ... sein.
  - Der dritte Teil ist der spezifische Erweiterungsname, in diesem Fall Countrybase.

## Datenbank

Die Manifestdatei definiert die Speicherorte aller Installations-, Aktualisierungs- oder Entfernungs-SQL-Dateien. Sie landen in einem SQL-Ordner im Administrator-Ordner.

```xml
    <install>
        <sql>
            <file driver="mysql" charset="utf8">sql/install.mysql.sql</file>
        </sql>
    </install>
    <uninstall>
        <sql>
            <file driver="mysql" charset="utf8">sql/uninstall.mysql.sql</file>
        </sql>
    </uninstall>
    <update>
        <schemas>
            <schemapath type="mysql">sql/updates/mysql</schemapath>
        </schemas>
    </update>
```

## Skriptdatei

Eine Skriptdatei kann für Pre-Flight- oder Post-Flight-Zwecke verwendet werden. Zum Beispiel könnte sie verwendet werden, um die minimalen Systemanforderungen vor der Installation zu überprüfen oder um Parameter nach der Installation festzulegen.

## Mediendateien

Der com_countrybase-Komponente benötigt keine komponentenspezifischen CSS- oder JavaScript-Dateien, aber der Code, um sie zu installieren, ist für alle Fälle enthalten. Die CSS- und JS-Ordner enthalten nur leere index.html-Dateien. Ohne diese Dateien werden die Ordner nicht erstellt.

```xml
    <scriptfile>script.php</scriptfile>

    <media destination="com_countrybase" folder="media">
        <file>joomla.asset.json</file>
        <folder>css</folder>
        <folder>js</folder>
    </media>
```

Beachten Sie, dass die Datei joomla.asset.json vom Web-Asset-Manager verwendet wird, der normalerweise aus einer Template-Datei aufgerufen wird.

## Website-Dateien

Dies sind die Dateien, die nach root/components/com_countrybase kopiert werden. Beachten Sie, dass der Sprachordner in den Komponentenordner und nicht in den root/language-Ordner kopiert wird.

```xml
    <files folder="site">
        <folder>language</folder>
        <folder>forms</folder>
        <folder>src</folder>
        <folder>tmpl</folder>
    </files>
```

## Administrator-Dateien

Es gibt mehr Dateien im Verwaltungsteil der Manifestdatei, weil er mehr zu tun hat:

```xml
    <administration>
        <files folder="admin">
            <filename>access.xml</filename>
            <filename>config.xml</filename>
            <folder>forms</folder>
            <folder>help</folder>
            <folder>language</folder>
            <folder>layouts</folder>
            <folder>services</folder>
            <folder>sql</folder>
            <folder>src</folder>
            <folder>tmpl</folder>
        </files>
        <menu img="class:default">countrybase</menu>
        <submenu>
            <!--
                Note that all & must be escaped to &amp; for the file to be valid
                XML and be parsed by the installer
            -->
            <menu
                link="option=com_countrybase&amp;view=countries"
                img="default"
            >
                COM_COUNTRYBASE_COUNTRIES
            </menu>
            <menu
                link="option=com_countrybase&amp;view=currencies"
                img="default"
            >
                COM_COUNTRYBASE_CURRENCIES
            </menu>
        </submenu>
    </administration>
```

Beachten Sie die Methode zur Erstellung der Joomla-Administrator-Menüpunkte. Es gibt einen Menüpunkt, der keinen Link hat. Und zwei Menüpunkte, die auf die beiden verfügbaren Administrationsansichten verweisen: die Länderliste und die Währungsliste. Außerdem müssen die Zeichenkettenübersetzungen in der Datei com_countrybase.sys.ini sein.

## Änderungsprotokoll

Das Änderungsprotokoll wird verwendet, um Änderungen jeder Version der Erweiterung zu dokumentieren. Weitere Informationen zum Inhalt des Änderungsprotokolls finden Sie im [Änderungsprotokoll](jdocmanual?article=docus/install-update/installation-change-log) Beitrag.

```xml
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-com-countrybase/master/changelog.xml</changelogurl>
```

## Server aktualisieren

Der Aktualisierungsservercode teilt Joomla mit, wo Aktualisierungsinformationen zu finden sind. Er wird in regelmäßigen Abständen ausgeführt, um festzustellen, ob eine Aktualisierung verfügbar ist. Lassen Sie diesen Abschnitt aus, wenn Sie keinen Aktualisierungsserver für Ihre eigene Komponente verwenden.

```xml
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" name="Countrybase Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-com-countrybase/master/updates.xml</server>
    </updateservers>
```

*Übersetzt von openai.com*


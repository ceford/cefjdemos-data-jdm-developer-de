<!-- Filename: Manifest_files / Display title: Manifest-Dateien -->

Joomla!  4.x Joomla!  3.x Joomla!  2.5

Innerhalb von Joomla gibt es Manifestdateien für alle Erweiterungen.
Diese Dateien enthalten die allgemeinen Installationsinformationen sowie
Parameter für die Konfiguration der
[Erweiterung](https://docs.joomla.org/extension "Special:MyLanguage/extension").
Seit Joomla! 2.5 gibt es nur sehr wenige Unterschiede zwischen den
Manifest-Dateiformaten für die verschiedenen [types of
extensions](https://docs.joomla.org/Extension_types_(technical_definitions) "Special:MyLanguage/Extension types (technical definitions)"),
so dass jeder Typ auf die volle Leistung des Joomla! Installers
zugreifen kann.

## Namenskonventionen

Die Datei **muss** `manifest.xml` (Joomla-Versionen 2.5 und 3) oder
`.xml` (Joomla-Versionen 2.5, 3 und 4) benannt werden und im
Root-Verzeichnis des Installations-Pakets liegen.

Joomla 4.x: Wenn der Dateiname `manifest.xml` für das
Erweiterungs-Manifest verwendet wird, schlägt das automatische
Namespace-Mapping fehl. Siehe:
<a href="https://github.com/joomla/joomla-cms/issues/37750"
class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla/joomla-cms/issues/37750</a>

## Die Syntax

### Das Root-Element

Das primäre Tag der Installationsdatei lautet:

Dieses Start- und End-Tag ist für alle Erweiterungen gleich. Die
folgenden Attribute sind innerhalb des Tags zulässig:

<table class="wikitable">

<tbody>
<tr class="header">
<th>Attribute</th>
<th>Werte</th>
<th>anwendbar bei</th>
<th>Beschreibung</th>
</tr>
&#10;<tr class="odd">
<td>type</td>
<td><code>component</code><br />
<code>file</code><br />
<code>language</code><br />
<code>library</code><br />
<code>module</code><br />
<code>package</code><br />
<code>plugin</code><br />
<code>template</code><br />
<code>element</code></td>
<td>Alle Erweiterungen</td>
<td>Dieses Attribut beschreibt den Typ der Erweiterung für das
Installationsprogramm. Basierend auf diesem Typ gelten weitere
Anforderungen an Sub-Tags.</td>
</tr>
<tr class="even">
<td>version</td>
<td><code>2.5</code><br />
<code>3.0</code></td>
<td>Alle Erweiterungen</td>
<td>Der String identifiziert die Joomla-Version für die diese
Erweiterung entwickelt wurde. Er wird in <img
src="https://docs.joomla.org/images/4/4d/Compat_icon_3_x.png"
decoding="async" data-file-width="40" data-file-height="17" width="40"
height="17" alt="Joomla 3.x" /> nicht verwendet und wurde aus <img
src="https://docs.joomla.org/images/f/f4/Compat_icon_4_0.png"
decoding="async" data-file-width="40" data-file-height="17" width="40"
height="17" alt="Joomla 4.0" /> und höher entfernt.</td>
</tr>
<tr class="odd">
<td>method</td>
<td><code>install</code><br />
<code>upgrade</code></td>
<td>Alle Erweiterungen</td>
<td>Der Standardwert <code>install</code> wird auch verwendet, wenn das
method-Attribut nicht verwendet wird. Der Wert <code>install</code>
bedeutet, daß das Installationsprogramm kontrolliert beendet wird, wenn
vorhandene Dateien/Ordner der neuen Erweiterung gefunden werden.</td>
</tr>
<tr class="even">
<td>client</td>
<td><code>site</code><br />
<code>administrator</code></td>
<td>Module</td>
<td>Mit dem client-Attribut gibt man an, für welchen Anwendungs-Client
(Front- oder Back-End) das neue Modul verfügbar ist.</td>
</tr>
<tr class="odd">
<td>group</td>
<td><em>string</em></td>
<td>Plugins</td>
<td>Der Gruppenname gibt an, für welche Plugin-Gruppe das neue Plugin
verfügbar ist. Die vorhandenen Gruppen sind die Ordnernamen im
Verzeichnis <code>/plugins</code>. Das Installationsprogramm erstellt
neue Ordnernamen für Gruppennamen, die noch nicht existieren.</td>
</tr>
</tbody>
</table>

### Metadaten

Die folgenden Elemente können zum Einfügen von Metadaten verwendet
werden. Keines dieser Elemente ist erforderlich. Wenn sie vorhanden
sind, müssen sie ein untergeordnetes Element des Root-Elements sein.

     – raw component name (e.g. com_banners).
     – author's name (e.g. Joomla! Project)
     – date of creation or release (e.g. April 2006)
     – a copyright statement (e.g. (C) 2005 - 2011 Open Source Matters. All rights reserved.)
     – a license statement (e.g. NU General Public License version 2 or later; see LICENSE.txt)
     – author's email address (e.g. admin@joomla.org)
     – URL to the author's website (e.g. www.joomla.org)
     – the version number of the extension (e.g. 1.6.0)
     – the description of the component. This is a translatable field. (e.g. COM_BANNERS_XML_DESCRIPTION)
     – the internal name of the component. If omitted, name will be cleaned and used

Hinweis: Die und Tags sind ebenfalls übersetzbare Felder, so dass der
Name und die Beschreibung der Erweiterung dem Benutzer in seiner
Muttersprache angezeigt werden können.

### Front-End Dateien

```xml
	<files folder="from-folder">
		<filename>example.php</filename>
		<folder>examples</folder>
	</files>
```

Dateien, die in das Front-End-Verzeichnis kopiert werden sollen, sollten
im -Element abgelegt werden. Sie können das optionale `folder`-Attribut
verwenden, um ein Verzeichnis **im ZIP-Paket** anzugeben, **aus dem**
kopiert werden soll. Jede zu kopierende Datei muss durch ein -Element
dargestellt werden. Wenn Sie den gesamten Ordner auf einmal kopieren
möchten, können Sie ihn mit definieren.

Bei **Plugins** sollte der Raw-Name des Plugins im `plugin`-Attribut des
-Elements stehen, das auf die Datei mit der Klasse des Plugins verweist.
Wenn Sie z.B. ein System-Plugin mit dem Namen „example“ (vollständiger
Name `plg_system_example`) erstellt haben, dann verwenden Sie
`example.php`.

### Medien Dateien

```xml
	<media folder="media" destination="com_example">
		<filename>com_example_logo.png</filename>
		<folder>css</folder>
		<folder>js</folder>
	</media>
```

In diesem Beispiel werden die aufgelistete(n) Datei(en)
(`/media/com_example_logo.png`) und die Ordner (`/media/css/` und
`/media/js/`) nach `/media/com_example/` kopiert, wobei bei Bedarf der
Ordner `com_example` erstellt wird. Sie können das optionale
`folder`-Attribut verwenden, um **im ZIP-Paket** ein Verzeichnis
anzugeben **aus dem** kopiert werden soll (in diesem Fall `media`).

Erweiterungen sollten Assets in `media` speichern, die sie benötigen um
über das Web erreichbar zu sein (JS, CSS, Bilder usw.). Unter anderem
wurde diese Funktion hinzugefügt, um den Multi-Site-Support und die
eventuelle Verlagerung von Code-Dateien (PHP) aus den im Web
zugänglichen Bereichen des Servers zu beschleunigen.
Hinweis: Der media-Abschnitt wird nicht nach Erweiterungen vom Typ
„package“ analysiert.

Ref:

- <a
  href="https://groups.google.com/forum/#!msg/joomla-dev-cms/4CAASJqFY-k/PvPj14gP29EJ"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Google Groups - joomla-dev-cms
  thread</a>
- <a
  href="https://groups.google.com/forum/#!msg/joomla-dev-cms/uNmhX98sKbE/p8p68Jke680J"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Google Groups - joomla-dev-cms
  thread</a>

### Der Administrations-Abschnitt

```xml
	<administration>
		<!-- various elements -->
	</administration>
```

Der Administrationsbereich ist im -Element definiert. Da nur
[Komponenten](https://docs.joomla.org/Component "Special:MyLanguage/Component")
für
[Administrator](https://docs.joomla.org/Site_(Application) "Special:MyLanguagehttps://docs.joomla.org/Administrator_(Application)")
gelten, können **nur Manifest-Dateien von Komponenten dieses Element
enthalten**.

#### Back-End Dateien

Dateien, die in das Back-End-Verzeichnis kopiert werden sollen, sollten
im Element unter abgelegt werden. Mit dem optionalen `folder`-Attribut
können Sie ein Verzeichnis **im ZIP-Paket** angeben, **aus dem** kopiert
werden soll. Weitere Regeln finden Sie unter *Front-End Dateien*.

#### Menü-Links und Untermenüs

**Versions-Hinweis:** noch vor Joomla 3.4 führte das Fehlen eines

-Tags in Ihrer XML-Manifestdatei immer dazu, dass ein Menüelement
erstellt wurde. Dieser Fehler wurde in Joomla 3.4 behoben. Wenn Ihre
Manifestdatei also kein

-Tag enthält, wird für die Komponente kein Admin-Menüelement erstellt.

```xml
	<menu>COM_EXAMPLE</menu>
	<submenu>
		<!--
			Note that all & must be escaped to &amp; for the file to be valid
			XML and be parsed by the installer
		-->
		<menu link="anoption=avalue&amp;anoption1=avalue1">COM_EXAMPLE_SUBMENU_ANOPTION</menu>
		<menu view="viewname">COM_EXAMPLE_SUBMENU_VIEWNAME</menu>
	</submenu>
```

Der Text für das Hauptmenü-Element der Komponente wird im

-Element definiert, einem untergeordneten Element von . Es kann auch ein
-Element vorhanden sein (ebenfalls ein untergeordnetes Element von ),
das noch weitere von

definierte Menüelemente enthalten kann.

Darüber hinaus kann jedes

-Element die folgenden Attribute definieren:

<table class="wikitable">

<tbody>
<tr class="header">
<th>Attribute</th>
<th>Beschreibung</th>
</tr>
&#10;<tr class="odd">
<td>link</td>
<td>Ein Link, an den der Benutzer weitergeleitet werden soll, wenn auf
das Menüelement geklickt wird. Alternativ kann auch „view” verwendet
werden.</td>
</tr>
<tr class="even">
<td>img</td>
<td>Der (relative) Pfad zu einem Bild (16x16 Pixel), das neben dem
Menü-Element angezeigt wird.
<p><u>Muss auch eine URL-kompatible Datei sein (z.B. keine
Leerzeichen)!</u></p></td>
</tr>
<tr class="odd">
<td>alt</td>
<td></td>
</tr>
<tr class="even">
<td>view</td>
<td>Ein URL-Parameter, der dem Link hinzugefügt werden soll.
Beispielsweise würde
COM_EXAMPLE
im XML-Manifest von com_example dazu führen, dass die URL des
Menü-Elements <code>index.php?option=com_example&amp;view=cpanel</code>
lautet. Alternativ kann auch „link“ verwendet werden.</td>
</tr>
</tbody>
</table>

Der Wert innerhalb des Tags ist die Bezeichnung des Menüs. Anders als in
Joomla! 1.5, können Sie keine Zeichenfolge in natürlicher Sprache
verwenden. Wenn Sie beispielsweise „Example Component“ anstelle von
COM_EXAMPLE eingeben, wird Ihr Komponentenname als Beispielkomponente im
Menü angezeigt und Sie können keine Übersetzung bereitstellen. Um eine
Übersetzung bereitzustellen, müssen Sie eine Datei mit dem Namen
en-GB.com_example.sys.ini in administrator/languages/en-GB oder in
administrator/components/com_example/language/en-GB erstellen (Sie
können das -Tag des Manifests verwenden, um sie während der Installation
zu kopieren). In letzterem Fall darf die Übersetzungsdatei nicht im -Tag
enthalten sein. Solange Sie das Sprachverzeichnis in Ihrem -Tag
gespeichert haben, wird es bei der Installation der Komponente
mitkopiert.

Der Inhalt dieser Datei sollte sein:

```ini
    COM_EXAMPLE="Example Component"
    COM_EXAMPLE_SUBMENU_ANOPTION="Another Option"
    COM_EXAMPLE_SUBMENU_VIEWNAME="Another View"
```

Bitte beachten, dass der Sprachstring in doppelte Anführungszeichen
eingeschlossen werden muss, wie es die Übersetzungsstandards von Joomla!
vorsehen.

Joomla! 1.6 und spätere Versionen sortieren die Menüpunkte der
Komponenten auf der Grundlage der tatsächlichen Übersetzung des
Schlüssels, der im XML-Manifest angeben ist. Das bedeutet, dass die
Sortierreihenfolge korrekt ist, unabhängig davon, wie der
Übersetzungsschlüssel benannt ist und in welcher Sprache die Website
angezeigt wird. Im Wesentlichen hat Joomla! 1.6 die falsche Sortierung
des Komponentenmenüs unter Joomla! 1.5 für die Mehrheit der (nicht
Englisch sprechenden!) Joomla!-Nutzer korrigiert.

### Dashboard

Spezifiziert die Details für die Darstellung eines Dashboards für die
Komponente im Administratorbereich der Website.

- Neben dem Administrator-Menüpunkt für die Komponente wird ein
  Dashboard-Symbol angezeigt.
- Das Dashboard-Symbol zeigt die Module an, die der Position des
  Administratormoduls cpanel-example zugeordnet sind.
- Der Titel und das Symbol, welche in der XML-Datei definiert sind,
  werden als Überschrift und Symbol am oberen Rand der Dashboard-Seite
  der Komponente verwendet.

```xml
	<dashboards>
		<dashboard title="COM_EXAMPLE_DASHBOARD_TITLE" icon="icon-lock">example</dashboard>
	</dashboards>
```

### Konfiguration

<img
src="https://docs.joomla.org/images/thumb/4/41/Stop_hand_nuvola.svg.png/25px-Stop_hand_nuvola.svg.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/4/41/Stop_hand_nuvola.svg.png/38px-Stop_hand_nuvola.svg.png 1.5x, https://docs.joomla.org/images/4/41/Stop_hand_nuvola.svg.png 2x"
data-file-width="40" data-file-height="40" width="25" height="25"
alt="Stop hand nuvola.svg.png" />Warning!

Die Komponenten **unterstützen in der Manifest-Datei keine**
Konfigurationsdefinitionen. Das war eine Möglichkeit, die in Joomla! 1.5
implementiert wurde. Konfigurationsoptionen für mehrere Ebenen können
mit der Datei config.xml definiert werden. Wie diese zu der Komponente
hinzugefügt werden kann, steht im [Tutorial zur Entwicklung einer
MVC-Komponente](https://docs.joomla.org/J3.x:Developing_an_MVC_Component/Adding_configuration "Special:MyLanguage/J3.x:Developing an MVC Component/Adding configuration").

Das Element , ein Kind des Root-Elements, beschreibt die
Konfigurationsoptionen für die Erweiterung. Falls zutreffend, werden die
Optionen vom entsprechenden Manager (Plugin-Manager, Modul-Manager oder
Template-Manager) angezeigt. **Konfigurationsoptionen für Komponenten
werden in einer separaten Datei (`config.xml`) definiert. Ihr
Root-Element sollte lauten, Plugins und Module verwenden den Abschnitt
in der Manifest-Datei der Erweiterung.**

Jedes Fieldset muss ein oder mehrere -Elemente enthalten, die jeweils
ein einzelnes [form
field](https://docs.joomla.org/form_field "Special:MyLanguage/form field")
mit einer Bezeichnung darstellen. Eine Liste der zulässigen
Formularfeldtypen und Beispiele für XML-Formularfelddefinitionen finden
Sie unter
[Standard-Formular-Feldtypen](https://docs.joomla.org/Standard_form_field_types "Special:MyLanguage/Standard form field types").

### Namensraum

Festlegen des Namensraums, der für das automatische Laden des Plugins
verwendet werden soll. Der angegebene Namensraum wird standardmäßig in
das Stammverzeichnis der Erweiterung geladen. Optional kann jedoch ein
"path"-Attribut zum Namensraum-Element hinzugefügt werden, um einen
Unterpfad im Stammverzeichnis der Erweiterung anzugeben.

### SQL

```sql
    <install>
        <sql>
            <file driver="mysql" charset="utf8">sql/example.install.sql</file>
        </sql>
    </install>
    <uninstall>
        <sql>
            <file driver="mysql" charset="utf8">sql/example.uninstall.sql</file>
        </sql>
    </uninstall>
```

Im obigen Beispiel haben wir die SQL-Dateien im Ordner `admin/sql` des
Installationspakets abgelegt. Sie müssen den `sql`-Ordner in die
Administration-Dateien aufnehmen (wie unter *Back-End Dateien*
beschrieben).

Sie können SQL während der Installation und/oder Deinstallation mit den
Elementen und ausführen. Ein -Element sollte als untergeordnetes Element
dieser Elemente angezeigt werden. kann eine beliebige Anzahl von
-Elementen enthalten, die jeweils eine einzelne auszuführende SQL-Datei
definieren. Ihre Datenbank-Treibertypen werden durch das Attribut
`driver`, ihre Zeichensätze durch das Attribut `charset` beschrieben.

#### Update des SQL-Schemas

Seit 1.6 gibt es auch ein -Tag, mit dem Sie eine Reihe von SQL-Dateien
zum Aktualisieren des aktuellen Schemas bereitstellen können.

```sql
	<update>
		<schemas>
			<schemapath type="mysql">sql/updates/mysql</schemapath>
			<schemapath type="sqlsrv">sql/updates/sqlsrv</schemapath>
		</schemas>
	</update>
```

Um beispielsweise bei einer **MySQL**-Datenbank von Version `1.0.0` auf
Version `1.0.1` zu wechseln, muss eine `1.0.1.sql`-Datei im Ordner
`sql/updates/mysql` erstellt und das -Tag des Manifests aktualisiert
werden auf

```sql
<version>1.0.1</version>
```

Die grundlegende Struktur des SQL-Ordners sieht folgendermaßen aus
(angenommen, dass eine **MySQL-Datenbank** genutzt wird)

    sql
     |-->example.install.sql
     |-->example.uninstall.sql
     |-->updates
         |-->mysql
            |-->1.0.1.sql

Ähnliche Dateien müssen für andere Versionen erstellt werden.

### Sprach-Dateien

Seit Joomla! 1.5 mussten Entwickler von Erweiterungen die Sprach-Dateien
im Joomla! Haupt-Sprachen-Ordner mit dem Tag ... ablegen (siehe unten).
**Diese Methode kann immer noch in allen Joomla!** Versionen verwendet
werden**.**

```xml
<!-- Joomla! language tag -->
<languages folder="langfiles">
	<language tag="en-GB">en-GB.com_example.ini</language>
</languages>
```

Dennoch wird seit Joomla! 1.6 empfohlen, dass die Sprachdateien der
Erweiterung in dessen Erweiterungsordner abgelegt werden sollen. Joomla!
wird dann automatisch die Sprachdateien der Erweiterung laden.

Durch das Speichern der Sprach-Dateien der Erweiterung im
Erweiterungsordner profitiert man von der Isolierung und dem Schutz der
Sprach-Dateien der Erweiterung. Ein Administrator entfernt zum Beispiel
eine Sprache aus seiner Joomla!-Installation. Die Sprach-Dateien der
Erweiterung werden dabei nicht entfernt. Sie bleiben erhalten und sind
verfügbar, falls die Sprache erneut installiert wird.

Die Struktur des Sprachordners für Frontend und Backend ist gleich. Man
legt diese mit dem Sprach-Tag (z.B. **en-GB**) der jeweiligen Sprache im
Sprachenverzeichnis ab, d.h. **language/en-GB/**. Diese Verzeichnisse
müssen auch in den Frontend- und Backend-Dateien festgelegt werden.

In der Manifest-Datei muss lediglich das Verzeichnis **language** in den
Abschnitt **files** eingefügt werden. Die Unterverzeichnisse für jede
Sprache werden automatisch kopiert. Innerhalb der Gruppe muss das
Element neben den Elementen in der Gruppe hinzugefügt werden, siehe
dieses Beispiel:

```xml
<files>
	<filename plugin="alpha">alpha.php</filename>
	<folder>sql</folder>
	<folder>language</folder>
</files>
```

**Hinweis:** Beide Wege können nebeneinander funktionieren. Hier ist ein
Beispiel aus dem Kern:

```xml
<files>
	<filename plugin="languagecode">languagecode.php</filename>
	<filename>index.html</filename>
	<folder>language</folder>
</files>
<languages>
	<language tag="en-GB">language/en-GB/en-GB.plg_system_languagecode.ini</language>
	<language tag="en-GB">language/en-GB/en-GB.plg_system_languagecode.sys.ini</language>
</languages>
```

Folgende Vorteile ergeben sich aus dieser Lösung:

Alle im Kernverzeichnis vorhandenen *.ini*-Dateien haben Vorrang vor den
Dateien im Erweiterungsordner *language/*. Beispielsweise wird eine
*.sys.ini*-Datei immer aus den Core-Ordnern im Back-End geladen, wenn
sie vorhanden ist, außer bei der Installation einer Erweiterung, die
eine *.sys.ini*-Datei in einem Sprachordner enthält. In diesem Fall und
nur in diesem Fall zeigt die *.sys.ini*-Datei im Erweiterungsordner den
übersetzten Inhalt zum Zeitpunkt der Installation an. Das ist sehr
praktisch. Denn ein Entwickler kann zwei *.sys.ini*-Dateien mit
unterschiedlichen Inhalten verwenden.

Ferner ist es für einen Benutzer, der eine *.ini*-Datei für eine
Erweiterung benötigt, die nicht in der gewünschten Sprache angeboten
wird, viel einfacher, sie in den Hauptverzeichnissen hinzuzufügen. Es
besteht keine Gefahr, dass sie bei einer versehentlichen Deinstallation
der Erweiterung oder aus einem anderen Grund gelöscht wird.

Siehe auch:

- [Making non-core language
  packs](https://docs.joomla.org/J2.5:Making_non-core_language_packs "Special:MyLanguage/J2.5:Making non-core language packs")
- [Creating language packs for extensions in Joomla
  2.5](https://docs.joomla.org/Creating_language_packs_for_extensions_in_Joomla_2.5 "Special:MyLanguage/Creating language packs for extensions in Joomla 2.5")

Während der Entwicklung kann in der „globalen Konfiguration“ von Joomla!
das Sprach-Debugging eingeschaltet werden. So kann man nachforschen,
falls ein Problem auftritt. Ab 3.2 ist dies notwendig, um das Debuggen
zu unterstützen, da bei inaktivem Debug-Modus „en-GB“ **immer** zuerst
geladen wird, um die Anzeige von Konstanten zu verhindern.

### Script Datei

```xml
    <scriptfile>example.script.php</scriptfile>
```

Ein optionale **Script-Datei** (PHP-Code, der vor, während und/oder nach
der Installation, Deinstallation und Aktualisierung ausgeführt wird)
kann mit Hilfe eines -Elements definiert werden.

Diese Datei sollte eine Klasse mit dem Namen "InstallerScript"
enthalten, wobei der Name der Erweiterung ist (z.B. com_componentname,
mod_modulename usw.). Plugins müssen die Gruppe angeben (z.B.
plgsystempluginname).

In <img src="https://docs.joomla.org/images/f/f4/Compat_icon_4_0.png"
decoding="async" data-file-width="40" data-file-height="17" width="40"
height="17" alt="Joomla 4.0" /> und neuer ist die Klassenstruktur wie
folgt:

```php
<?php

use Joomla\CMS\Installer\InstallerAdapter;

class com_componentnameInstallerScript
{
	/**
	 * Constructor
	 *
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 */
	public function __construct(InstallerAdapter $adapter)
	{
	}

	/**
	 * Called before any type of action
	 *
	 * @param   string  $route  Which action is happening (install|uninstall|discover_install|update)
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 *
	 * @return  boolean  True on success
	 */
	public function preflight($route, InstallerAdapter $adapter)
	{
		return true;
	}

	/**
	 * Called after any type of action
	 *
	 * @param   string  $route  Which action is happening (install|uninstall|discover_install|update)
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 *
	 * @return  boolean  True on success
	 */
	public function postflight($route, $adapter)
	{
		return true;
	}

	/**
	 * Called on installation
	 *
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 *
	 * @return  boolean  True on success
	 */
	public function install(InstallerAdapter $adapter)
	{
		return true;
	}

	/**
	 * Called on update
	 *
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 *
	 * @return  boolean  True on success
	 */
	public function update(InstallerAdapter $adapter)
	{
		return true;
	}

	/**
	 * Called on uninstallation
	 *
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 */
	public function uninstall(InstallerAdapter $adapter)
	{
		return true;
	}
}

?>
```

Seit Joomla 3.6 gibt es ein einfaches Skript, das man verwenden kann,
anstatt ein eigenes Skript zu entwickeln: **JInstallerScript** enthält
verschiedene hilfreiche Methoden, die in der Community häufig verwendet
werden.

### Bibliothek-Manifestdatei

Eine einfache Manifestdatei einer Bibliothek könnte wie folgt aussehen:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="library" method="upgrade" version="4.0">
    <name>My Test library.</name>
    <libraryname>mytest</libraryname>
    <files>
        <folder>Classes</folder>
        <folder>language</folder>
        <filename>mytest.php</filename>
    </files>
</extension>
```

Damit wird die Bibliothek in den Ordner `JPATH_SITE/libraries/mytest`
installiert.

Was ist, wenn in der Firma mehrere Bibliotheken existieren und diese
unter dem Ordner `JPATH_SITE/libraries/mycompany` zusammengefasst werden
sollen?

Ganz einfach: Der Name des Unternehmens wird in die
`libraryname`-Eigenschaft der einzelnen Bibliotheken eingefügt, etwa so:

```xml
    <libraryname>mycompany/mylibrary1</libraryname>
```

```xml
    <libraryname>mycompany/mylibrary2</libraryname>
```

Diese Bibliotheken werden dann in den Ordnern
`JPATH_SITE/libraries/mycompany/mylibrary1` und
`JPATH_SITE/libraries/mycompany/mylibrary2` installiert.

Wenn `mylibrary1` deinstalliert wird, bleibt `mylibrary2` weiterhin auf
der Website installiert.

Wenn `script files` mit solchen Firmenbibliotheken verwendet wird,
sollte der Klassenname des Installationsprogramms wie folgt aussehen:

```php
    class mycompanymylibrary1InstallerScript
```

```php
    class mycompanymylibrary2InstallerScript
```

### Update Server

```xml
    <updateservers>
        <server type="extension" priority="1" name="Extension Update Site">http://example.com/extension.xml</server>
        <server type="collection" priority="2" name="Collection Update Site">http://example.com/collection.xml</server>
    </updateservers>
```

Update-Server können im Element definiert werden, einem untergeordneten
Element von Root. Dieses Element kann ein oder mehrere -Elemente
enthalten, die jeweils den Standort angeben, von dem Updates
heruntergeladen werden sollen. Jeder Eintrag kann folgende Attribute
definieren:

<table class="wikitable">

<tbody>
<tr class="header">
<th>Attribut</th>
<th>Werte</th>
<th>Beschreibung</th>
</tr>
&#10;<tr class="odd">
<td>type</td>
<td><code>extension</code><br />
<code>collection</code></td>
<td>Der Typ des Update-Servers</td>
</tr>
<tr class="even">
<td>priority</td>
<td><em>integer</em></td>
<td>Die Priorität des Update-Servers</td>
</tr>
<tr class="odd">
<td>name</td>
<td><em>string</em></td>
<td>Der Name des Update-Servers</td>
</tr>
</tbody>
</table>

weitere Informationen:

- [Entwickeln einer MVC-Komponente/Hinzufügen eines
  Update-Servers](https://docs.joomla.org/J3.x:Developing_an_MVC_Component/Adding_an_update_server "Special:MyLanguage/J3.x:Developing an MVC Component/Adding an update server")
- [Managing Component Updates in Joomla
  2.5](https://docs.joomla.org/J2.5:Managing_Component_Updates "Special:MyLanguage/J2.5:Managing Component Updates")

### Download Keys Unterstützung

Ab Joomla 4.0.0 können Benutzer ihre Download-Schlüssel in der Liste der
Update-Sites eingeben. Dadurch haben sie einen zentralen Ort, um die
Download-Schlüssel zu verwalten. Wenn ein Benutzer eine Erweiterung
aktualisieren will, prüft Joomla, ob es einen Download-Schlüssel gibt.
Wenn es einen Download-Schlüssel gibt, fügt Joomla den
Download-Schlüssel zur Update-URL hinzu.

Um die Download-Schlüssel zu unterstützen, muss das Tag `dlid` in die
Manifest-Datei aufgenommen werden. Das Tag `dlid` akzeptiert 2
Argumente:

- prefix
- suffix

Das Tag `dlid` wird in der Manifest-Datei wie hier dargestellt aussehen:

```xml
<dlid prefix="dlid=" suffix="&amp;dummy=my.zip"/>
```

Das Präfix wird vor dem Download-Schlüssel und das Suffix nach dem
Download-Schlüssel eingefügt. Im Beispiel oben lautet die vollständige
Abfrage, die dem Download-Link hinzugefügt wird, wie folgt:

    dlid=KEY&dummy=my.zip

Der Schlüssel wird hinzugefügt, bevor das Event
`onInstallerBeforePackageDownload` getriggert wird, sodass die
vollständige URL dem Event übergeben wird.

### Beispiele

Ein reales Beispiel findet man unter: <a
href="https://github.com/joomla/joomla-cms/blob/3.9.16/administrator/components/com_banners/banners.xml"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">the manifest of the Banner component
in the latest version of Joomla! 3.9.16</a>.

Einige weitere Beispiele finden Sie im eigenständigen
Weblink-Repository:

- <a
  href="https://github.com/joomla-extensions/weblinks/blob/master/src/administrator/components/com_weblinks/weblinks.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">com_weblinks manifest</a>
- <a
  href="https://github.com/joomla-extensions/weblinks/blob/master/src/modules/mod_weblinks/mod_weblinks.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">mod_weblinks manifest</a>
- <a
  href="https://github.com/joomla-extensions/weblinks/blob/master/src/plugins/search/weblinks/weblinks.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">plg_search_weblinks manifest</a>
- <a
  href="https://github.com/joomla/joomla-cms/blob/3.9.16/templates/protostar/templateDetails.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">tpl_protostar manifest (3.9.16)</a>
- <a
  href="https://github.com/joomla/joomla-cms/blob/3.9.16/administrator/language/en-GB/en-GB.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">en-GB manifest (3.9.16)</a>

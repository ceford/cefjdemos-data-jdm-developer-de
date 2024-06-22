<!-- Filename: J4.x:Creating_a_Simple_Module / Display title: Ein einfaches Modul erstellen -->

Joomla!  4.x \>Tutorial Ein einfaches Modul für Joomla! 4.x erstellen

Dieses Tutorial zeigt, wie man ein einfaches Modul für Joomla Version
4.x erstellt.

## Einleitung

Joomla! 4 unterscheidet fünf Arten von Erweiterungen:

- **[Komponenten](https://docs.joomla.org/Component "Special:MyLanguage/Component")**
  Webseiten bestehen hauptsächlich aus Komponenten. Eine Komponente
  steuert die Datenverarbeitung wie auch die Eingabe und Speicherung in
  der Datenbank. Eine Komponente steht auf den meisten Webseiten im
  Mittelpunkt der jeweiligen Seite.
- **[Module](https://docs.joomla.org/Module "Special:MyLanguage/Module")**
  Ein Modul ist ein Add-on für die Webseite und erweitert die
  Funktionalität. Ein Modul ist für eine Webseite in der Regel
  zweitrangig und nicht dafür gedacht, im Mittelpunkt der jeweiligen
  Seite zu stehen. Es kann in verschiedenen Positionen angezeigt werden,
  wobei bestimmt werden kann, unter welchen aktiven Menüpunkten es
  angezeigt werden soll. Module sind einfach gestrickte und flexible
  Erweiterungen. Sie werden für einzelne Bereiche der Webseite
  verwendet, die in der Regel weniger komplex sind, und können über
  verschiedene Komponenten hinweg sichtbar sein.
- **[Plugins](https://docs.joomla.org/Plugin "Special:MyLanguage/Plugin")**
  Ein Plugin verändert den Output, der vom System bereits generiert
  wurde. Es bildet typischerweise keinen eigenständigen Teil der
  Webseite. Es übernimmt Daten aus anderen Quellen (z.B. dem Inhalt) und
  verändert diese Daten vor der Anzeige. Ein Plugin arbeitet
  normalerweise hinter den Kulissen.
- **[Sprachen](https://docs.joomla.org/Languages "Special:MyLanguage/Languages")**
  Die wahrscheinlich einfachsten Erweiterungen sind Sprachen. Im
  Wesentlichen bestehen die Sprachpaketdateien aus
  Schlüssel-/Wert-Paaren, die die Übersetzung statischer
  Textzeichenfolgen innerhalb des Joomla! Quellcodes ermöglichen.
- **[Templates](https://docs.joomla.org/Templates "Special:MyLanguage/Templates")**
  Ein Template ist im Grunde genommen das Design einer Joomla!-Webseite.

Joomla! 4 wurde so konstruiert, dass es fünf verschiedene Anwendungen
nutzt:

- Installation (wird zur Installation von Joomla genutzt und muss nach
  der Installation gelöscht werden)
- Administrator (Backend - wird zum Verwalten des Inhalts genutzt)
- Öffentlich oder Webseite (Frontend - wird zum Anzeigen des Inhalts
  genutzt)
- CLI (wird für den Zugriff auf Joomla über die Kommandozeile und für
  Cron-Jobs genutzt)
- API (Webservices - wird zur Erstellung von APIs für
  maschinenzugängliche Inhalte genutzt)

Die Installationsanwendung wird einmal genutzt. Administrator und
Öffentlich werden über das Konzept der *Komponenten* mit *Modulen*
genutzt. Jedes Modul hat einen einzigen Einstiegspunkt, der sich im
*modules* bzw. *administrator/modules* Verzeichnis befindet. Dieser
Einstiegspunkt heißt *`mod_modulename/mod_modulename.php`* (das Präfix
*mod\_* ist historisch bedingt). Für das Login Modul ist der
Einstiegspunkt beispielsweise */mod_login/mod_login.php*.

### Voraussetzungen

Für dieses Tutorial wird eine Joomla! 4.x-Installation benötigt
(<a href="https://github.com/joomla/joomla-cms/releases"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">Download der aktuellsten Version</a>).

Joomla! 4 kann unter
<a href="https://github.com/joomla/joomla-cms/releases"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">GitHub</a>, auf der
<a href="https://developer.joomla.org/nightly-builds.html"
class="external text" target="_blank"
rel="noreferrer noopener">Developer Website</a> heruntergeladen werden.
Alternativ kann eine kostenlose Website unter
<a href="https://launch.joomla.org" class="external free"
target="_blank" rel="noreferrer noopener">https://launch.joomla.org</a>
erstellt werden.

## Ein einfaches Modul erstellen/Ein Basismodul entwickeln - Teil 1

Viele Beispiele für Module finden sich in der Standard Joomla!
Installation. Zum Beispiel:

- Menüs
- Neuste Beiträge
- Anmeldeformular
- und viele weitere.

Dieses Tutorial erklärt, wie ein einfaches Modul erstellt wird. Es macht
außerdem mit der grundlegenden Dateistruktur eines Joomla! 4 Moduls
vertraut. Die Grundstruktur kann anschließend zu komplexeren Modulen
erweitert werden.

### Dateistruktur

Es gibt einige grundlegende Dateien, die im Standardschema der
Modulentwicklung genutzt werden:

- `mod_foo.php` - Diese Datei ist der Haupt-Startpunkt für das Modul.
  Sie führt alle notwendigen Initialisierungsroutinen und Hilferoutinen
  aus und sammelt alle notwendigen Daten. Sie bindet das Template für
  das Ausgabe-Modul ein.

`mod_foo.xml` - Diese Datei enthält Informationen über das Modul. Sie
definiert die Dateien, welche bei der Installation mit dem Joomla!
Installer notwendig sind und bestimmt Konfigurationsparameter für das
Modul.

- `tmpl/default.php` - Das ist das Modultemplate. Die Datei nutzt die
  Daten, welche durch mod_helloworld.php gesammelt wurden und generiert
  das HTML, welches dann auf der Seite angezeigt wird.
- `language/en-GB/mod_foo.ini` und `language/en-GB/mod_foo.sys.ini`- Das
  sind die Dateien, die den Text in englischer Sprache (Variante:
  Vereinigtes Königreich) enthalten.

### mod_foo.php erstellen

Die `mod_foo.php` Datei führt folgende Aufgaben durch:

- Import der Klasse `ModuleHelper` in den aktuellen Bereich. Sie wird
  später für die Anzeige des Outputs gebraucht.
- Hinzufügen der Vorlage, die den Output anzeigen soll

Die Hilfsklasse wird am Anfang der Datei in den aktuellen Bereich
importiert.

```php
    use Joomla\CMS\Helper\ModuleHelper;
```

Zuletzt wird die Vorlage eingefügt, welche den Output anzeigt via

```php
    require ModuleHelper::getLayoutPath('mod_foo', $params->get('layout', 'default'));
```

#### Vollständige mod_foo.php Datei

Die vollständige `mod_foo.php` Datei sieht so aus:

```php
<?php
/**
 * @package    [PACKAGE_NAME]
 *
 * @author     [AUTHOR] <[AUTHOR_EMAIL]>
 * @copyright  [COPYRIGHT]
 * @license    GNU General Public License version 2 or later; see LICENSE.txt
 * @link       [AUTHOR_URL]
 */

// No direct access to this file
defined('_JEXEC') or die;

use Joomla\CMS\Helper\ModuleHelper;

require ModuleHelper::getLayoutPath('mod_foo', $params->get('layout', 'default'));
```

*Anmerkung*ː In Joomla 3x wurde normalerweise eine Zeile wie
`$moduleclass_sfx = htmlspecialchars($params->get('moduleclass_sfx'));`
verwendet. Das ist nicht mehr notwendig. Siehe auch diesen Pull-Request:
<a href="https://github.com/joomla/joomla-cms/pull/17447"
class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla/joomla-cms/pull/17447</a>.

Die einzige Zeile, die wir bisher nicht erklärt haben, ist die erste
Zeile `defined('_JEXEC') or die;`. Diese Zeile überprüft
sicherheitshalber, dass die Datei von der Joomla! Anwendung eingebunden
wird. Dies ist notwendig, um unterschiedlichste Injektionen und andere
potenzielle Sicherheitsverletzungen zu verhindern.

### tmpl/default.php erstellen

Die Datei `default.php` ist das Template welches die Modulausgabe
anzeigt.

#### Vollständige tmpl/default.php Datei

Der Code für die tmpl/default.php Datei lautet wie folgt:

```php
<?php
/**
 * @package    [PACKAGE_NAME]
 *
 * @author     [AUTHOR] <[AUTHOR_EMAIL]>
 * @copyright  [COPYRIGHT]
 * @license    GNU General Public License version 2 or later; see LICENSE.txt
 * @link       [AUTHOR_URL]
 */

// No direct access to this file
defined('_JEXEC') or die;

echo '[PROJECT_NAME]';
```

Ein wichtiger Punkt ist, dass die Template-Datei den gleichen
Gültigkeits-Bereich wie die `mod_foo.php`-Datei hat. Das bedeutet, dass
eine Variable, die in der Datei `mod_foo.php` definiert ist dann ohne
zusätzliche Deklarationen oder Funktionsaufrufe in der Template-Datei
verwendet werden kann.

### mod_foo.xml erstellen

Die `mod_foo.xml` Datei ist die Installationsdatei. Die meisten Einträge
sind selbsterklärend.

#### Vollständige mod_foo.xml Datei

Der Code für die `mod_foo.xml` Datei lautet wie folgt:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="module" version="4.0" client="site" method="upgrade">
    <name>MOD_FOO</name>
    <creationDate>[DATE]</creationDate>
    <author>[AUTHOR]</author>
    <authorEmail>[AUTHOR_EMAIL]</authorEmail>
    <authorUrl>[AUTHOR_URL]</authorUrl>
    <copyright>[COPYRIGHT]</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <version>1.0</version>
    <description>MOD_FOO_XML_DESCRIPTION</description>
    <files>
        <filename module="mod_foo">mod_foo.php</filename>
        <folder>tmpl</folder>
        <folder>language</folder>
        <filename>mod_foo.xml</filename>
    </files>
</extension>
```

### Sprachdateien erstellen

Die Dateien `language/en-GB/mod̠foo.ini` und
`language/en-GB/mod̠foo.sys.ini` werden gebraucht, um Text im Frontend
und Backend zu übersetzen. Hinweis: Die Struktur der Sprachdateien wurde
in Joomla! 4 aktualisiert, und Sprachpräfixe auf den einzelnen Dateien
in einem Sprachverzeichnis sind nicht mehr erforderlich.

Der Code für `language/en-GB/mod̠foo.sys.ini` lautet wie folgt:

```ini
    MOD_FOO="[PROJECT_NAME]"
    MOD_FOO_XML_DESCRIPTION="Foo Module"
```

Der Code für `language/en-GB/mod̠_foo.ini` lautet wie folgt:

```ini
    MOD_FOO="[PROJECT_NAME]"
    MOD_FOO_XML_DESCRIPTION="Foo Module"
```

Die Datei `.sys.ini` wird verwendet, um die Beschreibung der Erweiterung
bei der Installation zu übersetzen, wohingegen die Datei `.ini` die
restlichen Zeichenketten und die Beschreibung zur Anzeige der
Erweiterung übersetzt.

Weitere Informationen zu Sprachdateien kann man
<a href="https://docs.joomla.org/Specification_of_language_files"
class="new"
title="Special:MyLanguage/Specification of language files (page does not exist)">hier</a>
finden.

## Modul testen

Jetzt können alle Dateien gezippt und über den Joomla Extension Manager
installiert werden.
Anschließend kann das Modul im Modul-Manager ausgewählt werden, wenn ein
neues Webseitenmodul angelegt wird.

<img
src="https://docs.joomla.org/images/thumb/4/40/Moduletutorial1-de.png/700px-Moduletutorial1-de.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/4/40/Moduletutorial1-de.png/1050px-Moduletutorial1-de.png 1.5x, https://docs.joomla.org/images/thumb/4/40/Moduletutorial1-de.png/1400px-Moduletutorial1-de.png 2x"
data-file-width="1656" data-file-height="807" width="700" height="341"
alt="Moduletutorial1-de.png" /> <img
src="https://docs.joomla.org/images/thumb/d/d1/Moduletutorial2-de.png/700px-Moduletutorial2-de.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/d/d1/Moduletutorial2-de.png/1050px-Moduletutorial2-de.png 1.5x, https://docs.joomla.org/images/d/d1/Moduletutorial2-de.png 2x"
data-file-width="1400" data-file-height="682" width="700" height="341"
alt="Moduletutorial2-de.png" />

Im Frontend erscheint das Modul, wie es im nächsten Bild dargestellt
wird.

<img
src="https://docs.joomla.org/images/thumb/8/88/Moduletutorial3-de.png/700px-Moduletutorial3-de.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/8/88/Moduletutorial3-de.png/1050px-Moduletutorial3-de.png 1.5x, https://docs.joomla.org/images/8/88/Moduletutorial3-de.png 2x"
data-file-width="1400" data-file-height="682" width="700" height="341"
alt="Moduletutorial3-de.png" />

## Fazit

Die Modulentwicklung für Joomla gestaltet sich als ziemlich einfach und
unkompliziert. Mit den Techniken, die in diesem Tutorial beschrieben
werden, lassen sich mit geringem Aufwand unterschiedlichste Module
entwickeln.

Vorlagen finden sich hier:

- <a
  href="https://github.com/joomla-extensions/boilerplate/tree/master/module"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/joomla-extensions/boilerplate/tree/master/module</a>

Die Beispieldateien für dieses Tutorial finden sich hier:

- <a
  href="https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules</a>

## Eine Hilfsklasse mittels Namespaces hinzufügen - Teil 2

### Voraussetzungen

Für dieses Tutorial wird Joomla! 4.x benötigt (zum Zeitpunkt des
Verfassens aktuell Joomla! 4.0.0-alpha6-dev)

## Namespaces

Mit PHP 5.3 wurden Namespaces eingeführt. Sie sind in anderen
Programmiersprachen schon seit langem im Einsatz. Nun können diese
kleinen Strukturen auch hier verwendet werden, um Übersichtlichkeit in
den Code zu bringen.

Namespaces sind getrennte Bereiche, in denen bestimmte logische Elemente
(in unserem Fall Klassen, Schnittstellen, Funktionen und Konstanten)
existieren können. Diese Bereiche bieten eine Verkapselung des Codes und
verhindern Namenskonflikte.

Nun soll es darum gehen, wie sie in Joomla 4 verwendet werden. Hierbei
kommt die helper-Datei zum Einsatz.

### Dateistruktur

Die folgenden Dateien werden erstellt/geändert:

- `Helper/FooHelper.php` - Das ist die Datei, die wir hier als Beispiel
  verwenden. Wir müssen sie erstellen.
- `mod_foo.php` - In dieser Datei laden wir hauptsächlich den Namespace.
- `tmpl/default.php` - In dieser Datei zeigen wir, wie die Daten eines
  Helpers angezeigt werden können.
- `mod_foo.xml` - Wir erstellen ein neues Verzeichnis mit einer neuen
  Datei, die während der Installation installiert werden muss. Dafür
  müssen wir sie in dieser Datei angeben.

### Helper/FooHelper.php erstellen

Unsere neue Helferklasse sollte zu einem Namespace gehören. Wir
erreichen das mit dem folgenden Code. Es ist wichtig, dass sich diese
Zeile am Anfang der Datei befindet.

```php
    namespace Joomla\Module\Foo\Site\Helper;
```

Dann erstellen wir eine einfache Klasse mit einer einfachen Methode.
Natürlich kann man hier noch viel mehr tun. Werfen Sie einen Blick auf
die Joomla-Core-Methoden. Hier gibt es viele Beispiele. Sie zeigen aus
erster Hand, welche Möglichkeiten bestehen.

#### Vollständige Helper/FooHelper.php

Die vollständige `FooHelper.php` Datei sieht so aus:
```php
<?php
/**
 * @package     Joomla.Site
 * @subpackage  mod_foo
 *
 * @copyright   Copyright (C) 2005 - 2019 Open Source Matters, Inc. All rights reserved.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace Joomla\Module\Foo\Site\Helper;

// No direct access to this file
defined('_JEXEC') or die;

/**
 * Helper for mod_foo
 *
 * @since  4.0
 */
class FooHelper
{
	/**
	 * Retrieve foo test
	 *
	 * @param   Registry        $params  The module parameters
	 * @param   CMSApplication  $app     The application
	 *
	 * @return  array
	 */
	public static function getText()
	{
		return 'FooHelpertest';
	}
}
```

### Editing mod_foo.php

Our new helper class is imported to our current scope at the beginning of the file.

```php
    use Joomla\Module\Foo\Site\Helper\FooHelper;
```

Am Schluss wird die Hilfsdatei genutzt, um zu testen, ob korrekt geladen
wird:

```php
    $test  = FooHelper::getText($params, $app);
```

#### Vollständige mod_foo.php Datei

Die vollständige `mod_foo.php` Datei sieht so aus:

```php
<?php
/**
 * @package    [PACKAGE_NAME]
 *
 * @author     [AUTHOR] <[AUTHOR_EMAIL]>
 * @copyright  [COPYRIGHT]
 * @license    GNU General Public License version 2 or later; see LICENSE.txt
 * @link       [AUTHOR_URL]
 */

// No direct access to this file
defined('_JEXEC') or die;

use Joomla\CMS\Helper\ModuleHelper;
use Joomla\Module\Foo\Site\Helper\FooHelper;

$test  = FooHelper::getText();

require ModuleHelper::getLayoutPath('mod_foo', $params->get('layout', 'default'));
```

### tmpl/default.php bearbeiten

In dieser Datei muss nur eine kleine Änderung vorgenommen werden, um im
Frontend zu demonstrieren, dass die Hilfedatei ordnungsgemäß
funktioniert. Den Wert der Variablen am Ende der aktuellen Ausgabe
einfügen.

```php
    echo '[PROJECT_NAME]' . $test;
```

#### Vollständige tmpl/default.php Datei

Die vollständige `tmpl/default.php` Datei sieht so aus:

```php
<?php
/**
 * @package    [PACKAGE_NAME]
 *
 * @author     [AUTHOR] <[AUTHOR_EMAIL]>
 * @copyright  [COPYRIGHT]
 * @license    GNU General Public License version 2 or later; see LICENSE.txt
 * @link       [AUTHOR_URL]
 */

// No direct access to this file
defined('_JEXEC') or die;

echo '[PROJECT_NAME]' . $test;
```

### mod_foo.xml bearbeiten

Zuerst muss die folgende Zeile eingefügt werden, damit der Namenspace in
Joomla automatisch gesetzt wird. Nach dieser Änderung müssen das Modul
neu installiert werden. Ein einfacher Austausch eines bereits
installierten Moduls reicht nicht aus. Wenn eine lokale Entwicklung
durchgeführt wird, können auch die Dateien library/autoload_psr4.php
gelöscht werden. Diese werden dann automatisch neu erstellt. Nach dem
Einfügen und Installieren ist der Namespace dem Loader
`JPATH_LIBRARIES . '/autoload_psr4.php'` bekannt.

```xml
<namespace>Joomla\Module\Foo</namespace>
```

Joomla! soll die Hilfedatei bei der Installation des Moduls kopieren.
Also muss Joomla! die Hilfedatei kennen. Dazu mussen der XML-Datei die
folgende Zeile hinzufügen werden.

```xml
<folder>Helper</folder>
```

#### Vollständige mod_foo.xml

Die vollständige `mod_foo.xml` Datei sieht so aus:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="module" version="4.0" client="site" method="upgrade">
    <name>MOD_FOO</name>
    <creationDate>[DATE]</creationDate>
    <author>[AUTHOR]</author>
    <authorEmail>[AUTHOR_EMAIL]</authorEmail>
    <authorUrl>[AUTHOR_URL]</authorUrl>
    <copyright>[COPYRIGHT]</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <version>1.0</version>
    <description>MOD_FOO_XML_DESCRIPTION</description>
    <namespace>Joomla\Module\Foo</namespace>
    <files>
        <filename module="mod_foo">mod_foo.php</filename>
        <folder>tmpl</folder>
        <folder>Helper</folder>
        <folder>language</folder>
        <filename>mod_foo.xml</filename>
    </files>
</extension>
```

## Modul testen

Jetzt können alle Dateien gezippt und über den Joomla Extension Manager
installiert werden. Anschließend kann das Modul im Modul-Manager
ausgewählt werden, wenn ein neues Webseitenmodul angelegt wird.

Im nächsten Bild ist das Modul zu sehen wie es im Frontend dargestellt
wird. Der Text der Hilfedatei wird angezeigt.

<img
src="https://docs.joomla.org/images/thumb/3/3b/Moduletutorial23-de.png/700px-Moduletutorial23-de.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/3/3b/Moduletutorial23-de.png/1050px-Moduletutorial23-de.png 1.5x, https://docs.joomla.org/images/3/3b/Moduletutorial23-de.png 2x"
data-file-width="1400" data-file-height="682" width="700" height="341"
alt="Moduletutorial23-de.png" />

## Fazit

Wie man sehen kann, sind die Namespaces nicht mehr so ​​kompliziert, wenn
man sich erst einmal damit befasst hat. In großen Projekten kann das
viele Vorteile bringen. Man sollte jedoch Namespaces vorab planen, da es
sonst schnell zeitaufwändiger wird.

Vorlagen finden sich hier:

- <a
  href="https://github.com/joomla-extensions/boilerplate/tree/master/module"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/joomla-extensions/boilerplate/tree/master/module</a>

Die Beispieldateien für dieses Tutorial finden sich hier:

- <a
  href="https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules</a>

## Anpassung: Parameter über Formularfelder hinzufügen - Teil 3

### Voraussetzungen

Für dieses Tutorial wird Joomla! 4.x benötigt (zum Zeitpunkt des
Verfassens aktuell Joomla! 4.0.0-alpha6-dev)

## Anpassung mit Formularfeldern und Parametern

Formularfelder sind in Joomla sehr anpassbar und für Module die einzige
Möglichkeit, das Modul an die Anforderungen der Site anzupassen.

In diesem Fall erweitern wir unser vorheriges Beispiel um ein URL-Feld
zum Einfügen einer Domain, die wir als Link im Frontend verwenden
können. Um dies zu ermöglichen, verwenden wir den Typ [URL Form
Field](https://docs.joomla.org/URL_form_field_type "Special:MyLanguage/URL form field type").

Danach fügt man Parameter ein, die es erlauben, die
Standardfunktionalität von Joomla zu nutzen.

Werfen wir einen Blick auf die Verwendung in Joomla 4.

### Dateistruktur

Die folgenden Dateien werden geändert:

- `mod_foo.xml` - In dieser Datei werden die Felder hinzugefügt.
- `tmpl/default.php` - In dieser Datei wird gezeigt, wie die Daten eines
  Feldes verwendet werden können.
- `language/en-GB/en-GB.mod_foo.ini` - Hier verwenden wir
  Sprach-Strings, womit das Feld in verschiedenen Sprachen korrekt
  beschriftet werden kann.

### mod_foo.xml bearbeiten

Zunächst wird ein benutzerdefinierter Parameter gesetzt.

```xml
<field
    name="domain"
    type="url"
    label="MOD_FOO_FIELD_URL_LABEL"
    filter="url"
/>
```

Dann werden Joomla-Standardfelder eingefügt, die Cache,
Modulklassensuffix und Layouts ermöglichen.

```xml
<field
    name="layout"
    type="modulelayout"
    label="JFIELD_ALT_LAYOUT_LABEL"
    class="custom-select"
/>

<field
    name="moduleclass_sfx"
    type="textarea"
    label="COM_MODULES_FIELD_MODULECLASS_SFX_LABEL"
    rows="3"
/>

<field
    name="cache"
    type="list"
    label="COM_MODULES_FIELD_CACHING_LABEL"
    default="0"
>
    <option value="1">JGLOBAL_USE_GLOBAL</option>
    <option value="0">COM_MODULES_FIELD_VALUE_NOCACHING</option>
</field>

<field
    name="cache_time"
    type="number"
    label="COM_MODULES_FIELD_CACHE_TIME_LABEL"
    default="0"
/>

<field
    name="cachemode"
    type="hidden"
    default="itemid"
>
    <option value="itemid"></option>
</field>
```

#### Vollständige mod_foo.xml

Die vollständige `mod_foo.xml` Datei sieht so aus:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="module" version="4.0" client="site" method="upgrade">
    <name>MOD_FOO</name>
    <creationDate>[DATE]</creationDate>
    <author>[AUTHOR]</author>
    <authorEmail>[AUTHOR_EMAIL]</authorEmail>
    <authorUrl>[AUTHOR_URL]</authorUrl>
    <copyright>[COPYRIGHT]</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <version>1.0</version>
    <description>MOD_FOO_XML_DESCRIPTION</description>
    <namespace>Joomla\Module\Foo</namespace>
    <files>
        <filename module="mod_foo">mod_foo.php</filename>
        <folder>tmpl</folder>
        <folder>Helper</folder>
        <folder>language</folder>
        <filename>mod_foo.xml</filename>
    </files>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="domain"
                    type="url"
                    label="MOD_FOO_FIELD_URL_LABEL"
                    filter="url"
                />
            </fieldset>
            <fieldset name="advanced">
                <field
                    name="layout"
                    type="modulelayout"
                    label="JFIELD_ALT_LAYOUT_LABEL"
                    class="custom-select"
                />
                <field
                    name="moduleclass_sfx"
                    type="textarea"
                    label="COM_MODULES_FIELD_MODULECLASS_SFX_LABEL"
                    rows="3"
                />
                <field
                    name="cache"
                    type="list"
                    label="COM_MODULES_FIELD_CACHING_LABEL"
                    default="0"
                >
                    <option value="1">JGLOBAL_USE_GLOBAL</option>
                    <option value="0">COM_MODULES_FIELD_VALUE_NOCACHING</option>
                </field>
                <field
                    name="cache_time"
                    type="number"
                    label="COM_MODULES_FIELD_CACHE_TIME_LABEL"
                    default="0"
                />
                <field
                    name="cachemode"
                    type="hidden"
                    default="itemid"
                >
                    <option value="itemid"></option>
                </field>
            </fieldset>
        </fields>
    </config>
</extension>
```

### tmpl/default.php bearbeiten

Man kann den Wert des Parameters verwenden, um einen Hyperlink im
Frontend zu erstellen. Dann kann über die Variable \$params auf den Wert
zugegriffen werden.

```php
    $domain = $params->get('domain', 'https://www.joomla.org');
```

Später nutzen wir diesen Wert für die Erstellung des Hyperlinks.

```php
<a href="<?php echo $domain; ?>">
	<?php echo '[PROJECT_NAME]' . $test; ?>
</a>
```

#### Vollständige tmpl/default.php Datei

Die vollständige `tmpl/default.php` Datei sieht so aus:

```php
<?php
/**
 * @package    [PACKAGE_NAME]
 *
 * @author     [AUTHOR] <[AUTHOR_EMAIL]>
 * @copyright  [COPYRIGHT]
 * @license    GNU General Public License version 2 or later; see LICENSE.txt
 * @link       [AUTHOR_URL]
 */

// No direct access to this file
defined('_JEXEC') or die;

$domain = $params->get('domain', 'https://www.joomla.org');
?>

<a href="<?php echo $domain; ?>">
	<?php echo '[PROJECT_NAME]' . $test; ?>
</a>
```

### language/en-GB/en-GB.mod_foo.ini bearbeiten

Hier kann der Text für die englische Version der Feldbezeichnungen
eingetragen werden.

```ini
    MOD_FOO_FIELD_URL_LABEL="Url"
```

#### Vollständige language/en-GB/en-GB.mod_foo.ini Datei

Die vollständige `language/en-GB/en-GB.mod_foo.ini` Datei sieht so aus:

```ini
    MOD_FOO="[PROJECT_NAME]"
    MOD_FOO_XML_DESCRIPTION="Foo Module"
    MOD_FOO_FIELD_URL_LABEL="Url"
```

## Modul testen

Jetzt können alle Dateien gezippt und über den Joomla Extension Manager
installiert werden. Anschließend kann das Modul im Modul-Manager
ausgewählt werden, wenn ein neues Site-Modul angelegt wird.

Im Backend erscheint das Modul wie es im nächsten Bild dargestellt wird.

Auf der Registerkarte „Modul“ ist jetzt das benutzerdefinierte Feld, in
das Sie eine Domain einzufügen ist. Der Text für die Beschriftung wird
aus der Sprachdatei abgerufen.

<img
src="https://docs.joomla.org/images/thumb/7/7e/Moduletutorial31-de.png/700px-Moduletutorial31-de.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/7/7e/Moduletutorial31-de.png/1050px-Moduletutorial31-de.png 1.5x, https://docs.joomla.org/images/7/7e/Moduletutorial31-de.png 2x"
data-file-width="1400" data-file-height="682" width="700" height="341"
alt="Moduletutorial31-de.png" />

Auf der Registerkarte „Erweitert“ werden alle Standardoptionen außer dem
Cache-Modus angezeigt, da das ein verstecktes Feld ist.

<img
src="https://docs.joomla.org/images/thumb/d/df/Moduletutorial32-de.png/700px-Moduletutorial32-de.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/d/df/Moduletutorial32-de.png/1050px-Moduletutorial32-de.png 1.5x, https://docs.joomla.org/images/d/df/Moduletutorial32-de.png 2x"
data-file-width="1400" data-file-height="682" width="700" height="341"
alt="Moduletutorial32-de.png" />

Im Frontend erscheint das Modul, wie im nächsten Bild dargestellt. Der
Text der Hilfedatei wird angezeigt und man sollte einen Hyperlink sehen.

<img
src="https://docs.joomla.org/images/thumb/d/d8/Moduletutorial33-de.png/700px-Moduletutorial33-de.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/d/d8/Moduletutorial33-de.png/1050px-Moduletutorial33-de.png 1.5x, https://docs.joomla.org/images/d/d8/Moduletutorial33-de.png 2x"
data-file-width="1400" data-file-height="682" width="700" height="341"
alt="Moduletutorial33-de.png" />

## Fazit

Formularfelder bieten dem User eine einfache Möglichkeit, das Modul an
die Einstellungen seiner Website anzupassen. Dadurch kann der
Anwendungsbereich der Module erweitert werden.

Vorlagen finden sich hier:

- <a
  href="https://github.com/joomla-extensions/boilerplate/tree/master/module"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/joomla-extensions/boilerplate/tree/master/module</a>

Die Beispieldateien für dieses Tutorial finden sich hier:

- <a
  href="https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules</a>

Beachten Sie folgendes Problem wegen möglicher zukünftiger Änderungen:

- <a href="https://github.com/joomla/joomla-cms/pull/23553"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/joomla/joomla-cms/pull/23553</a>

## Verwenden eines Skripts zum Installieren, Aktualisieren und Deinstallieren - Teil 4

### Voraussetzungen

Für dieses Tutorial wird Joomla! 4.x benötigt (zum Zeitpunkt des
Verfassens aktuell Joomla! 4.0.0-alpha6-dev)

### Skripte

Die Installation, Aktualisierung und Deinstallation eines Moduls kann
zusätzliche Operationen erfordern, die mit den in der XML-Hauptdatei
beschriebenen, grundlegenden Operationen nicht erreicht werden können.
Joomla bietet einen neuen Ansatz zur Lösung dieses Problems. Sie besteht
aus der Verwendung einer PHP-Skript-Datei, die eine Klasse mit fünf
Methoden enthält:

- preflight wird vor Installation und Update ausgeführt
- install
- update
- uninstall
- postflight wird nach Installation und Update ausgeführt

Lassen Sie uns einen Blick darauf werfen, wie man sie in Joomla 4
verwendet. Dazu verwenden wir die Helper-Datei.

### Dateistruktur

Die folgenden Dateien werden erstellt bzw. geändert:

- `script.php` – Diese Datei verwenden wir beispielhaft, sie muss neu
  angelegt werden.
- `language/en-GB/en-GB.mod_foo.sys.ini` – In diese Datei wird Text
  hinzugefügt.
- `mod_foo.xml` – Eine neue Datei wird erstellt, die während der
  Installation kopiert werden muss. Deshalb muss sie hier, in dieser
  Datei, angegeben werden.

### script.php erstellen

Das Schreiben eines Erweiterungsskripts besteht darin, eine Klasse mit
dem Namen `mod_ModuleNameInstallerScript` mit diesen 5 Methoden zu
deklarieren. Siehe die Kommentare in der Datei für weitere
Informationen. In diesem Kommentar erkläre ich, wann eine Methode
aufgerufen wird.

In diesem Beispiel verwende ich nur Text, um anzuzeigen, wann welche
Methode ausgeführt wird. Darüber hinaus zeige ich Ihnen, wie Sie die
Mindestanforderungen überprüfen können. Natürlich können Sie noch viel
mehr tun. Beispielsweise können Sie Dateien löschen, die in einer neuen
Version Ihres Moduls nicht mehr benötigt werden. Dies ist in der Datei
zu sehen. Eine weitere Möglichkeit für diese Datei ist, Beispielinhalte
zu erstellen, eine Erfolgsmeldung mit der aktuell installierten
Versionsnummer auszugeben oder nach erfolgreicher Installation auf die
Seite mit den Modul-Einstellungen zu wechseln.

#### Vollständige script.php

Die vollständige `script.php` Datei sieht so aus:

```php
<?php
/**
 * @package    [PACKAGE_NAME]
 *
 * @author     [AUTHOR] <[AUTHOR_EMAIL]>
 * @copyright  [COPYRIGHT]
 * @license    GNU General Public License version 2 or later; see LICENSE.txt
 * @link       [AUTHOR_URL]
 */

// No direct access to this file
defined('_JEXEC') or die;

use Joomla\CMS\Language\Text;
use Joomla\CMS\Log\Log;

/**
 * Script file of Foo module
 */
class mod_fooInstallerScript {

    /**
     * Extension script constructor.
     *
     * @return  void
     */
    public function __construct() {
        $this->minimumJoomla = '4.0';
        $this->minimumPhp = JOOMLA_MINIMUM_PHP;
    }

    /**
     * Method to install the extension
     *
     * @param   InstallerAdapter  $parent  The class calling this method
     *
     * @return  boolean  True on success
     */
    function install($parent) {
        echo Text::_('MOD_FOO_INSTALLERSCRIPT_INSTALL');

        return true;
    }

    /**
     * Method to uninstall the extension
     *
     * @param   InstallerAdapter  $parent  The class calling this method
     *
     * @return  boolean  True on success
     */
    function uninstall($parent) {
        echo Text::_('MOD_FOO_INSTALLERSCRIPT_UNINSTALL');

        return true;
    }

    /**
     * Method to update the extension
     *
     * @param   InstallerAdapter  $parent  The class calling this method
     *
     * @return  boolean  True on success
     */
    function update($parent) {
        echo Text::_('MOD_FOO_INSTALLERSCRIPT_UPDATE');

        return true;
    }

    /**
     * Function called before extension installation/update/removal procedure commences
     *
     * @param   string            $type    The type of change (install, update or discover_install, not uninstall)
     * @param   InstallerAdapter  $parent  The class calling this method
     *
     * @return  boolean  True on success
     */
    function preflight($type, $parent) {
        // Check for the minimum PHP version before continuing
        if (!empty($this->minimumPhp) && version_compare(PHP_VERSION, $this->minimumPhp, '<')) {
            Log::add(Text::sprintf('JLIB_INSTALLER_MINIMUM_PHP', $this->minimumPhp), Log::WARNING, 'jerror');

            return false;
        }

        // Check for the minimum Joomla version before continuing
        if (!empty($this->minimumJoomla) && version_compare(JVERSION, $this->minimumJoomla, '<')) {
            Log::add(Text::sprintf('JLIB_INSTALLER_MINIMUM_JOOMLA', $this->minimumJoomla), Log::WARNING, 'jerror');

            return false;
        }

        echo Text::_('MOD_FOO_INSTALLERSCRIPT_PREFLIGHT');
        echo $this->minimumJoomla . ' ' . $this->minimumPhp;

        return true;
    }

    /**
     * Function called after extension installation/update/removal procedure commences
     *
     * @param   string            $type    The type of change (install, update or discover_install, not uninstall)
     * @param   InstallerAdapter  $parent  The class calling this method
     *
     * @return  boolean  True on success
     */
    function postflight($type, $parent) {
        echo Text::_('MOD_FOO_INSTALLERSCRIPT_POSTFLIGHT');

        return true;
    }
}
```

### Bearbeiten von language/en-GB/en-GB.mod_foo.sys.ini

Es gibt hier nicht viel zu erklären. Den Text für die Übersetzung in
diese Datei übernehmen.

#### Vollständige language/en-GB/en-GB.mod_foo.ini Datei

Die vollständige `language/en-GB/en-GB.mod_foo.sys.ini` Datei sieht wie
folgt aus:

```ini
    MOD_FOO="[PROJECT_NAME]"
    MOD_FOO_XML_DESCRIPTION="Foo Module"
    MOD_FOO_INSTALLERSCRIPT_PREFLIGHT="Anything here happens before the installation/update/uninstallation of the module"
    MOD_FOO_INSTALLERSCRIPT_UPDATE="The module has been updated"
    MOD_FOO_INSTALLERSCRIPT_UNINSTALL="The module has been uninstalled"
    MOD_FOO_INSTALLERSCRIPT_INSTALL="The module has been installed"
    MOD_FOO_INSTALLERSCRIPT_POSTFLIGHT="Anything here happens after the installation/update/uninstallation of the module"
```

### mod_foo.xml bearbeiten

Damit das Skript in Joomla automatisch aufgerufen wird muss hier eine
Zeile hinzugefügt werden.

```xml
<scriptfile>script.php</scriptfile>
```

#### Vollständige mod_foo.xml

Die vollständige `mod_foo.xml` Datei sieht so aus:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="module" version="4.0" client="site" method="upgrade">
    <name>MOD_FOO</name>
    <creationDate>[DATE]</creationDate>
    <author>[AUTHOR]</author>
    <authorEmail>[AUTHOR_EMAIL]</authorEmail>
    <authorUrl>[AUTHOR_URL]</authorUrl>
    <copyright>[COPYRIGHT]</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <version>1.0</version>
    <description>MOD_FOO_XML_DESCRIPTION</description>
    <namespace>Joomla\Module\Foo</namespace>
    <scriptfile>script.php</scriptfile>
    <files>
        <filename module="mod_foo">mod_foo.php</filename>
        <folder>tmpl</folder>
        <folder>Helper</folder>
        <folder>language</folder>
        <filename>mod_foo.xml</filename>
    </files>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="domain"
                    type="url"
                    label="MOD_FOO_FIELD_URL_LABEL"
                    filter="url"
                />
            </fieldset>
            <fieldset name="advanced">
                <field
                    name="layout"
                    type="modulelayout"
                    label="JFIELD_ALT_LAYOUT_LABEL"
                    class="custom-select"
                />
                <field
                    name="moduleclass_sfx"
                    type="textarea"
                    label="COM_MODULES_FIELD_MODULECLASS_SFX_LABEL"
                    rows="3"
                />
                <field
                    name="cache"
                    type="list"
                    label="COM_MODULES_FIELD_CACHING_LABEL"
                    default="0"
                >
                    <option value="1">JGLOBAL_USE_GLOBAL</option>
                    <option value="0">COM_MODULES_FIELD_VALUE_NOCACHING</option>
                </field>
                <field
                    name="cache_time"
                    type="number"
                    label="COM_MODULES_FIELD_CACHE_TIME_LABEL"
                    default="0"
                />
                <field
                    name="cachemode"
                    type="hidden"
                    default="itemid"
                >
                    <option value="itemid"></option>
                </field>
            </fieldset>
        </fields>
    </config>
</extension>
```

## Modul testen

Jetzt können alle Dateien gezippt und über den Joomla Extension Manager
installiert werden. Sofort nach der Installation werden folgende
Informationen angezeigt. Das wurde ja so im Script eingetragen.

<img
src="https://docs.joomla.org/images/thumb/f/fc/Moduletutorial14-de.png/700px-Moduletutorial14-de.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/f/fc/Moduletutorial14-de.png/1050px-Moduletutorial14-de.png 1.5x, https://docs.joomla.org/images/f/fc/Moduletutorial14-de.png 2x"
data-file-width="1400" data-file-height="682" width="700" height="341"
alt="Moduletutorial14-de.png" />

## Fazit

Wie Sie sehen können, bietet Ihnen Joomla eine Menge, um Ihre
Erweiterung einfach zu bedienen - von Anfang an.

Vorlagen finden sich hier:

- <a
  href="https://github.com/joomla-extensions/boilerplate/tree/master/module"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/joomla-extensions/boilerplate/tree/master/module</a>

Die Beispieldateien für dieses Tutorial finden sich hier:

- <a
  href="https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules</a>

## Joomla Updater benutzen: Auto-Update einfügen – Teil 5

### Voraussetzungen

Für dieses Tutorial wird Joomla! 4.x benötigt (zum Zeitpunkt des
Verfassens aktuell Joomla! 4.0.0-alpha6-dev)

### Joomla Updater

Das erste, was Sie tun müssen, ist, das [Managing Component Upgrades
with Joomla 2.5 - Part
1](https://docs.joomla.org/J2.5:Managing_Component_Updates "Special:MyLanguage/J2.5:Managing Component Updates")-Tutorial
zu lesen, um eine Vorstellung davon zu vermitteln, wie der
Upgrade-Prozess in Joomla! funktioniert. Obwohl für 2.5 geschrieben, hat
sich der Prozess nicht geändert. Lesen Sie auch [Einen Update-Server
bereitstellen](https://docs.joomla.org/Deploying_an_Update_Server "Special:MyLanguage/Deploying an Update Server") -
das ist es, was wir in diesem Tutorial implementieren werden.

### Dateistruktur

Die folgenden Dateien werden erstellt/geändert:

- `mod_foo.xml` – Die einzige Möglichkeit, unser vorhandenes Modul zu
  aktualisieren, besteht darin, einen Aktualisierungsserver –
  beispielsweise `http://www.example.com/foo_update.xml` – in die
  XML-Datei einzufügen.
- `foo_update.xml` – Dann müssen wir die XML-Datei für den Update-Server
  bei `http://www.example.com/foo_update.xml` erstellen, um Joomla
  mitzuteilen, dass ein Update verfügbar ist.

#### mod_foo.xml bearbeiten

Um den Update-Server einzufügen, müssen wir die folgenden Zeilen in die
XML-Datei einfügen:

```xml
<updateservers>
    <server type="extension" priority="1" name="[PROJECT_NAME]">https://www.example.com/mod_foo.xml</server>
</updateservers>
```

#### Vollständige mod_foo.xml

Die vollständige `mod_foo.xml` Datei sieht so aus:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="module" version="4.0" client="site" method="upgrade">
    <name>MOD_FOO</name>
    <creationDate>[DATE]</creationDate>
    <author>[AUTHOR]</author>
    <authorEmail>[AUTHOR_EMAIL]</authorEmail>
    <authorUrl>[AUTHOR_URL]</authorUrl>
    <copyright>[COPYRIGHT]</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <version>1.0</version>
    <description>MOD_FOO_XML_DESCRIPTION</description>
    <namespace>Joomla\Module\Foo</namespace>
    <files>
        <filename module="mod_foo">mod_foo.php</filename>
        <folder>tmpl</folder>
        <folder>Helper</folder>
        <folder>language</folder>
        <filename>mod_foo.xml</filename>
    </files>
    <updateservers>
        <server type="extension" priority="1" name="[PROJECT_NAME]">https://www.example.com/mod_foo.xml</server>
    </updateservers>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="domain"
                    type="url"
                    label="MOD_FOO_FIELD_URL_LABEL"
                    filter="url"
                />
            </fieldset>
            <fieldset name="advanced">
                <field
                    name="layout"
                    type="modulelayout"
                    label="JFIELD_ALT_LAYOUT_LABEL"
                    class="custom-select"
                />
                <field
                    name="moduleclass_sfx"
                    type="textarea"
                    label="COM_MODULES_FIELD_MODULECLASS_SFX_LABEL"
                    rows="3"
                />
                <field
                    name="cache"
                    type="list"
                    label="COM_MODULES_FIELD_CACHING_LABEL"
                    default="0"
                >
                    <option value="1">JGLOBAL_USE_GLOBAL</option>
                    <option value="0">COM_MODULES_FIELD_VALUE_NOCACHING</option>
                </field>
                <field
                    name="cache_time"
                    type="number"
                    label="COM_MODULES_FIELD_CACHE_TIME_LABEL"
                    default="0"
                />
                <field
                    name="cachemode"
                    type="hidden"
                    default="itemid"
                >
                    <option value="itemid"></option>
                </field>
            </fieldset>
        </fields>
    </config>
</extension>
```

Nun, da Joomla! nach Updates für unsere Erweiterung sucht - lassen Sie
uns eine erstellen, um den Prozess zu testen. Aber zuerst müssen wir die
Datei `foo_update.xml` erstellen. Bisher haben wir nur den Update-Server
beschrieben. Wir wissen noch nicht, ob es ein Update gibt. In der Datei
`foo_update.xml` geben wir an, wann eine neue Version veröffentlicht
wird und wo sie heruntergeladen werden kann.

### foo_update.xml erstellen

Jetzt müssen wir die XML-Datei bei
`http://www.example.com/foo_update.xml` erstellen, um Joomla
mitzuteilen, dass ein Update verfügbar ist.

#### Vollständige foo_update.xml

Die vollständige `foo_update.xml` Datei sieht so aus:

```xml
<?xml version="1.0" ?>
<updates>
    <update>
        <name>Foo</name>
        <description>This is mod_foo 1.0.1</description>
        <element>mod_foo</element>
        <type>module</type>
        <version>1.0.1</version>
        <downloads>
            <downloadurl type="full" format="zip">http://www.example.com/mod_foo_101.zip</downloadurl>
        </downloads>
        <maintainer>Joomla</maintainer>
        <maintainerurl>http://www.example.com</maintainerurl>
        <targetplatform name="joomla" version="4.0"/>
        <client>site</client>
    </update>
</updates>
```

Nach dem Hochladen dieser Datei an die Adresse
`https://www.example.com/mod_foo.xml` wird das Update im Joomla-Backend
angezeigt.

### Modul-Update testen

Jetzt eine Kopie des Moduls erstellen, so wie es aktuell ist, dann die
Versionsnummer auf 1.0.1 aktualisieren und nun alle Dateien
komprimieren. Danach kann die Zip-Datei auf die URL
`http://www.example.com/mod_foo_101.zip` geladen werden. Anschließend
kann das Modul über den Joomla Updater aktualisiert werden.

### Kommerzielle Module

Beachten Sie, dass wir in unseren Dateien auf die XML-Datei für
Erweiterungs-Updates verlinkt haben. Und aus dieser XML-Datei haben wir
einen Speicherort für die Zip-Datei des Moduls ausgelesen. Das bedeutet,
dass jemand, der dies rückwärts nachverfolgen sollte, die physische
Quelle der Zip-Datei Ihres Moduls finden könnte. Wenn Ihnen das nicht
gefällt, finden Sie eine Lösung in diesem Pull-Request:
<a href="https://github.com/joomla/joomla-cms/pull/15185"
class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla/joomla-cms/pull/15185</a>

### Fazit

Mit dem Joomla! Updater können Sie auf einfache Weise alle Benutzer
erreichen und ihnen mitteilen, dass es eine neue Version gibt. Auch das
Update selbst ist einfach.

Jede Erweiterung sollte einen Update-Server verwenden, vor allem aus
Sicherheitsgründen.

Vorlagen finden sich hier:

- <a
  href="https://github.com/joomla-extensions/boilerplate/tree/master/module"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/joomla-extensions/boilerplate/tree/master/module</a>

Die Beispieldateien für dieses Tutorial finden sich hier:

- <a
  href="https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules</a>

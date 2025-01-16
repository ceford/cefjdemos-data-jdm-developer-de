<!-- Filename: J4.x:J4_Module_example_-_Mydownmsg / Display title: Beispiel: Nachricht bei Website-Ausfall -->

## Einführung

Dieser Beitrag präsentiert ein komplettes Joomla-Site-Modul für neue Entwickler, damit sie dessen Struktur und Funktionsweise nachvollziehen können. Es ersetzt ein vorheriges Modul, das ältere Codierkonventionen verwendete. Dieses ist für Joomla 5 und höher konzipiert.

Es gibt einführende Beiträge zu *Allgemeine Konzepte* und *Module* in der Joomla Programmiererdokumentation und im Jdocmanual für Ihre Bequemlichkeit reproduziert. Dieser Beitrag ähnelt dem JPD [Basis-Modul](jdocmanual?article=docus/modules/basic-module), bietet jedoch einige zusätzliche Funktionen.

## Zweck

Das Modul ist so konzipiert, dass es eine temporäre Nachricht in einer von mehreren Sprachen für einen kurzen Zeitraum anzeigt, bevor die Website wegen Systemwartung geschlossen wird. Das gibt den Benutzern die Möglichkeit, ihre Tätigkeiten abzuschließen, bevor die Schließung erfolgt.

Der Modulname ist `mod_downmsg` und die Nachricht, die es auf Englisch anzeigt, lautet ähnlich wie *Diese Seite wird um 12:00 Uhr GMT für eine kurze Zeit schließen*. Die Zeit und die Nachricht können angepasst werden, um der bevorstehenden Schließung der Seite gerecht zu werden.

Das Modul kann von GitHub heruntergeladen werden, um es nach Bedarf zu installieren oder zu untersuchen.

## Quellcode-Struktur

Im Folgenden sehen Sie eine schematische Darstellung der Struktur des Quellcodes, der zur Erstellung der Erweiterung verwendet wird:

```
cefjdemos-plg-toc
|-- .vscode (folder for build settings)
|-- mod_downmsg (folder compressed to create an installable zip file)
    |-- help
        |-- en-GB
            |-- downmsg.html (this is Help screen used in the backend module edit form)
    |-- language
        |-- de-DE
            |-- mod_downmsg.ini (only frontend strings are included)
        |-- en-GB (language folder, kept with the extension code)
            |-- mod_downmsg.ini
            |-- mod_downmsg.sys.ini
        |-- fr-FR
            |-- mod_downmsg.ini (only frontend strings are included)
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Dispatcher (folder for extension specific code)
            |-- Dispatcher.php (the extension class code)
    |-- tmpl (folder for layouts)
        |-- default.php (the table of contents layout)
    |-- mod_downmsg.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- changelog.xml (record of changes)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
|-- updates.xml (update server specification)
```

Dies ist die Struktur, wie sie in der VSCode oder VSCodium IDE zu sehen ist:

![Plugin-Entwicklungs-Dateistruktur in vscodium](../../../en/images/modules/downmsg-module-vscodium.png)

## Die Manifestdatei

Die Manifestdatei steuert die Installations-, Aktualisierungs- und Deinstallationsprozesse der Erweiterung:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<extension type="module" method="upgrade" client="site">
    <name>mod_downmsg</name>
    <creationDate>December 2024</creationDate>
    <author>Clifford E Ford</author>
    <authorEmail>cliff@xxx.yyy.co.uk</authorEmail>
    <authorUrl>http://jdocmanual.org/</authorUrl>
    <copyright>Copyright (C) 2024 Clifford E Ford, All rights reserved.</copyright>
    <license>GNU/GPLv3 http://www.gnu.org/licenses/gpl-3.0.html</license>
    <!--  The version string is recorded in the components table -->
    <version>0.1.0</version>
    <!-- The description is optional and defaults to the name -->
    <description>MOD_DOWNMSG_XML_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Module\Downmsg</namespace>
    <files>
        <folder>help</folder>
        <folder>language</folder>
        <folder module="mod_downmsg">services</folder>
        <folder>src</folder>
        <folder>tmpl</folder>
    </files>
    <help url="MOD_DOWNMSG_HELP_URL" />
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-mod-downmsg/main/changelog.xml</changelogurl>
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" priority="2" name="Mod Downmsg Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-mod-downmsg/main/updates.xml</server>
    </updateservers>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="msg_id"
                    type="list"
                    label="MOD_DOWNMSG_PARAMS_MSG_ID_LABEL"
                    description="MOD_DOWNMSG_PARAMS_MSG_ID_DESC"
                    default="v1"
                    required="true"
                >
                    <option value="v1">MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V1</option>
                    <option value="v2">MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V2</option>
                </field>
                <field
                    name="hour"
                    type="number"
                    label="MOD_DOWNMSG_PARAMS_HOUR_LABEL"
                    description="MOD_DOWNMSG_PARAMS_HOUR_DESC"
                    default="12"
                    min="0"
                    max="23"
                    required="true"
                />
                <field
                    name="minute"
                    type="number"
                    label="MOD_DOWNMSG_PARAMS_MINUTE_LABEL"
                    description="MOD_DOWNMSG_PARAMS_MINUTE_DESC"
                    default="00"
                    min="00"
                    max="59"
                    required="true"
                />
                <field
                    name="tz"
                    type="number"
                    label="MOD_DOWNMSG_PARAMS_TZ_LABEL"
                    description="MOD_DOWNMSG_PARAMS_TZ_DESC"
                    default="0"
                    min="-11"
                    max="11"
                    required="true"
                />
            </fieldset>
        </fields>
    </config>
</extension>
```

### Elementhinweise

- Die **Erweiterungs**attribute:
  - **type="module"** gibt den Typ der Erweiterung an.
  - **method="upgrade"** bedeutet, dass diese Erweiterung installiert und aktualisiert werden kann, wenn neuere Versionen verfügbar werden.
  - **client="site"** teilt Joomla mit, dass es sich um ein *Site*-Modul und nicht um ein *Administrator*-Modul handelt.
- Die **versions**-Anweisung wird verwendet, um Aktualisierungen zu steuern. Wenn ein Update-Server verfügbar ist, wird er mit der dort aufgezeichneten Version verglichen. Es verhindert auch, dass eine ältere Version eine neuere Version überschreibt.
- Das **namespace**-Pfadattribut teilt Joomla mit, wo nach Namespace-Klassen gesucht werden soll.
- Der **files**-Abschnitt listet Ordner und Dateien zur Installation auf. Wenn ein Element hier nicht vorhanden ist, wird es nicht installiert, selbst wenn es in der gezippten Installationsdatei vorhanden ist.
- Das **hilfe**-URL-Attribut ermöglicht die Verwendung einer benutzerdefinierten Hilfedatei im Modulbearbeitungsformular. Der Pfad zur Hilfedatei ist ein Wert für den angegebenen Schlüssel in der en-GB/mod_downmsg.ini-Datei.
- Der **config**-Abschnitt zeigt, welche Parameter dieses Modul benötigt. Alle sollten Standardwerte haben, da sie bei der Installation festgelegt werden.

## Die Sprachdateien

Dieses Modul enthält sehr wenige Zeichenfolgen. Die in den **sys.ini**-Dateien verwendeten Zeichenfolgen werden bei der Installation und zur Auflistung unter anderen Modulen verwendet. Die **.ini**-Dateien werden im Administratorformular und bei der Seitenwiedergabe des Moduls verwendet. Beachten Sie die Konventionen bei der Identifizierung von Zeichenfolgen:

MOD\_\[Name\]\_\[Zweck\]\_\[Verwendung\]

Die folgenden Dateien zeigen, dass die deutschen und französischen Übersetzungen nur die Zeichenketten enthalten, die für die Seitenbesucher sichtbar sind, da das Parameterformular immer auf Englisch verwaltet wird. Für den Fall, dass Sie es nicht wussten: en-GB-Zeichenketten werden standardmäßig immer zuerst geladen, um sicherzustellen, dass versehentlich nicht übersetzte Zeichenketten nicht als Zeichenkettenschlüssel erscheinen. Die Zeichenketten der benötigten Sprache werden dann geladen und überschreiben die entsprechenden englischen Zeichenketten.

Die französischen und deutschen Versionen der Zeichenfolgen hier wurden mit den Google-Übersetzungstools erhalten. Dies funktioniert möglicherweise besser für Sätze als für einzelne Wörter!

### de-DE/mod_downmsg.ini

```ini
MOD_DOWNMSG="System Down Message"
MOD_DOWNMSG_XML_DESCRIPTION="Show a message about imminent system down time."

MOD_DOWNMSG_HELP_URL="../modules/mod_downmsg/help/en-GB/downmsg.html"

MOD_DOWNMSG_MSG_V1="This site will close for a short period at %s"
MOD_DOWNMSG_MSG_V2="Please log out. This site will close for one hour or more at %s"

MOD_DOWNMSG_PARAMS_MSG_ID_LABEL="Select Message version"
MOD_DOWNMSG_PARAMS_MSG_ID_DESC="The short downtime or long downtime message vesion."

MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V1="Short < 1 hour"
MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V2="Long > 1 hour"

MOD_DOWNMSG_PARAMS_HOUR_LABEL="Hour"
MOD_DOWNMSG_PARAMS_HOUR_DESC="Set the hour when the site will close"

MOD_DOWNMSG_PARAMS_MINUTE_LABEL="Minute"
MOD_DOWNMSG_PARAMS_MINUTE_DESC="Set the minutes when the site will close"

MOD_DOWNMSG_PARAMS_TZ_LABEL="Time Zone"
MOD_DOWNMSG_PARAMS_TZ_DESC="Set your time zone offset from GMT"
```

### de-DE/mod_downmsg.sys.ini

Diese Datei wird für die Systemverwaltung verwendet.

```ini
MOD_DOWNMSG="System Down Message"
MOD_DOWNMSG_XML_DESCRIPTION="Show a message about imminent system down time."
```

### de-DE/mod_downmsg.ini

```ini
MOD_DOWNMSG_MSG_V1="Diese Seite wird für kurze Zeit um %s geschlossen"
MOD_DOWNMSG_MSG_V2="Bitte melden Sie sich ab. Diese Seite wird für eine Stunde oder länger um %s geschlossen"
MOD_DOWNMSG_HELP_URL="../modules/mod_downmsg/help/en-GB/downmsg.html"
```

### fr-FR/mod_downmsg.ini

```ini
MOD_DOWNMSG_MSG_V1="Ce site sera fermé pour une courte période à %s"
MOD_DOWNMSG_MSG_V2="Veuillez vous déconnecter. Ce site sera fermé pendant une heure ou plus à %s"
MOD_DOWNMSG_HELP_URL="../modules/mod_downmsg/help/en-GB/downmsg.html"
```

## Der Modulcode

Der Modulcode ist unglaublich einfach. Es gibt drei PHP-Dateien:

- services/provider.php (der Code für die Abhängigkeitsinjektion).
- src/Dispatcher/Dispatcher.php (stellt Daten zusammen, die zur Anzeige verwendet werden sollen).
- tmpl/default.php (die Vorlage zum Anzeigen der Daten, die überschrieben werden kann).

### dienste/anbieter.php

```php
<?php

/**
 * @package     Cefjdemos.Site
 * @subpackage  mod_downmsg
 *
 * @copyright   (C) 2023 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Extension\Service\Provider\Module;
use Joomla\CMS\Extension\Service\Provider\ModuleDispatcherFactory;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;

/**
 * The module Downmsg service provider.
 *
 * @since  4.4.0
 */
return new class () implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.4.0
     */
    public function register(Container $container): void
    {
        $container->registerServiceProvider(new ModuleDispatcherFactory('\\Cefjdemos\\Module\\Downmsg'));
        $container->registerServiceProvider(new Module());
    }
};
```

### src/Dispatcher/Dispatcher.php

```php
<?php

/**
 * @package     Cefjdemos.Site
 * @subpackage  mod_downmsg
 *
 * @copyright   (C) 2023 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace Cefjdemos\Module\Downmsg\Site\Dispatcher;

use Joomla\CMS\Dispatcher\AbstractModuleDispatcher;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

/**
 * Dispatcher class for mod_downmsg
 *
 * @since  4.4.0
 */
class Dispatcher extends AbstractModuleDispatcher
{
    /**
     * Returns the layout data.
     *
     * @return  array
     *
     * @since   4.4.0
     */
    protected function getLayoutData()
    {
        $data = parent::getLayoutData();

        return $data;
    }
}
```

### tmpl/default.php

```php
<?php

/**
 * @package     Cefjdemos.Module
 * @subpackage  mod_downmsg
 *
 * @copyright   Copyright (C) 2019 Clifford E Ford. All rights reserved.
 * @license     GNU/GPLv3 http://www.gnu.org/licenses/gpl-3.0.html
 */

defined('_JEXEC') or die;

use Joomla\CMS\Language\Text;

// the $msg string contains a %s placeholder to be replaced in a sprintf statement
$msg = Text::_('MOD_DOWNMSG_MSG_' . strtoupper($params->get('msg_id')));

$tod = $params->get('hour') . ':' . $params->get('minute');
$tz = $params->get('tz');

if ($tz > 0) {
    $tz = '(+' . $tz . ')';
} elseif ($tz < 0) {
    $tz = '(-' . $tz . ')';
} else {
    $tz = '';
}

$tod .= ' GMT ' . $tz;

?>

<div class="alert alert-warning text-center" role="alert">
    <?php echo sprintf($msg, $tod); ?>
</div>
```

## Installation

Aus dem Administrator-Menü navigieren Sie zur Seite System / Installieren / Erweiterungen und verwenden Sie die Registerkarte Paketdatei hochladen, um die ZIP-Datei hochzuladen. Navigieren Sie dann zu Inhalte / Site-Module und bearbeiten Sie das neu installierte Modul.

Im Modulbearbeitungsformular:

1. Geben Sie einen Titel ein. Denken Sie daran, dass ein Modul mehr als eine Instanz haben kann, sodass sie an verschiedenen Orten erscheinen oder unterschiedliche Parameter haben können.
2. Legen Sie die Zeit fest, zu der die Website offline gehen wird.
3. Stellen Sie die Zeitzone ein (es gibt wahrscheinlich eine bessere Methode als die Verwendung von GMT +- n Stunden).

In der gemeinsamen Parameterliste auf der rechten Seite

1. Setzen Sie den **Titel** auf **verstecken**.
2. Wählen Sie eine **Position** für die Vorlage - in Cassiopeia platziert **der Topbar** die Nachricht über dem Seiteninhalt, perfekt.
3. Setzen Sie den **Status** auf **Veröffentlicht**.
4. Wählen Sie im Tab **Menüzuweisung** die Option **Auf allen Seiten**.
5. **Speichern** und Sie sind bereit, das Erscheinungsbild der Seite zu überprüfen.

![das Modulbearbeitungsformular](../../../en/images/modules/downmsg-module-edit-form.png)

## Testen

So erscheint die Nachricht auf Englisch:

![Website nicht erreichbar Nachricht auf Deutsch](../../../en/images/modules/downmsg-module-result-de.png)

In Deutsch:

# Einführung in die Programmierung

Das Programmieren kann für Anfänger einschüchternd wirken, aber mit den richtigen Ressourcen und etwas Geduld wird es zugänglicher. In dieser Serie von Beiträgen werden wir die Grundlagen der Programmierung anhand der Sprache Python erkunden. Python ist bekannt für seine Einfachheit und Lesbarkeit, was es zu einer ausgezeichneten Wahl für Neulinge macht.

## Was ist Programmierung?

Programmieren ist der Prozess des Entwerfens und Erstellens von Anweisungen, die einem Computer sagen, was er tun soll. Diese Anweisungen, auch als Code bekannt, werden in einer Sprache geschrieben, die der Computer verstehen kann. Jede Programmiersprache hat ihre eigene Syntax und Struktur, aber das zugrunde liegende Konzept bleibt gleich: Aufgaben automatisieren und Probleme lösen.

## Warum Python lernen?

- **Einfach zu lernen**: Python hat eine klare und konsistente Syntax, die es leicht macht, Code zu lesen und zu schreiben.
- **Vielseitig**: Python kann für Webentwicklung, Datenanalyse, maschinelles Lernen und mehr verwendet werden.
- **Aktive Community**: Es gibt eine große Gemeinschaft von Python-Entwicklern, die bereit sind, Unterstützung zu leisten und Ressourcen zu teilen.

Im nächsten Beitrag werden wir die grundlegende Syntax von Python untersuchen und unser erstes Programm schreiben. Bleiben Sie dran!

![Seite ausgefallen Nachricht auf Englisch](../../../en/images/modules/downmsg-module-result-de.png)

Und Französisch:

![Seite nicht verfügbar Nachricht auf Deutsch](../../../en/images/modules/downmsg-module-result-fr.png)

## Website aktualisieren und Änderungsprotokoll

Diese Beiträge sind in der Quelle enthalten, werden hier jedoch nicht behandelt.

*Übersetzt von openai.com*

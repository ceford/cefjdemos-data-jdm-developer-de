<!-- Filename: J4.x:J4_Plugin_example_-_Table_of_Contents / Display title: Beispiel: Inhaltsverzeichnis -->

## Einführung

Dieser Beitrag bietet ein Beispiel für ein Content-Plugin, um das Codieren mithilfe der neuesten Joomla-Codierungsrichtlinien zu veranschaulichen. Der vollständige Code ist auf [GitHub](https://github.com/ceford/cefjdemos-plg-content-toc) verfügbar. Sie können es herunterladen und installieren oder einfach entpacken, um den Code zu überprüfen.

Es gibt einen Abschnitt zur [Plugin-Entwicklung](https://manual.joomla.org/docs/building-extensions/plugins/) und weitere Beispiele in der Joomla Programmierer-Dokumentation, [zur Bequemlichkeit auf dieser Seite kopiert](jdocmanual?article=docus/plugins/index).

Dieses Plugin erzeugt ein *Inhaltsverzeichnis* aus den Überschriften in jedem Beitrag, der einen `{cefjdemostoc}`-Platzhalter enthält. Das Ergebnis wird im letzten Screenshot in diesem Beitrag veranschaulicht.

## Quellcode-Struktur

Im Folgenden ist eine schematische Darstellung der Struktur des Quellcodes, der zur Erstellung der Erweiterung verwendet wird:

```
cefjdemos-plg-toc
|-- .vscode (folder for build settings)
|-- plg_content_cefjdemostoc (folder compressed to create an installable zip file)
    |-- language/en-GB/ (language folder, kept with the extension code)
        |-- plg_content_cefjdemostoc.ini
        |-- plg_content_cefjdemostoc.sys.ini
    |-- media
        |-- css
            |-- cefjdemostoc.css (layout styles)
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Extension (folder for extension specific code)
            |-- Cefjdemostoc.php (the extension class code)
    |-- tmpl (folder for layouts)
        |-- default.php (the table of contents layout)
    |-- cefjdemostoc.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
```

Dies ist die Struktur, wie sie in der VSCode- oder VSCodium-IDE zu sehen ist:

![Plugin-Entwicklungsdateistruktur in vscodium](../../../en/images/plugins/cefjdemostoc-vscodium.png)

## Die Manifestdatei

Jede Erweiterung muss eine Manifestdatei im XML-Format haben. Diese wird von Joomla für die Installation, Aktualisierung und Entfernung verwendet. Dies ist die Manifestdatei für diese Erweiterung:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<extension type="plugin" group="content" method="upgrade">
    <name>plg_content_cefjdemostoc</name>
    <author>Clifford E Ford</author>
    <creationDate>2024-12-23</creationDate>
    <copyright>(C) 2024 Clifford E Ford</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <authorEmail>cliff@xxxx.yyyy.co.uk</authorEmail>
    <authorUrl>jdocmanual.org</authorUrl>
    <version>0.2.2</version>
    <description>PLG_CONTENT_CEFJDEMOSTOC_XML_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Plugin\Content\Cefjdemostoc</namespace>
    <files>
        <folder plugin="cefjdemostoc">services</folder>
        <folder>src</folder>
        <folder>language</folder>
    </files>
    <media destination="plg_content_cefjdemostoc" folder="media">
        <folder>css</folder>
    </media>
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-plg-content-toc/main/changelog.xml</changelogurl>
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" priority="2" name="Cefjdemostoc Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-plg-content-toc/main/updates.xml</server>
    </updateservers>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="help"
                    type="list"
                    label="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_LABEL"
                    description="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_DESC"
                    default="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_DEFAULT"
                >
                    <option value="">PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_DEFAULT</option>
                </field>

                <field
                    name="toc_class"
                    type="text"
                    label="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_CLASS_LABEL"
                    description="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_CLASS_DESC"
                />

                <field
                    name="toc_head_class"
                    type="list"
                    label="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_HEAD_CLASS_LABEL"
                    description="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_HEAD_CLASS_DESC"
                    default="center"
                >
                    <option value="text-center">text-center</option>
                    <option value="text-start">text-start</option>
                </field>
            </fieldset>
        </fields>
    </config>
</extension>
```

## Die Datei services/provider.php

Der Zweck dieser Datei ist es, einen Dienstanbieter zu registrieren und alle benötigten Werkzeuge zu deklarieren. Dieses spezielle Plugin benötigt nicht viel. Wenn Ihr benutzerdefiniertes Plugin Datenbankabfragen durchführen müsste, würden Sie es hier deklarieren. Schauen Sie sich ein Authentifizierungs-Plugin an. Sie benötigen Datenbank- und Benutzerwerkzeuge.

```php
<?php

/**
 * @package     Cefjdemostoc.Plugin
 * @subpackage  Content.cefjdemostoc
 *
 * @copyright   (C) 2024 Clifford E Ford <https://jdocmanual.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Extension\PluginInterface;
use Joomla\CMS\Factory;
use Joomla\CMS\Plugin\PluginHelper;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;
use Joomla\Event\DispatcherInterface;
use Cefjdemos\Plugin\Content\Cefjdemostoc\Extension\Cefjdemostoc;

return new class () implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.3.0
     */
    public function register(Container $container)
    {
        $container->set(
            PluginInterface::class,
            function (Container $container) {
                $plugin     = new Cefjdemostoc(
                    $container->get(DispatcherInterface::class),
                    (array) PluginHelper::getPlugin('content', 'cefjdemostoc')
                );
                $plugin->setApplication(Factory::getApplication());

                return $plugin;
            }
        );
    }
};
```

## Die Erweiterungsklasse

Der Zweck der Datei src/Extension/Cefjdemostoc.php besteht darin, Daten für die Ausgabe vorzubereiten. Die Datei ist 91 Zeilen lang und wird daher hier nicht vollständig wiedergegeben. Einige Teile davon verdienen jedoch eine weitere Erläuterung.

### Codesniffer-Direktiven

Zeilen 16 bis 18 haben dies:

```php
// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects
```

Die kommentierten Zeilen vermeiden Fehlermeldungen vom CodeSniffer. Sie dienen keinen anderen Zwecken.

### öffentliche Funktion beimInhaltVorbereiten()

Dies ist das Ereignis, auf das das Plugin reagiert. Es wird ein *Kontext*, die Beitragsdaten und die Beitragsparameter übergeben. Ein *Inhaltsverzeichnis* sollte nur generiert werden, wenn die gerenderte Seite ein einzelner Beitrag ist. Andernfalls muss der Platzhalter `{cefjdemostoc}` entfernt werden.

Um ein Inhaltsverzeichnis zu erstellen, werden einige Beitragsparameter festgelegt und jede Überschrift im Beitrag wird geändert, um eine Sequenznummer **id** hinzuzufügen, sodass `<h2>Introduction</h2>` zu `<h2 id="cefjemostoc0">Introduction</h2>` wird. Anschließend wird eine Vorlage geladen, um das gerenderte Inhaltsverzeichnis hinzuzufügen.

### Die Datei tmpl/default.php

Der Zweck dieser Datei besteht darin, die Ausgabe mit den bereitgestellten Daten darzustellen. Die Datei kann überschrieben werden und Sie werden sie unter den **plugins/contents** Überschreibungen für das Cassiopeia-Template sehen.

Die Datei wird unten reproduziert. Beachten Sie, dass einige Elemente fest codierte CSS-Klassen haben und einige aus den Beitragsparametern stammen. Die **fs-** Klassen sind Bootstrap-Schriftgrößen. Die Überschriften sind im `$headings2` Array gespeichert.

```php
<?php

/**
 * @package     Cefjdemostoc.Plugin
 * @subpackage  Content.cefjemostoc
 *
 * @copyright   (C) 2024 Clifford E Ford.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

defined('_JEXEC') or die;

use Joomla\CMS\Language\Text;

/**
 * @var Joomla\CMS\WebAsset\WebAssetManager $wa
 * @var \Joomla\Plugin\Content\Cefjdemostoc\Extension\Cefjdemostoc $this
 */
$wa = $this->getApplication()->getDocument()->getWebAssetManager();
$wa->registerAndUseStyle('plg_content_cefjdemostoc', 'plg_content_cefjdemostoc/cefjdemostoc.css');

?>
<div class="card cefjdemostoc <?php echo $toc_class; ?>">
    <div class="card-header">
        <div class="<?php echo $toc_head_class; ?> fs-2">
            <?php echo Text::_('PLG_CONTENT_CEFJDEMOSTOC_CONTENTS'); ?>
        </div>
    </div>
    <div class="card-body">
        <ul class="list-group">
            <?php foreach ($headings2 as $i => $heading) : ?>
                <li class="list-group-item fs-<?php echo $heading[1] + 2; ?> ps-<?php echo $heading[1] + 2; ?>">
                    <a href="#cefjemostoc<?php echo $i; ?>" class="link-underline-light">
                        <?php echo $heading[2]; ?>
                    </a>
                </li>
            <?php endforeach; ?>
        </ul>
    </div>
</div>
```

#### Der Web Asset Manager

Ohne zusätzliche Gestaltung würde das Inhaltsverzeichnis (ToC) an der Stelle des `{cefjemostoc}`-Platzhalters im Quelltext die volle Breite des Bildschirms einnehmen. Das wird manchmal in technischen Veröffentlichungen gesehen, ist jedoch möglicherweise nicht das, was Sie möchten. Auf breiten Bildschirmen ist es oft besser, das ToC schweben zu lassen und den Beitragstext drumherum fließen zu lassen. Allerdings kann das auf schmalen Bildschirmen unbrauchbar werden. Die beste Lösung ist, ein Stylesheet mit benutzerdefinierten Media Queries bereitzustellen. Auf breiten Bildschirmen wird das ToC nach rechts verschoben, auf schmalen Bildschirmen jedoch nicht.

Der obige `default.php` Code zeigt, wie man ein benutzerdefiniertes Stylesheet für das Plugin einbindet. Das CSS befindet sich in der Datei `media/css/cefjdemostoc.css`. Die dortigen Werte dienen zur Veranschaulichung dieses Tutorials.

## Ergebnis

![Das resultierende Inhaltsverzeichnis](../../../en/images/plugins/cefjdemostoc-result.png)

*Übersetzt von openai.com*


<!-- Filename: J4.x:CLI_example_-_Onoffbydate / Display title: CLI-Beispiel - Onoffbydate -->

## Einführung

Es gibt Gelegenheiten, bei denen eine Website ein Modul je nach Datum anzeigen oder verbergen muss. Ein Beispiel könnte ein benutzerdefiniertes HTML-Modul sein, das im Winter eine Nachricht anzeigt. Ein weiteres Beispiel könnte der Wechsel von benutzerdefinierten Modulen je nach Wochentag sein, zum Beispiel eines für Werktage und ein anderes für Wochenenden. Das hier vorgestellte Beispiel verwendet ein Plugin, einen CLI-Befehl und einen Cron, um den gewünschten Effekt zu erzielen.

Der Code ist verfügbar auf [GitHub](https://github.com/ceford/cefjdemos-plg-onoffbydate).

Es gibt weitere Beiträge zur Nutzung der CLI im [Benutzerhandbuch](http://localhost/jdm4/jdocmanual?article=user/command-line-interface/using-the-cli) und in der Joomla! Programmierer-Dokumentation [lokale Kopie](jdocmanual?article=docus/plugins/plugin-examples-console-plugin-sqlfile) oder [Originalquelle](https://manual.joomla.org/docs/building-extensions/plugins/plugin-examples/console-plugin-sqlfile/);

## Joomla-Standards

In früheren Versionen von Joomla verwendete das Pluginsystem eine Implementierung des Beobachtbar/Beobachter-Musters. Infolgedessen wurden bei jedem geladenen Plugin sofort alle öffentlichen Methoden als Beobachter registriert. Dies konnte zu Problemen führen.

Joomla 4 und später verwendet das Joomla Framework Event-Paket, um Plugin-Ereignisse zu verwalten. Dies sorgt für bessere Leistung und Sicherheit. In praktischen Begriffen bedeutet dies, dass die Dateistruktur eines Joomla 4/5/6-Plugins sich von der Legacy-Plugin-Struktur früherer Versionen unterscheidet.

## Die Plugin-Struktur

Das Plugin heißt **onoffbydate**. Das folgende schematische Diagramm zeigt die Dateistruktur, die für die lokale Entwicklung des Plugins verwendet wird:

```
cefjdemos-plg-onoffbydate
|-- .vscode (folder for build settings)
|-- cefjdemos-plg-onoffbydate (folder compressed to create an installable zip file)
    |-- language
        |-- en-GB (language folder, kept with the extension code)
            |-- plg_system_onoffbydate.ini
            |-- plg_system_onoffbydate.sys.ini
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Console (folder for extension specific code)
            |-- OnoffbydateCommand.php (the plugin execution code)
        |-- Extension
            |-- Onoffbydate.php (the plugin register code)
    |-- onoffbydate.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- changelog.xml (a record of changes in each release)
|-- README.md (brief description and instructions on use)
|-- updates.xml (the update server specification)
```

## Die Manifestdatei onoffbydate.xml

Dies ist die Installationsdatei - ein Standardpunkt für jede Erweiterung.

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="plugin" group="console" method="upgrade">
    <name>plg_console_onoffbydate</name>
    <author>Clifford E Ford</author>
    <creationDate>Jamuary 2025</creationDate>
    <copyright>(C) Clifford E Ford</copyright>
    <license>GNU General Public License version 3 or later</license>
    <authorEmail>cliff@xxx.yyy.co.uk</authorEmail>
    <version>0.3.0</version>
    <description>PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Plugin\Console\Onoffbydate</namespace>
    <files>
        <folder>language</folder>
        <folder plugin="onoffbydate">services</folder>
        <folder>src</folder>
    </files>
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-plg-onoffbydate/main/changelog.xml</changelogurl>
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" priority="2" name="Cefjdemosonoffbydate Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-plg-onoffbydate/main/updates.xml</server>
    </updateservers>
    <config>
        <fields>

        </fields>
    </config>
</extension>
```

- Die **namespace**-Zeile teilt Joomla mit, wo der namensraumbezogene Code für dieses Plugin zu finden ist.
- Der **language**-Ordner wird zusammen mit dem Plugin-Code aufbewahrt.
- Ein **updateserver** ist erforderlich, um die Anforderungen des JED zu erfüllen.
- Es gibt keine **config**-Optionen für dieses Plugin, aber das könnte sich in Zukunft ändern. Zum Beispiel könnte der Benutzer es vorziehen, die Wintermonate mit Kontrollkästchen festzulegen, anstatt sie fest zu kodieren.

## Registrierung: dienste/anbieter.php

Dies ist der Einstiegspunkt für den Plugin-Code.

```php
<?php

/**
 * @package     Cefjdemos.Plugin
 * @subpackage  Console.Onoffbydate
 *
 * @copyright   Copyright (C) 2025 Clifford E Ford.
 * @license     GNU General Public License version 3 or later.
 */

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\Extension\PluginInterface;
use Joomla\CMS\Plugin\PluginHelper;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;
use Joomla\CMS\Factory;
use Joomla\Event\DispatcherInterface;
use Cefjdemos\Plugin\Console\Onoffbydate\Extension\Onoffbydate;

return new class implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.2.0
     */
    public function register(Container $container)
    {
        $container->set(
            PluginInterface::class,
            function (Container $container) {
                $dispatcher = $container->get(DispatcherInterface::class);
                $plugin     = new Onoffbydate(
                    $dispatcher,
                    (array) PluginHelper::getPlugin('console', 'onoffbydate')
                );
                $plugin->setApplication(Factory::getApplication());

                return $plugin;
            }
        );
    }
};
```

## Ereignisabonnement: src/Extension/Onoffbydate.php

Dies ist der Ort, an dem das Ereignis, das das Plugin auslöst, registriert wird und der Ort, an dem der Code, der das Ereignis behandelt, festgelegt wird.

```php
<?php

/**
 * @package     Cefjdemos.Plugin
 * @subpackage  Console.Onoffbydate
 *
 * @copyright   Copyright (C) 2025 Clifford E Ford.
 * @license     GNU General Public License version 3 or later.
 */

namespace Cefjdemos\Plugin\Console\Onoffbydate\Extension;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\Plugin\CMSPlugin;
use Joomla\Event\SubscriberInterface;
use Cefjdemos\Plugin\Console\Onoffbydate\Console\OnoffbydateCommand;

final class Onoffbydate extends CMSPlugin implements SubscriberInterface
{
    /**
     * Returns the event this subscriber will listen to.
     *
     * @return  array
     */
    public static function getSubscribedEvents(): array
    {
        return [
                \Joomla\Application\ApplicationEvents::BEFORE_EXECUTE => 'registerCommands',
        ];
    }

    /**
     * Returns the command class for the Onoffbydate CLI plugin.
     *
     * @return  void
     */
    public function registerCommands(): void
    {
        $myCommand = new OnoffbydateCommand();
        $myCommand->setParams($this->params);
        $this->getApplication()->addCommand($myCommand);
    }
}
```

Die **OnoffbydateCommand**-Klasse befindet sich im *src/Console*-Ordner, da die eingebauten Joomla-Konsolenbefehle in einem Console-Ordner untergebracht sind. Sie hätte auch in einem *Extension*-Ordner platziert werden können.

## Die Befehlsdatei: src/Console/OnoffbydateCommand.php

Diese Datei enthält den Code, der die gesamte Arbeit für diese Erweiterung erledigt.

```php
<?php

/**
 * @package     Cefjdemos.Plugin
 * @subpackage  Console.Onoffbydate
 *
 * @copyright   Copyright (C) 2025 Clifford E Ford.
 * @license     GNU General Public License version 3 or later.
 */

namespace Cefjdemos\Plugin\Console\Onoffbydate\Console;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\Factory;
use Joomla\CMS\Language\Text;
use Joomla\Console\Command\AbstractCommand;
use Joomla\Database\DatabaseInterface;
use Symfony\Component\Console\Input\InputArgument;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;
use Symfony\Component\Console\Style\SymfonyStyle;
use Joomla\Database\DatabaseAwareTrait;

class OnoffbydateCommand extends AbstractCommand
{
    use DatabaseAwareTrait;

    /**
     * The default command name
     *
     * @var    string
     *
     * @since  4.0.0
     */
    protected static $defaultName = 'onoffbydate:action';

    /**
     * @var InputInterface
     * @since version
     */
    private $cliInput;

    /**
     * SymfonyStyle Object
     * @var SymfonyStyle
     * @since 4.0.0
     */
    private $ioStyle;

    /**
     * The params associated with the plugin, plus getter and setter
     * These are injected into this class by the plugin instance
     */
    protected $params;

    protected function getParams()
    {
        return $this->params;
    }

    public function setParams($params)
    {
        $this->params = $params;
    }

    /**
     * Initialise the command.
     *
     * @return  void
     *
     * @since   4.0.0
     */
    protected function configure(): void
    {
        $lang = Factory::getApplication()->getLanguage();
        $test = $lang->load(
            'plg_console_onoffbydate',
            JPATH_BASE . '/plugins/console/onoffbydate'
        );

        $this->addArgument(
            'season',
            InputArgument::REQUIRED,
            'winter or weekend'
        );
        $this->addArgument(
            'action',
            InputArgument::REQUIRED,
            'publish or unpublish'
        );

        $this->addArgument(
            'module_id',
            InputArgument::REQUIRED,
            'module id'
        );

        $help = Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_1');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_2');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_3');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_4');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_5');

        $this->setDescription(Text::_('PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION'));
        $this->setHelp($help);
    }

    /**
     * Internal function to execute the command.
     *
     * @param   InputInterface   $input   The input to inject into the command.
     * @param   OutputInterface  $output  The output to inject into the command.
     *
     * @return  integer  The command exit code
     *
     * @since   4.0.0
     */
    protected function doExecute(InputInterface $input, OutputInterface $output): int
    {
        $this->configureIO($input, $output);

        $season = $this->cliInput->getArgument('season');
        $action = $this->cliInput->getArgument('action');
        $module_id = $this->cliInput->getArgument('module_id');

        switch ($season) {
            case 'winter':
                $result = $this->winter($module_id, $action);
                break;
            case 'weekend':
                $result = $this->weekend($module_id, $action);
                break;
            default:
                $result = Text::_('PLG_CONSOLE_ONOFFBYDATE_ERROR', $season);
                $this->ioStyle->error($result);
                return 0;
        }

        return 1;
    }

    protected function publish($module_id, $published)
    {
        $db = Factory::getContainer()->get(DatabaseInterface::class);
        //$db    = $this->getDatabase();
        $query = $db->getQuery(true);
        $query->update('#__modules')
            ->set('published = ' . $published)
            ->where('id = ' . $module_id);
        $db->setQuery($query);
        $db->execute();
    }

    protected function weekend($module_id, $action)
    {
        // get the day of the week
        $day = date('w');
        if (in_array($day, array(0,6))) {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_A_WEEKEND');
            $published = $action === 'publish' ? 1 : 0;
        } else {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_A_WEEKEND');
            $published = $action === 'publish' ? 0 : 1;
        }

        $this->publish($module_id, $published);

        $state = empty($published) ? Text::_('JUNPUBLISHED') : Text::_('JPUBLISHED');
        $result = Text::sprintf('PLG_CONSOLE_ONOFFBYDATE_SUCCESS', $msg, $module_id, $state);

        $this->ioStyle->success($result);
    }

    protected function winter($module_id, $action)
    {
        // get the month of the month
        $month = date('n');
        if (in_array($month, array(1,2,11,12))) {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_WINTER');
            $published = $action === 'publish' ? 1 : 0;
        } else {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_WINTER');
            $published = $action === 'publish' ? 0 : 1;
        }

        $this->publish($module_id, $published);

        $state = empty($published) ? Text::_('JUNPUBLISHED') : Text::_('JPUBLISHED');

        $state = empty($published) ? Text::_('JUNPUBLISHED') : Text::_('JPUBLISHED');
        $result = Text::sprintf('PLG_CONSOLE_ONOFFBYDATE_SUCCESS', $msg, $module_id, $state);

        $this->ioStyle->success($result);
    }

    /**
     * Configures the IO
     *
     * @param   InputInterface   $input   Console Input
     * @param   OutputInterface  $output  Console Output
     *
     * @return void
     *
     * @since 4.0.0
     *
     */
    private function configureIO(InputInterface $input, OutputInterface $output)
    {
        $this->cliInput = $input;
        $this->ioStyle = new SymfonyStyle($input, $output);
    }
}
```

- Die Funktion `configure` stellt fest, dass drei Befehlszeilenargumente erforderlich sind:
  - `season` muss entweder `weekend` oder `winter` sein.
  - `action` muss entweder `publish` oder `unpublish` sein.
  - `module_id` muss die ganzzahlige ID des Moduls sein, das veröffentlicht oder nicht veröffentlicht werden soll.
- Die Funktion `doExecute` ist der Ort, an dem die Arbeit erledigt wird. Zwei Aktionsoptionen werden erkannt:
  - `winter` ruft eine Funktion auf, um ein Modul zu veröffentlichen oder nicht zu veröffentlichen, wenn heute in den Wintermonaten liegt.
  - `weekend` ruft eine Funktion auf, um ein Modul zu veröffentlichen oder nicht zu veröffentlichen, wenn heute ein Wochenendtag ist.
- Die Funktion `publish` führt einen Datenbankaufruf durch, um das Feld `publishes` des Moduls auf `0` oder `1` zu setzen, wie es angemessen ist.
- Die Funktionen `winter` und `weekend` führen einige Datumsberechnungen durch, um zu bestimmen, ob heute Winter ist (oder nicht) oder ein Wochenende (oder nicht), um entsprechende Parameter an die publish-Funktion zu übergeben.
- Der Funktionsaufruf `$this->ioStyle->success()` erzeugt eine Konsolentextnachricht mit schwarzem Text auf grünem Hintergrund. `$this->ioStyle->error()` erzeugt eine FEHLER-Nachricht mit weißem Text auf weinrotem Hintergrund.

## Sprachdateien

Es gibt zwei Sprachdateien, die während der Installation und Plugin-Konfiguration verwendet werden. Die Datei en-GB/plg_console_onoffbydate.sys.ini enthält die während der Installation und Verwaltung sichtbaren Zeichenketten:

```ini
PLG_CONSOLE_ONOFFBYDATE="Console - Onoffbydate"
PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION="Set date-dependent enabled state of a module via cli"
```

Die Datei en-GB/plg_console_onoffbydate.ini enthält die im Einsatz befindlichen Zeichenfolgen:

```ini
PLG_CONSOLE_ONOFFBYDATE="Console - Onoffbydate"
PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION="Set date-dependent enabled state of a module via cli"
PLG_CONSOLE_ONOFFBYDATE_ERROR="Unknown season: %s."
PLG_CONSOLE_ONOFFBYDATE_HELP_1="<info>%command.name%</info> Toggles module Enabled/Disabled state\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_2="Usage: <info>php %command.full_name% season action module_id where\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_3="    season is one of winter or weekend,\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_4="    action is one of publish or unpublish and\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_5="    module_id is the id of the module to publish or unpublish.</info>\n"
PLG_CONSOLE_ONOFFBYDATE_SUCCESS="That seemed to work. %s Module %s has been %s."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_A_WEEKEND="Today is in a weekend."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_WINTER="Today is in winter."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_A_WEEKEND="Today is not in a weekend."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_WINTER="Today is not in winter."
```

Beachten Sie, dass die Befehlsausgabe nur von Ihnen auf Ihrer Joomla-Installation gesehen wird.

## Das Modul

Dieser Code ändert einfach den Veröffentlichungsstatus eines Moduls in Abhängigkeit von einer Funktion des Datums. Daher benötigen Sie die Modul-ID, wie sie in der rechten Spalte der Modul-Liste (Site) Seite dargestellt ist.

## Die Befehlszeile

Installieren Sie das Plugin in Ihrer Joomla-Testinstallation und aktivieren Sie es. Wenn es Ihre Website abstürzt, verwenden Sie phpMyAdmin, um das neu installierte Plugin in der #__extensions-Tabelle zu finden und setzen Sie seinen enabled-Parameter auf 0. Beheben Sie das Problem und versuchen Sie es erneut.

In einem Terminalfenster wechseln Sie das Verzeichnis zum Stammverzeichnis Ihrer Website und verwenden Sie den folgenden Befehl:

```sh
php cli/joomla.php list
```

Wenn in diesem Stadium etwas schiefgeht, überprüfen Sie, ob die aufgerufene PHP-Version die Befehlszeilenversion ist und nicht die, die vom Webserver verwendet wird. Sie können Deprecation-Warnungen mit dieser Version der Befehlszeile unterdrücken.

```sh
php -d error_reporting="E_ALL & ~E_DEPRECATED" cli/joomla.php onoffbydate:action garbage publish 133
```

Wenn der Code funktioniert, sehen Sie `onoffbydate` in der Liste der verfügbaren Befehle und Sie können Hilfe aufrufen, um zu sehen, wie es verwendet werden sollte:

```sh
php cli/joomla.php onoffbydate:action --help
Usage:
  onoffbydate:action <season> <action> <module_id>

Arguments:
  season                       winter or summer or weekday or weekend
  action                       publish or unpublish
  module_id                    module id

Options:
  -h, --help                   Display the help information
  -q, --quiet                  Flag indicating that all output should be silenced
  -V, --version                Displays the application version
      --ansi                   Force ANSI output
      --no-ansi                Disable ANSI output
  -n, --no-interaction         Flag to disable interacting with the user
      --live-site[=LIVE-SITE]  The URL to your site, e.g. https://www.example.com
  -v|vv|vvv, --verbose         Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Help:
  onoffbydate:action Toggles module Enabled/Disabled state
  Usage: php joomla.php onoffbydate:action season action module_id where
      season is one of winter or summer or weekday or weekend,
      action is one of publish or unpublish and
      module_id is the id of the module to publish or unpublish.
```

Und dann probieren Sie es einfach aus:

```sh
php cli/joomla.php onoffbydate:action weekend publish 133

     [OK] That seemed to work. Today is not a weekend. Module 133 has been Unpublished
```

### In Verwendung

Die Verwendung dieser Logik ist ein wenig unlogisch! Wenn Sie ein Modul haben, das nur am Wochenende veröffentlicht werden soll, lauten die Parameter zur täglichen Verwendung `weekend publish module_id`. Das veröffentlicht das Modul an Tagen eines Wochenendes und hebt die Veröffentlichung an Tagen auf, die nicht am Wochenende sind. Für ein Modul, das an Werktagen veröffentlicht werden muss, lauten die Parameter zur täglichen Verwendung `weekend unpublish module_id`. Das hebt die Veröffentlichung des Moduls an Wochenendtagen auf und veröffentlicht es an Tagen, die nicht am Wochenende sind. Die gleiche Logik gilt für die `winter` Version.

## Der Cron

Der Befehl kann in einem Terminalfenster getestet werden, aber Sie möchten ihn wahrscheinlich von einem Cron aus verwenden. Die `winter`-Option könnte am ersten Tag jedes Monats ausgeführt werden. Die `weekend`-Option würde täglich ausgeführt. Der wichtige Punkt ist, dass Sie so viele Crons haben können, wie Sie benötigen, um den Veröffentlichungsstatus von so vielen Modulen zu ändern, wie Sie möchten, und das in beliebigen passenden Intervallen. Der gleiche Code funktioniert für alle.

Auf einem Hosting-Service müssen Sie die vollständigen Pfade zur PHP-Executable und zum Joomla-CLI-Befehl angeben. Beispiel:

```sh
/usr/local/bin/php /home/username/public_html/pathtojoomla/cli/joomla.php onoffbydate:action winter publish 130
```

Je nachdem, wie Sie Ihren Cron-Job und Ihr System eingerichtet haben, können Sie eine beruhigende E-Mail erhalten, die genau die gleichen Informationen enthält, die Sie in der Befehlszeile sehen.

## Überprüfen

Und natürlich: Gehe zu deiner Startseite und überprüfe, ob das Modul wirklich veröffentlicht oder nicht veröffentlicht wurde.

*Übersetzt von openai.com*

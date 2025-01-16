<!-- Filename: J4.x:Dependency_Injection_in_Joomla_4 / Display title: Abhängigkeitsinjektion -->

## Einführung

Abhängigkeitsinjektion (DI) ist eine Softwareentwicklungstechnik, die einer Klasse ihre Abhängigkeiten von außen bereitstellt, anstatt sie intern zu erstellen. Sie verbessert die Flexibilität, Wartbarkeit und Testbarkeit des Codes. Dennoch ist sie schwer zu verstehen!

Es gibt eine Erklärung von DI und Dependency Injection Containers in der [Joomla Framework DI](https://github.com/joomla-framework/di/blob/4.x-dev/docs/why-dependency-injection.md) Dokumentation. Es gibt auch eine [Übersicht](https://github.com/joomla-framework/di/blob/4.x-dev/docs/overview.md) über deren Verwendung.

Die Erklärungen im Abschnitt [Dependency Injection](jdocmanual?article=docus/dependency-injection/index) der Joomla! Programmierer-Dokumentation sind nicht so leicht zu verstehen!

## Beispiele

Wenn Ihr Ziel darin besteht, mit der Erstellung einer Erweiterung zu beginnen, können die folgenden Beispiele aus den Joomla-Kernbeiträgen hilfreich sein.

Der Code für die Dependency Injection befindet sich in services/provider.php, aber der genaue Speicherort dieser Datei hängt von der Erweiterungstyp ab.

### Komponentenbeispiel

Der vollständige Pfad zur Datei services/provider.php ist administrator/components/com_example/services/provider.php. Dieses Beispiel ist com_tags, das keine Kategorien verwendet. Wenn Ihre Komponente Kategorien verwendet, werfen Sie einen Blick auf den Code von services/provider.php für com_content. Sie benötigen mehrere Kategorien-bezogene Anweisungen.

```php
<?php

/**
 * @package     Joomla.Administrator
 * @subpackage  com_tags
 *
 * @copyright   (C) 2018 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Component\Router\RouterFactoryInterface;
use Joomla\CMS\Dispatcher\ComponentDispatcherFactoryInterface;
use Joomla\CMS\Extension\ComponentInterface;
use Joomla\CMS\Extension\Service\Provider\ComponentDispatcherFactory;
use Joomla\CMS\Extension\Service\Provider\MVCFactory;
use Joomla\CMS\Extension\Service\Provider\RouterFactory;
use Joomla\CMS\MVC\Factory\MVCFactoryInterface;
use Joomla\Component\Tags\Administrator\Extension\TagsComponent;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;

/**
 * The tags service provider.
 *
 * @since  4.0.0
 */
return new class () implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.0.0
     */
    public function register(Container $container)
    {
        $container->registerServiceProvider(new MVCFactory('\\Joomla\\Component\\Tags'));
        $container->registerServiceProvider(new ComponentDispatcherFactory('\\Joomla\\Component\\Tags'));
        $container->registerServiceProvider(new RouterFactory('\\Joomla\\Component\\Tags'));
        $container->set(
            ComponentInterface::class,
            function (Container $container) {
                $component = new TagsComponent($container->get(ComponentDispatcherFactoryInterface::class));
                $component->setMVCFactory($container->get(MVCFactoryInterface::class));
                $component->setRouterFactory($container->get(RouterFactoryInterface::class));

                return $component;
            }
        );
    }
};
```

### Modulbeispiel

Dies ist das Versionsmodul, das die Joomla-Version in der Titelleiste der Administratorseiten anzeigt. Der Code befindet sich in administrator/modules/mod_version/services/provider.php.

```php
<?php

/**
 * @package     Joomla.Administrator
 * @subpackage  mod_version
 *
 * @copyright   (C) 2024 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Extension\Service\Provider\HelperFactory;
use Joomla\CMS\Extension\Service\Provider\Module;
use Joomla\CMS\Extension\Service\Provider\ModuleDispatcherFactory;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;

return new class () implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   5.1.0
     */
    public function register(Container $container)
    {
        $container->registerServiceProvider(new ModuleDispatcherFactory('\\Joomla\\Module\\Version'));
        $container->registerServiceProvider(new HelperFactory('\\Joomla\\Module\\Version\\Administrator\\Helper'));

        $container->registerServiceProvider(new Module());
    }
};
```

### Plugin-Beispiel

Dies ist das Plugin, das Kontaktinformationen auf Inhaltsseiten bereitstellt. Der Code befindet sich in plugins/content/contact/services/provider.php.

```php
<?php

/**
 * @package     Joomla.Plugin
 * @subpackage  Content.contact
 *
 * @copyright   (C) 2022 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Extension\PluginInterface;
use Joomla\CMS\Factory;
use Joomla\CMS\Plugin\PluginHelper;
use Joomla\Database\DatabaseInterface;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;
use Joomla\Event\DispatcherInterface;
use Joomla\Plugin\Content\Contact\Extension\Contact;

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
                $plugin     = new Contact(
                    $container->get(DispatcherInterface::class),
                    (array) PluginHelper::getPlugin('content', 'contact')
                );
                $plugin->setApplication(Factory::getApplication());
                $plugin->setDatabase($container->get(DatabaseInterface::class));

                return $plugin;
            }
        );
    }
};
```

*Übersetzt von openai.com*

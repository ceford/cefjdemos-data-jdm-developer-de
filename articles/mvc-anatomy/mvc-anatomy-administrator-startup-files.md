<!-- Filename: J4.x:MVC_Anatomy:_Administrator_Startup_Files / Display title: MVC Anatomie: Administrator-Startdateien -->

## Übersicht der Administrator-Dateien

Es gibt mehr Dateien im Administrator-Teil der Komponente, teilweise weil es zwei Tabellen gibt, jede mit einer Listen- und Bearbeitungsansicht, und teilweise weil einige der Dateien Aspekte des Verhaltens der Komponente sowohl in der Seite als auch in den Administratoroberflächen steuern.

Die Erstellung von Code für eine Listenansicht wurde im Teil des Tutorials behandelt, der sich mit den Site-Dateien befasst, und wird hier nicht wiederholt. Dieser Abschnitt befasst sich mit den Dateien, die den meisten Komponenten gemeinsam sind. Ein nachfolgendes Tutorial wird eine der Bearbeitungsansichten abdecken, die zum Aktualisieren oder Hinzufügen neuer Datensätze erforderlich sind.

## dienste/anbieter.php

Diese Datei wird verwendet, wenn die Joomla-Anwendung den ComponentHelper aufruft, um die Komponente zu rendern. Es ist der allererste Komponenten-Code, der sowohl für die Website als auch für den Administrator ausgeführt wird. Seine Rolle ist es, die Komponente zu registrieren:

```php
    public function register(Container $container)
    {
        $container->registerServiceProvider(new MVCFactory('\\Cefjdemos\\Component\\Countrybase'));
        $container->registerServiceProvider(new ComponentDispatcherFactory('\\Cefjdemos\\Component\\Countrybase'));
        $container->registerServiceProvider(new RouterFactory('\\Cefjdemos\\Component\\Countrybase'));
        $container->set(
            ComponentInterface::class,
            function (Container $container) {
                $component = new CountrybaseComponent($container->get(ComponentDispatcherFactoryInterface::class));
                $component->setMVCFactory($container->get(MVCFactoryInterface::class));
                $component->setRouterFactory($container->get(RouterFactoryInterface::class));

                return $component;
            }
        );
    }
```

## src/Erweiterung/LänderbasisKomponente.php

Die Registerfunktion dieser Datei wird aus der Anweisung \$component = new CountrybaseComponent(...); in services/provider.php aufgerufen. Ihr Zweck ist es, die Komponente sowohl in der Site- als auch in der Administrator-Schnittstelle zu starten.

```php
       public function boot(ContainerInterface $container)
        {
        }
```

## access.xml

Diese Datei legt die Zugriffskontrollen fest, die in dieser Komponente verwendet werden. Die hier aufgeführten Elemente erscheinen im Register Optionen Berechtigungen der Komponente.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<access component="com_countrybase">
    <section name="component">
        <action name="core.admin" title="JACTION_ADMIN" />
        <action name="core.options" title="JACTION_OPTIONS" />
        <action name="core.manage" title="JACTION_MANAGE" />
        <action name="core.create" title="JACTION_CREATE" />
        <action name="core.delete" title="JACTION_DELETE" />
        <action name="core.edit" title="JACTION_EDIT" />
    </section>
</access>
```

## config.xml

Diese Datei wird verwendet, um zu steuern, ob im Komponenten-Formular *Optionen* Optionen angezeigt werden und unter welchem Tab sie erscheinen. Für com_countrybase gibt es keine Optionen außer den Standardberechtigungsoptionen von Joomla. Es wäre möglich, hier Optionen hinzuzufügen, zum Beispiel ob eine bestimmte Spalte in der Ausgabedarstellung angezeigt oder verborgen werden soll. Dies würde in ein separates Feldset namens Optionen gehen.

```xml
<?xml version="1.0" encoding="utf-8"?>
<config>

    <fieldset
        name="permissions"
        label="JCONFIG_PERMISSIONS_LABEL"
        description="JCONFIG_PERMISSIONS_DESC"
        >

        <field
            name="rules"
            type="rules"
            label="JCONFIG_PERMISSIONS_LABEL"
            filter="rules"
            validate="rules"
            component="com_countrybase"
            section="component"
         />
    </fieldset>
</config>
```

*Übersetzt von openai.com*


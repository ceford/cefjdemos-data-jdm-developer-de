<!-- Filename: J4.x:Triggering_content_plugins_in_your_extension / Display title: Inhaltsplugins in eigenen Erweiterungen auslösen -->

Joomla!  4.x

Ein gängiges Beispiel für die [Verwendung von
Plugins](https://docs.joomla.org/Supporting_plugins_in_your_component "Special:MyLanguage/Supporting plugins in your component")
ist die Ausführung der **Inhaltsplugins** für irgendeinen Text. Dies ist
nützlich, wenn Plugins unterstützt werden sollen, die normalerweise mit
Inhalten aus einer benutzerdefinierten Erweiterung arbeiten. Für den
Trigger *content prepare* kann beispielsweise folgender Code verwendet
werden:

```php
    $text = \Joomla\CMS\HTML\HTMLHelper::_('content.prepare', $text);
```

Für alle anderen Content-Trigger kann folgendes verwendet werden:

```php
    // Diese 3 Zeilen stehen am Anfang der Datei direkt unter der JEXEC-Anweisung
    use Joomla\CMS\CMSObject;
    use Joomla\CMS\Factory;
    use Joomla\CMS\Plugin\PluginHelper;

    // Dieser Code geht in Methode der Komponente, die die Plugins auslöst
    $article = new \stdClass;
    $article->text = $text;

    // Falls nötig, mehr Parameter hinzufügen
    $params = new CMSObject;

    PluginHelper::importPlugin('content');
    $app = Factory::getApplication();
    $app->triggerEvent('onContentPrepare', array('some.context', &$article, &$params, 0));
```

Beispiele können in Kernkomponenten (z. B. com_content) gefunden werden.
Siehe die Seite
[Trigger](https://docs.joomla.org/Plugin/Events "Special:MyLanguage/Plugin/Events")
für die möglichen Inhalts-Plugin-Trigger.

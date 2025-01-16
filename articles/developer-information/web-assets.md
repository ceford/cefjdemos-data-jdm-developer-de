<!-- Filename: J4.x:Web_Assets / Display title: Web-Assets -->

## Über Web-Assets

Es gibt eine vollständige Beschreibung von Web-Ressourcen in der Joomla! Programmierer-Dokumentation, die auf [dieser Seite](jdocmanual?article=docus/web-asset-manager/index) wiedergegeben oder aus der [originalen Quelle](https://manual.joomla.org/docs/general-concepts/web-asset-manager/) verfügbar ist.

## Zusätzliche Beispiele

In Jdocmanual enthalten viele der Beiträge Codeblöcke, die von Syntaxhervorhebung profitieren. So wird es erreicht:

```php
/** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
$wa = $this->document->getWebAssetManager();

// Register and attach a custom item in one run
$wa->registerAndUseStyle('highlight', 'https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/styles/default.min.css', [], [], []);

// Register and attach a custom item in one run
$wa->registerAndUseScript('highlight','https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/highlight.min.js', [], [], ['core']);

// Add an inline content as usual, will be rendered in flow after all assets
$wa->addInlineScript('hljs.highlightAll();');
```

Dieser Code befindet sich in der Datei `default.php`, die verwendet wird, um diese Seite anzuzeigen! Es ist etwas knifflig zu implementieren, da der Aufruf von `hljs.highlightAll()` gemacht werden muss, nachdem die `css`- und `js`-Dateien geladen wurden und erneut, wann immer der Seiteninhalt mit einem JavaScript-Aufruf geändert wird.

Andere Syntax-Highlighter sind verfügbar!

*Übersetzt von openai.com*


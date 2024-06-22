<!-- Filename: J4.x:Web_Assets / Display title: Web-Assets -->

## Konzept

In der Frontend-Welt hängen viele Assets zusammen. Zum Beispiel hängt
unser Keepalive-Skript von der Datei core.js für die Optionsverwaltung
ab. In Joomla gab es nie eine einfache Möglichkeit, dies zu
spezifizieren, man musste mehrere Dateien einfügen. Joomla 4 ändert dies
mit dem Konzept der Web-Assets.

## Definition

Verwandte Assets werden in einer JSON-Datei wie z. B. <a
href="https://github.com/joomla/joomla-cms/blob/7b72c565b610e02c1b01f8958d622879631fa6a2/build/media_source/system/joomla.asset.json#L14-L21"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">system/joomla.asset.json#L14-L21</a>
definiert.

Diese haben eine Struktur mit einer Schema-Definition (zur Validierung),
Name, Version, Lizenz und einer oder mehreren Asset-Definitionen. Assets
bestehen aus einer Liste von js-Dateien und css-Dateien, die sich auf
die Assets und deren Abhängigkeiten beziehen. Der Abschnitt
Abhängigkeiten ist nur eine Liste von Asset-Namen, die für die Funktion
der Anlage erforderlich sind. Beispiel:Beispiel:

```json
    {
      "$schema": "https://developer.joomla.org/schemas/json-schema/web_assets.json",
      "name": "com_example",
      "version": "4.0.0",
      "description": "Joomla CMS",
      "license": "GPL-2.0+",
      "assets": [
        {
          "name": "bar",
          "type": "style",
          "uri": "com_example/bar.css"
        },
        {
          "name": "bar",
          "type": "script",
          "uri": "com_example/bar.js"
        },
        {
          "name": "beer",
          "type": "style",
          "uri": "com_example/beer.css",
          "dependencies": [
            "bar"
          ],
        },
        {
          "name": "beer",
          "type": "script",
          "dependencies": [
            "core",
            "bar"
          ],
          "uri": "com_example/beer.js",
          "attributes": {
            "defer": true,
            "data-foo": "bar"
          }
        }
      ]
    }
```

Das Attribut `$schema` dient zur Schemadefinition, mit der eine eigene
Datei mittels JSON-Schema überprüft werden kann. Weitere Informationen
zur Funktionsweise der JSON-Schema-Validierung stehen auf
<a href="https://json-schema.org/understanding-json-schema/index.html"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">der offiziellen Website</a> zur
Verfügung.

**Hinweis:** Das Bereitstellen von joomla.asset.json für eine
Erweiterung oder Template ist sinnvoll, aber nicht erforderlich, damit
WebAssset funktioniert (siehe nächster Abschnitt).

**Hinweis:** Es wird nicht empfohlen, ein Inline-Asset zu einer
json-Datei hinzuzufügen, besser sollte man dafür eine Datei verwenden.

## Beschreiben der Asset-Stufen

Jedes Asset hat 2 Stufen: *registriert* und *in Verwendung*.

**Registriert** (registered) bedeutet, dass ein Asset in die
**WebAssetRegistry** geladen wurde. Das bedeutet, dass der
**WebAssetManager** über die Existenz dieser Assets informiert ist.
Allerdings werden sie beim Rendern nicht an ein Dokument angehängt. Alle
Assets, die aus *joomla.asset.json* geladen werden, befinden sich auf
der Stufe **registered**.

**In Verwendung** (used) bedeutet, dass ein Asset über
*\$wa-\>useAsset()* aktiviert wurde (*-\>useScript()*, *-\>useStyle()*,
*-\>registerAndUseX()* usw.). Das heißt, dass der **WebAssetManager**
diese Assets und deren Abhängigkeiten beim Rendern an ein Dokument
anhängt.

Ein Asset lässt sich nicht verwenden, wenn es vorher nicht registriert
wurde. Der Versuch führt zu einer unbekannten Asset-Exception.

## Ein Asset registrieren

Alle bekannten Assets werden geladen und dann in **WebAssetRegistry**
gespeichert. (Zum Aktivieren/Deaktivieren eines Assets muss jedoch der
**WebAssetManager** verwendet werden, siehe nächster Abschnitt)

Joomla! wird während der Ausführung automatisch nach der nächsten
Asset-Definition suchen (in der folgenden Reihenfolge):

    media/vendor/joomla.asset.json (on first access to WebAssetRegistry)
    media/system/joomla.asset.json
    media/legacy/joomla.asset.json
    media/{com_active_component}/joomla.asset.json (on dispatch the application)
    templates/{active_template}/joomla.asset.json

Und lädt sie in die Liste der verfügbaren Assets.

**Hinweis:** Bei jeder nachfolgenden Asset-Definition werden die
Asset-Elemente der vorherigen Asset-Definition durch einen Element-Namen
überschrieben.


Eigene Asset-Definitionen können über die **WebAssetRegistry**
registriert werden:

```php
    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();
    $wr = $wa->getRegistry();
    $wr->addRegistryFile('relative/path/to/your/joomla.asset.json');
```

Ein benutzerdefiniertes Asset-Element während der Ausführung hinzufügen:

```php
    $wr->add('script', new Joomla\CMS\WebAsset\WebAssetItem('foobar', 'com_foobar/file.js', ['type' => 'script']));
```

Als Alternative kann auch der **WebAssetManager** verwendet werden:

```php
    $wa->registerScript('foobar', 'com_foobar/file.js');
```

Das neue Asset-Element **foobar** wird der Registry der bekannten Assets
hinzugefügt, wird aber erst dann an ein Dokument angehängt, wenn es vom
Code (einem Layout, Template etc.) angefordert wird.

Zum Überprüfen, ob ein Asset verfügbar ist:

```php
    if ($wa->assetExists('script', 'foobar'))
    {
        var_dump('Script "foobar" exists!');
    }
```

## Ein Asset aktivieren

Die gesamte Asset-Verwaltung im aktuellen Dokument wird über den
**WebAssetManager** gesteuert, der mit **\$doc-\>getWebAssetManager();**
angesprochen wird. Mit Hilfe des Asset-Managers können die benötigten
Assets in Joomla! durch Standardmethoden aktiviert oder deaktiviert
werden.

Man aktiviert ein Asset auf einer Seite mit der „useAsset“ Funktion. Zum
Beispiel:

```php
    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();
    $wa->useScript('keepalive');

    // Or multiple
    $wa->useScript('keepalive')
        ->useScript('fields.validate')
        ->useStyle('foobar')
        ->useScript('foobar');

    // Add new asset item with dependency and use it
    $wa->registerAndUseScript('bar', 'com_foobar/bar.js', [], [], ['core', 'foobar']);
```

Der **WebAssetManager** prüft bei der **WebAssetRegistry**, ob das
angeforderte Asset vorhanden ist, und aktiviert es für die aktuelle
Dokument-Instanz. Andernfalls wird eine „UnknownAssetException“
ausgelöst.

Man deaktiviert ein Asset auf einer Seite mit der „disableAsset“
Funktion. Im folgenden Beispiel wird das Laden des Assets
„jquery-noconflict“ deaktiviert.

```php
    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();
    $wa->disableScript('jquery-noconflict');
```

**Hinweis:** Gibt es Abhängigkeiten zu dem deaktivierten Asset wird
dieses Asset automatisch wieder aktiviert, egal was auch immer passiert.

Überprüfen, ob das Asset aktiviert ist und wie der Asset-Status ist:

```php
    // Checking whether an asset are active (enabled manually or automatically as dependency)
    if ($wa->isAssetActive('script', 'foobar'))
    {
        var_dump('Script "foobar" is active!');
    }

    // Checking state
    switch($wa->getAssetState('script', 'foobar')){
        case Joomla\CMS\WebAsset\WebAssetManager::ASSET_STATE_ACTIVE:
            var_dump('Active! Was enabled manually');
            break;
        case Joomla\CMS\WebAsset\WebAssetManager::ASSET_STATE_DEPENDENCY:
            var_dump('Active! Was enabled automatically while resolving dependencies');
            break;
        default:
            var_dump('not active!');
    }
```

## Überschreiben eines Assets

Ein Override kann nützlich sein, wenn die URI eines Asset-Elements oder
seiner Abhängigkeiten neu definiert werden muss. Wie bereits erwähnt,
überschreibt jede der folgenden Asset-Definitionen aus joomla.asset.json
die Asset-Elemente der vorherigen Asset-Definitionen, und zwar anhand
des Element-Namens. Das bedeutet, wenn joomla.asset.json vorhanden ist,
werden die bereits geladenen Asset-Elemente durch die eigenen Elemente
ersetzt. Eine weitere Möglichkeit, den Code zu überschreiben, besteht
darin, ein Element mit dem gleichen Namen zu registrieren. Beispiel: Wir
haben ein Skript „foobar“, das die Bibliothek com_example/foobar.js
lädt, und wir möchten CDN für genau diese Bibliothek verwenden:

So war es zunächst im System definiert:

```json
    ...
    {
      "name": "foobar",
      "type": "script",
      "uri": "com_example/foobar.js",
      "dependencies": ["core"]
    }
    ...
```

Um die URI zu überschreiben, definieren wir das Asset-Element mit dem
Namen „foobar“ in unserem joomla.asset.json:

```json
    ...
    {
      "name": "foobar",
      "type": "script",
      "uri": "http://foobar.cdn.blabla/foobar.js",
      "dependencies": ["core"]
    }
    ...
```

Oder, registrieren ein neues Asset-Element mit dem AssetManager:

```php
    $wa->registerScript('foobar', 'http://fobar.cdn.blabla/foobar.js', [], [], ['core']);
```

## Mit Styles arbeiten

Der AssetManager ermöglicht die Verwaltung von Stylesheet-Dateien.
Stylesheet-Asset-Elemente haben den Typ „style“.

Zum Beispiel die json-Definition eines Elements in joomla.asset.json:

```json
    ...
    {
      "name": "foobar",
      "type": "style",
      "uri": "com_example/foobar.css"
    }
    ...
```

### Die Methoden zum Arbeiten mit Styles

AssetManager bietet die folgenden Methoden zum Arbeiten mit Stildateien:

```php
    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();

    // Attach foobar to the document
    $wa->useStyle('foobar');

    // Disable foobar from being attached
    $wa->disableStyle('foobar');

    // Register custom item without json definition
    $wa->registerStyle('bar', 'com_example/bar.css', [], ['data-foo' => 'some attribute'], ['some.dependency']);
    // And use it later
    $wa->useStyle('bar');

    // Register and attach a custom item in one run
    $wa->registerAndUseStyle('bar', 'com_example/bar.css', [], ['data-foo' => 'some attribute'], ['some.dependency']);
```

### Inline-Stil einfügen

Zusätzlich zu den Stil-Dateien können mit dem WebAssetManager
Inline-Stile eingefügt werden, die ihre Beziehung zum Datei-Asset
beibehalten. Inline-Stile können direkt vor der Abhängigkeit, nach der
Abhängigkeit oder wie üblich hinter sämtlichen Stilen platziert werden.

Inline-Assets können einen Namen haben, ebenso wie andere Assets (ist
aber nicht erforderlich). Der Name kann verwendet werden, um das
Asset-Element aus einer Registrierung abzurufen oder um eine
Abhängigkeit zu einem anderen Inline-Asset herzustellen. Wenn der Name
nicht angegeben wird, wird ein generierter Name basierend auf einem
Inhalts-Hash verwendet.

```php
    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();

    // Add an inline content as usual, will be rendered in flow after all assets
    $wa->addInlineStyle('content of inline1');

    // Add an inline content that will be placed after "foobar" asset
    $wa->addInlineStyle('content of inline2', ['position' => 'after'], ['data-foo' => 'bar'], ['foobar']);

    // Add an inline content that will be placed before "foobar" asset
    $wa->addInlineStyle('content of inline3', ['position' => 'before'], [], ['foobar']);

    // Named inline asset
    $wa->addInlineStyle('content of inline4', ['name' => 'my.inline.asset']);
```

**Hinweis:** Das Asset „foobar“ sollte in der Asset-Registrierung
vorhanden sein, andernfalls kommt es zu einer Ausnahme wegen nicht
erfüllter Abhängigkeiten (unsatisfied dependency exception).

Das vorstehende Beispiel ergibt:

    ...
    content of inline3

    content of inline2
    ...
    ...
    content of inline1
    content of inline4
    ...

Wenn das Inline-Asset mehrere Abhängigkeiten hat, wird die letzte für
die Positionierung verwendet. Beispiel:

```php
    $wa->addInlineStyle('content of inline1', ['position' => 'before'], [], ['foo', 'bar']);
    $wa->addInlineStyle('content of inline2', ['position' => 'after'], [], ['foo', 'bar']);
```

Wird erzeugen:

    ...

    content of inline1

    content of inline2
    ...

**Hinweis:** Bestimmte Inline-Assets könnten in Abhängigkeit zu einem
anderen Inline-Asset stehen, es wird jedoch nicht empfohlen, ein
Inline-Asset als abhängig von einem Nicht-Inline-Asset zu verwenden.
Dies funktioniert zwar, aber dieses Verhalten wird sich möglicherweise
in Zukunft ändern. Stattdessen sollte lieber die Funktion „position“
verwendet werden.

## Mit Skripts arbeiten

AssetManager ermöglicht die Verwaltung von Skript-Dateien. Skript-Assets
haben den Typ „script“. Zum Beispiel die json-Definition eines Elements
in joomla.asset.json:

```json
    ...
    {
      "name": "foobar",
      "type": "script",
      "uri": "com_example/foobar.js",
      "dependencies": ["core"]
    }
    ...
```

Beispiel json-Definition eines ES6-Modulskripts, mit Fallback auf die
Legacy-Version:

```json
    ...
    {
      "name": "foobar-legacy",
      "type": "script",
      "uri": "com_example/foobar-as5.js",
      "attributes": {
        "nomodule": true,
        "defer": true
      },
      "dependencies": ["core"]
    }
    {
      "name": "foobar",
      "type": "script",
      "uri": "com_example/foobar.js",
      "attributes": {
        "type": "module"
      },
      "dependencies": [
        "core",
        "foobar-legacy"
      ]
    }
    ...
```

### Die Methoden zum Arbeiten mit Skripts

Der AssetManager bietet folgende Methoden für die Arbeit mit
Skriptdateien:

```php
    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();

    // Attach foobar to the document
    $wa->useScript('foobar');

    // Disable foobar from being attached
    $wa->disableScript('foobar');

    // Register custom item without json definition
    $wa->registerScript('bar', 'com_example/bar.js', [], ['defer' => true], ['core']);
    // And use it later
    $wa->useScript('bar');

    // Register and attach a custom item in one run
    $wa->registerAndUseScript('bar','com_example/bar.js', [], ['defer' => true], ['core']);
```

### Inline-Skript einfügen

Zusätzlich zu Skriptdateien erlaubt der WebAssetManager das Hinzufügen
eines Inline-Skripts, wobei die Beziehung zum Datei-Asset erhalten
bleibt. Ein Inline-Skript kann direkt vor einer Abhängigkeit, nach einer
Abhängigkeit oder wie üblich nach allen Skripten platziert werden.

Inline-Assets können ebenso wie andere Assets einen Namen haben (ist
aber nicht erforderlich). Der Name kann verwendet werden, um das
Asset-Element aus einer Registrierung abzurufen oder um eine
Abhängigkeit zu einem anderen Inline-Asset herzustellen. Wenn der Name
nicht angegeben wird, wird ein generierter Name basierend auf einem
Inhalts-Hash verwendet.

```php
    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();

    // Add an inline content as usual, will be rendered in flow after all assets
    $wa->addInlineScript('content of inline1');

    // Add an inline content that will be placed after "foobar" asset
    $wa->addInlineScript('content of inline2', ['position' => 'after'], ['data-foo' => 'bar'], ['foobar']);

    // Add an inline content that will be placed before "foobar" asset
    $wa->addInlineScript('content of inline3', ['position' => 'before'], [], ['foobar']);

    // Named inline asset
    $wa->addInlineScript('content of inline4', ['name' => 'my.inline.asset']);

    // Specify script type
    $wa->addInlineScript('content of inline5', [], ['type' => 'module']);
```

**Hinweis:** Das Asset „foobar“ sollte in der Asset-Registrierung
vorhanden sein, andernfalls kommt es zu einer Ausnahme wegen nicht
erfüllter Abhängigkeiten (unsatisfied dependency exception).

Das vorstehende Beispiel ergibt:

    ...
    content of inline3

    content of inline2
    ...
    ...
    content of inline1
    content of inline4
    content of inline5
    ...

Wenn das Inline-Asset mehrere Abhängigkeiten hat, wird die letzte für
die Positionierung verwendet. Beispiel:

```php
    $wa->addInlineScript('content of inline1', ['position' => 'before'], [], ['foo', 'bar']);
    $wa->addInlineScript('content of inline2', ['position' => 'after'], [], ['foo', 'bar']);
```

Wird erzeugen:

    ...

    content of inline1

    content of inline2
    ...

**Hinweis:** Bestimmte Inline-Assets könnten in Abhängigkeit zu einem
anderen Inline-Asset stehen, es wird jedoch nicht empfohlen, ein
Inline-Asset als abhängig von einem Nicht-Inline-Asset zu verwenden.
Dies funktioniert zwar, aber dieses Verhalten wird sich möglicherweise
in Zukunft ändern. Stattdessen sollte lieber die Funktion „position“
verwendet werden.

## Mit einer Webkomponente arbeiten

Joomla! ermöglicht es
<a href="https://developer.mozilla.org/de/docs/Web/Web_Components"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">Webkomponenten</a> zu verwenden. In
Joomla! werden Webkomponenten nicht als reguläres Skript geladen,
sondern über den Web-Component-Loader, sodass sie asynchron geladen
werden. Daher muss ein Webkomponenten-Asset-Element einen Flag
„webcomponent“ haben, das auf den booleschen Wert *true* gesetzt ist. In
allen anderen Aspekten ist das Arbeiten mit Webkomponenten im
AssetManager dasselbe wie das Arbeiten mit einem „script“-Asset-Element.

Beispiel json-Definition einiger Webkomponenten in joomla.asset.json
(als ES6-Modul):

```json
    ...
    {
      "name": "webcomponent.foobar",
      "type": "style",
      "uri": "com_example/foobar-custom-element.css",
    },
    {
      "name": "webcomponent.foobar",
      "type": "script",
      "uri": "com_example/foobar-custom-element.js",
      "attributes": {
         "type": "module"
      },
    }
    ...
```

Beispiel mit Fallback, für Browser, welche die ES6-Funktion *module*
nicht unterstützen. Hinweis: Das Legacy-Skript sollte eine
„wcpolyfill“-Abhängigkeit haben, und das Modul-Skript sollte eine
Abhängigkeit vom Legacy-Skript haben:

```json
    ...
    {
      "name": "webcomponent.foobar",
      "type": "style",
      "uri": "com_example/foobar-custom-element.css",
    },
    {
      "name": "webcomponent.foobar-legacy",
      "type": "script",
      "uri": "com_example/foobar-custom-element-es5.js",
      "attributes": {
        "nomodule": true,
        "defer": true
      },
      "dependencies": [
        "wcpolyfill"
      ]
    },
    {
      "name": "webcomponent.foobar",
      "type": "script",
      "uri": "com_example/foobar-custom-element.js",
      "attributes": {
        "type": "module"
      },
      "dependencies": [
        "webcomponent.foobar-legacy"
      ]
    }
    ...
```

Alternativ kann auch die Registrierung in PHP (als ES6-Modul) erfolgen:

```php
    $wa->registerStyle('webcomponent.foobar', 'com_example/foobar-custom-element.css')
        ->registerScript('webcomponent.foobar', 'com_example/foobar-custom-element.js', ['type' => 'module']);
```

In das Dokument einfügen:

```php
    $wa->useStyle('webcomponent.foobar')
        ->useScript('webcomponent.foobar');
```

**Hinweis:** Es wird empfohlen, dem Asset-Namen das Präfix
„webcomponent.“ voranzustellen, um es leicht zu erkennen und von
normalen Skripten in einem Layout zu unterscheiden.

### Die Methoden zum Arbeiten Webkomponenten

Die Methoden zum Arbeiten mit einer Webkomponente sind die gleichen wie
die Methoden zum Arbeiten mit einem Skript-Asset-Element.

## Mit Presets arbeiten

„Preset“ (Voreinstellung) ist eine spezielle Art von Asset-Element, das
eine Liste von Elementen enthält, die aktiviert werden müssen, genauso
wie der direkte Aufruf von useAsset() für jedes Element in der Liste.
Preset kann gemischte Asset-Typen enthalten (Skript, Stil, ein anderes
Preset, usw.), der Typ sollte nach dem \#-Symbol angegeben werden und
folgt nach einem Asset-Namen, Beispiel: foo#style, bar#script.

Zum Beispiel die json-Definition eines Elements in joomla.asset.json:

```json
    ...
    {
      "name": "foobar",
      "type": "preset",
      "uri": "",
      "dependencies": [
        "core#script",
        "foobar#style",
        "foobar#script",
      ]
    }
    ...
```

### Die Methoden zum Arbeiten mit Presets

Der AssetManager bietet folgende Methoden für die Arbeit mit
Preset-Elementen:

```php
    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();

    // Attach all items from foobar preset to the document
    $wa->usePreset('foobar');

    // Disable all items from foobar preset from being attached
    $wa->disablePreset('foobar');

    // Register custom item without json definition
    $wa->registerPreset('bar', '', [], [], ['core#script', 'bar#script']);

    // And use it later
    $wa->usePreset('bar');

    // Register and attach a custom item in one run
    $wa->registerAndUsePreset('bar','', [], [], ['core#script', 'bar#script']);
```

## Fortgeschritten: benutzerdefinierte WebAssetItem Klasse

Die Standard-Klasse für alle WebAsset-Elemente ist
**Joomla\CMS\WebAsset\WebAssetItem**.

Eine benutzerdefinierte Klasse darf auch verwendet werden; sie muss
**Joomla\CMS\WebAsset\WebAssetItemInterface** implementieren oder
**Joomla\CMS\WebAsset\WebAssetItem** erweitern.

Eine benutzerdefinierte Klasse kann es ermöglichen, erweiterte Aktionen
durchzuführen, z. B. eine Skriptdatei einfügen, in Abhängigkeit von
einer aktiven Sprache:

```php
    class MyComExampleAssetItem extends WebAssetItem
    {
        public function getUri($resolvePath = true): string
        {
            $langTag = Factory::getApplication()->getLanguage()->getTag();
            // For script asset use ".js", for style we would use ".css"
            $path    = 'com_example/bar-' . $langTag . '.js';

            if ($resolvePath)
            {
                // For script asset use "script", for style we would use "stylesheet"
                $path = $this->resolvePath($path, 'script');
            }

            return $path;
        }
    }
```

Zusätzlich ermöglicht die Implementierung von
**Joomla\CMS\WebAsset\WebAssetAttachBehaviorInterface** das Hinzufügen
einer Skript-Option (die vom Environment abhängen kann), wenn das Asset
aktiviert und an das Dokument angehängt ist.

```php
    class MyFancyFoobarAssetItem extends WebAssetItem implements WebAssetAttachBehaviorInterface
    {
        public function onAttachCallback(Document $doc): void
        {
            $user = Factory::getApplication()->getIdentity();
            $doc->addScriptOptions('com_example.fancyfoobar', ['userName' => $user->username]);
        }
    }
```

**Wichtiger Hinweis:** Ein Asset-Element, das
**WebAssetAttachBehaviorInterface** implementiert, sollte vor dem
Element <a
href="https://docs.joomla.org/Plugin/Events/System#onBeforeCompileHead"
class="external text" target="_blank"
rel="noreferrer noopener"><em>onBeforeCompileHead</em></a> aktiviert
werden, sonst wird *onAttachCallback* ignoriert.

### eine benutzerdefinierte WebAssetItem Klasse in joomla.asset.json anlegen

Über joomla.asset.json kann definiert werden, welche Klasse für ein
bestimmtes AssetItem verwendet werden soll. Dazu können die beiden
Eigenschaften **namespace** und **class** verwendet werden.
**namespace** kann auf der Root-Ebene (dann wird er als
Standard-Namespace für alle Asset-Elemente in joomla.asset.json benutzt)
oder auf der Elementebene definiert werden. Zum Beispiel:

```json
    {
      "$schema": "https://developer.joomla.org/schemas/json-schema/web_assets.json",
      "name": "com_example",
      "version": "4.0.0",
      "namespace": "Joomla\Component\Example\WebAsset",
      "assets": [
        {
          "name": "foo",
          "type": "script",
          "class": "FooAssetItem",
          "uri": "com_example/foo.js"
        },
        {
          "name": "bar",
          "type": "script",
          "namespace": "MyFooBar\Library\Example\WebAsset",
          "class": "BarAssetItem",
          "uri": "com_example/bar.js"
        }
      ]
    }
```

Hier wird das Asset **foo** der Klasse
**Joomla\Component\Example\WebAsset\FooAssetItem** und **bar** der
Klasse **MyFooBar\Library\Example\WebAsset\BarAssetItem** zugeordnet.

**Hinweis: Wenn** namespace **nicht definiert ist, wird standardmäßig**
Joomla\CMS\WebAsset **verwendet. Wenn** namespace **definiert, aber leer
ist, wird kein Namespace verwendet, sondern nur** class**. Zum
Beispiel:**

```json
    {
      "$schema": "https://developer.joomla.org/schemas/json-schema/web_assets.json",
      "name": "com_example",
      "assets": [
        {
          "name": "foo",
          "type": "script",
          "class": "FooAssetItem",
          "uri": "com_example/foo.js"
        },
        {
          "name": "bar",
          "type": "script",
          "namespace": "",
          "class": "BarAssetItem",
          "uri": "com_example/bar.js"
        }
      ]
    }
```

Hier wird das Asset **foo** der Klasse
**Joomla\CMS\WebAsset\FooAssetItem** und **bar** der Klasse
**BarAssetItem** (ohne Namespace) zugeordnet.

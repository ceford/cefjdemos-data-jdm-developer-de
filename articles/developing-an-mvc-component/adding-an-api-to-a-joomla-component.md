<!-- Filename: J4.x:Adding_an_API_to_a_Joomla_Component / Display title: Eine API zu einer Joomla-Komponente hinzufügen -->

Joomla!  4.0

Diese Seite soll dokumentieren, wie sich die in Joomla 4.0
eingeführte Webservices-Schicht in
bestehende Joomla-Komponenten integrieren können. Dies setzt voraus,
dass die Standard-Joomla-MVC-Schicht verwendet wird.

*** Ein Beispiel der Webservices-Integration zur Erweiterung von Weblinks ***

Repository der Erweiterung:
<a href="https://github.com/joomla-extensions/weblinks"
class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla-extensions/weblinks</a>

Pull request:
<a href="https://github.com/joomla-extensions/weblinks/pull/407"
class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla-extensions/weblinks/pull/407</a>

## Plugin Code

The point of entry to your component from an API call is a plugin.

The plugin re-routes the API call into the API code that services the request.

In this example an API call to the weblinks component might be

```
(yourSite)/api/index.php/v1/weblinks
```

This means your installation file is best represented as a package, pkg_weblinks in this case, so that it can contain not only your original component, but also the required plugin.

1. Create the plugin folder ***plugins/webservices/weblinks***.

Your plugin code goes in a subdirectory of the webservices directory under the plugins directory, in this example ***plugins/webservices/weblinks***.

<img alt="" src="https://docs.joomla.org/images/0/0f/Struct2.png.png" decoding="async" class="thumbimage" data-file-width="328" data-file-height="339" width="328" height="339">

2. In ***weblinks.php***, create the class  ***PlgWebservicesWeblinks***.

```php
use Joomla\\CMS\\Plugin\\CMSPlugin;
use Joomla\\CMS\\Router\\ApiRouter;

class PlgWebservicesWeblinks extends CMSPlugin
{
    public function onBeforeApiRoute(&$router)
    {
        $router->createCRUDRoutes('v1/weblinks', 'weblinks', ['component' => 'com_weblinks']);
    }
}
```

In the ***onBeforeApiRoute*** method, register all the routes needed for the webservice.

When Joomla receives an API call it loads the API router which then collects a list of endpoints it should be aware of by running the ''onBeforeApiRoute'' method in all enabled plugin classes that contain it. Then it can locate the API component relevant to the API endpoint that has been called.

The createCRUDRoutes method creates five routes; two GET routes for list and item data, and one POST, one PATCH, and one DELETE route. If you don't require this, simply create the routes directly here.

## The API code

The router specifies the path for the relevant API code.

This API code is in a folder named ''api'' off the site root. It is exactly analogous to the ''administrator'' and ''site'' sections.

In your installation package this code should be included in your component installer because the Joomla installation process automatically creates a section for each installed component there, whether it has any API code or not.

The structure of this section has the same directory pattern as the other extension sections (i.e. components, modules, plugins).

1\. Verzeichnis erstellen: ***/api***

<img src="https://docs.joomla.org/images/e/ec/Struct1.png"
class="thumbimage" decoding="async" data-file-width="414"
data-file-height="262" width="414" height="262" />

2\. Anlegen der Klasse: ***WeblinksController***

The controller is named in the second parameter of the ''createCRUDRoutes'' method in the plugin.

You can expose more than one output structure by registering more than one route, specifying a different controller for each structure.

```php
use Joomla\\CMS\\Plugin\\CMSPlugin;
use Joomla\\CMS\\Router\\ApiRouter;

class PlgWebservicesWeblinks extends CMSPlugin
{
    public function onBeforeApiRoute(&$router)
    {
        $router->createCRUDRoutes('v1/weblinks', 'weblinks', ['component' => 'com_weblinks']);
    }
}
```

Override the following fields:

```
    $contentType - wird als Standard für $modelName verwendet, wenn die Antwort als Typ-Objekt ausgegeben wird
    $default_view - wird als Standard für $viewName verwendet
```

3\. Anlegen der Klasse: ***JsonApiView.php***

```php
    use Joomla\CMS\MVC\View\JsonApiView as BaseApiView;

    class JsonApiView extends BaseApiView
    {
        protected $fieldsToRenderItem = [
            'id',
            'catid',
            'title',
            'alias',
            'url',
            'xreference',
            'tags',
        ];

        protected $fieldsToRenderList = [
            'id',
            'title',
            'alias',
        ];
    }
```

Die folgenden Felder übersteuern (override):

```
    $fieldsToRenderItem - Array aus Feldern zur Anzeige eines einzelnen Objekts
    $fieldsToRenderList - Array aus Feldern um Objekte aufzulisten
```

Notice that this indicates a significant difference between JSON API views and HTML views. With HTML views you are used to having separate files and directories for displaying a list of items and displaying individual items. There isn't the same "display" difference for JSON, so one JsonapiView handles both types. That's why both fields lists are both shown in this one view file.

3\. Erstellen von: ***weblinks.xml***

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension version="3.1" type="plugin" group="webservices" method="upgrade">
    <name>PLG_WEBSERVICES_WEBLINKS</name>
    <author>Joomla! Project</author>
    <creationDate>August 2017</creationDate>
    <copyright>(C) 2005 - 2019 Open Source Matters. All rights reserved.</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <authorEmail>admin@joomla.org</authorEmail>
    <authorUrl>www.joomla.org</authorUrl>
    <version>4.0.0</version>
    <description>PLG_WEBSERVICES_WEBLINKS_XML_DESCRIPTION</description>
    <files>
         ##FILES##
    </files>
    <languages folder="administrator/language">
         ##LANGUAGE_FILES##
    </languages>
</extension>
```

4\. Erstellen der Dateien
***en-GB/en-GB.plg_webservices_weblinks.ini***,
***en-GB/en-GB.plg_webservices_weblinks.sys.ini*** mit dem folgenden
Inhalt:

```ini
    ; Joomla! Project
    ; Copyright (C) 2005 - 2019 Open Source Matters. All rights reserved.
    ; License GNU General Public License version 2 or later; see LICENSE.txt, see LICENSE.php
    ; Note : All ini files need to be saved as UTF-8

    PLG_WEBSERVICES_WEBLINKS="Web Services - Weblinks"
    PLG_WEBSERVICES_WEBLINKS_XML_DESCRIPTION="Used to add weblinks routes to the Web Services API for your website."
```

## Packaging

There are two considerations for packaging, the code in the API section and the plugin.

### Component Packaging

As mentioned elsewhere, the API code is packaged with the component code. To include the code in the package, add the following section to your component manifest:

```xml
<api>
    <files folder="api">
        <folder>src</folder>
    </files>
</api>
```

This will create a section for your API code in the correctly named (for your component) folder under the **api** folder.

### Plugin Packaging

The manifest file for the plugin itself is shown above. Since the plugin is an integral part of the component now, it is best to package the two together. Similar to a single extension installation file, a package installation file contains each extension in the package, already compressed, and a package manifest file. In the case of this example, the manifest file would look something like this (some fields omitted):

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--~
  ~ @package Weblinks
    ....
  -->

<extension version="4.0.0" type="package" method="upgrade">
    <name>Weblinks</name>
    <packagename>weblinks</packagename>
    <version>x.y.z</version>
    <!-- etc etc -->

    <!-- List of extensions to install -->
    <files>
        <!-- Component -->
        <file type="component" id="com_weblinks">com_weblinks.zip</file>

        <!-- Plugins: web services -->
        <file type="plugin" id="weblinks" group="webservices">plg_webservices_weblinks.zip</file>
    </files>
</extension>
```

## Kategorien

1\. Hinzufügen der Kategorie-Unterstützung für den Weblinks-Webservice.
Bearbeiten der Datei
***src/plugins/webservices/weblinks/weblinks.php***.

```php
    class PlgWebservicesWeblinks extends CMSPlugin
    {
        public function onBeforeApiRoute(&$router)
        {
            ...
            $router->createCRUDRoutes(
                'v1/weblinks/categories',
                'categories',
                ['component' => 'com_categories', 'extension' => 'com_weblinks']
            );
        }
    }
```

Wir verwenden die vorgefertigte Komponente ***com_categories*** und
müssen nur noch den Parameter 'extension' =\> 'com_weblinks'

## Felder

1\. Hinzufügen von Feldern und Feldgruppen für den Webservice
„Weblinks“. Bearbeiten der Datei
***src/plugins/webservices/weblinks/weblinks.php***

```php
    class PlgWebservicesWeblinks extends CMSPlugin
    {
        public function onBeforeApiRoute(&$router)
        {
            ...
            $router->createCRUDRoutes(
                'v1/fields/weblinks',
                'fields',
                ['component' => 'com_fields', 'context' => 'com_weblinks.weblink']
            );

            $router->createCRUDRoutes(
                'v1/fields/groups/weblinks',
                'groups',
                ['component' => 'com_fields', 'context' => 'com_weblinks.weblink']
            );
        }
    }
```

2\. Überschreiben der Funktion ***save*** in ***WeblinksController***

```php
    class WeblinksController extends ApiController
    {
        ...

        protected function save($recordKey = null)
        {
            $data = (array) json_decode($this->input->json->getRaw(), true);

            foreach (FieldsHelper::getFields('com_weblinks.weblink') as $field)
            {
                if (isset($data[$field->name]))
                {
                    !isset($data['com_fields']) && $data['com_fields'] = [];

                    $data['com_fields'][$field->name] = $data[$field->name];
                    unset($data[$field->name]);
                }
            }

            $this->input->set('data', $data);

            return parent::save($recordKey);
        }

        ...
    }
```

3\. Überschreiben der Funktionen ***displayList, displayItem,
prepareItem*** in ***Weblinks\JsonApiView***

```php
    class JsonApiView extends BaseApiView
    {
        ...

        public function displayList(array $items = null)
        {
            foreach (FieldsHelper::getFields('com_weblinks.weblink') as $field)
            {
                $this->fieldsToRenderList[] = $field->name;
            }

            return parent::displayList();
        }

        public function displayItem($item = null)
        {
            foreach (FieldsHelper::getFields('com_weblinks.weblink') as $field)
            {
                $this->fieldsToRenderItem[] = $field->name;
            }

            return parent::displayItem();
        }

        protected function prepareItem($item)
        {
            foreach (FieldsHelper::getFields('com_weblinks.weblink', $item, true) as $field)
            {
                $item->{$field->name} = isset($field->apivalue) ? $field->apivalue : $field->rawvalue;
            }

            return parent::prepareItem($item);
        }

        ...
    }
```

In der Funktion *prepareItem* muss geachtet werden auf: `$field->apivalue`.
Falls der Typ des Feldes komplex ist, wird hoffentlich ein Wert für die
Ausgabe in der Web Services API-Komponente zurückgegeben, andernfalls
greifen wir zurück auf: `$field->rawvalue`.

## Data

The core functionality renders the data that you would expect from a simple component in the normal web interface. For this, the API defaults to administration data models. However, providing a **Model** section in your API code gives you back the flexibility to construct whatever data shape you want just for your API output. Provide a model for each view with the usual naming conventions.

The system JsonApiView class formats the JSON output as three data sets; links, items, and metadata. To change that, override the display class in your own JsonapiView and **don't** call the parent class.

## Beispiel für eine integrierte Arbeit

***Hinweis: Nicht vergessen, das Weblinks Webservice Plugin zu
aktivieren!***

### Weblinks

#### Liste von Weblinks erhalten

curl -X GET /api/index.php/v1/weblinks

#### Einzelnen Weblink erhalten

curl -X GET /api/index.php/v1/weblinks/{weblink_id}

#### Weblink löschen

curl -X DELETE /api/index.php/v1/weblinks/{weblink_id}

#### Weblink erstellen

```bash
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/weblinks -d
'   {
        "access": "1",
        "alias": "",
        "catid": "8",
        "description": "text",
        "images": {
            "float_first": "",
            "float_second": "",
            "image_first": "",
            "image_first_alt": "",
            "image_first_caption": "",
            "image_second": "",
            "image_second_alt": "",
            "image_second_caption": ""
        },
        "language": "*",
        "metadata": {
            "rights": "",
            "robots": ""
        },
        "metadesc": "",
        "metakey": "",
        "modified": "",
        "params": {
            "count_clicks": "",
            "height": "",
            "target": "",
            "width": ""
        },
        "title": "weblink title",
        "url": "http://somelink.com/",
        "xreference": "xreference"
    }'
```

#### Weblink aktualisieren

```bash
curl -X PUT -H "Content-Type: application/json"
/api/index.php/v1/weblinks/{weblink_id} -d
`   {
        "catid": "8",
        "description": "some new text",
        "language": "*",
        "title": "new title",
        "url": "http://newsomelink.com/"
    }'
```

### Kategorien

Die Route zu den Weblinks-Kategorien ist: "v1/weblinks/categories"

Die Arbeitsweise ist vergleichbar mit:
[Banner-Kategorien](https://docs.joomla.org/J4.x:Joomla_Core_APIs#Kategorien "Special:MyLanguage/J4.x:Joomla Core APIs").

## Felder

Die Route zu den Weblinks-Feldern ist: "v1/fields/weblinks"

Die Arbeitsweise ist vergleichbar mit:
[Kontakt-Felder](https://docs.joomla.org/J4.x:Joomla_Core_APIs#Kontakt-Felder "Special:MyLanguage/J4.x:Joomla Core APIs").

### Gruppenfelder

Die Route zu den Weblinks-Gruppenfeldern ist:
"v1/fields/groups/weblinks"

Die Arbeitsweise ist vergleichbar mit:
[Kontakt-Feldgruppen](https://docs.joomla.org/J4.x:Joomla_Core_APIs#Kontakt-Feldgruppen "Special:MyLanguage/J4.x:Joomla Core APIs")

<!-- Filename: J4.x:MVC_Anatomy:_Site_Files / Display title: MVC-Anatomie: Beitragsdateien -->

## Datei-Struktur

Es gibt weniger Dateien im Site-Teil der Komponente als im Administrator-Teil, deshalb scheint dies ein guter Ausgangspunkt zu sein. Es werden nur die Teile jeder Datei behandelt, die einer Erklärung bedürfen. Es ist am besten, wenn Sie jede erwähnte Datei öffnen, den gesamten Inhalt betrachten und dann die erläuterten Teile finden. Die alphabetische Reihenfolge der Dateistruktur ist wie folgt:

```bash
    site
    |- forms
        |- filter_countries.xml
    |- language
        |- en-GB
           |- com_countrybase.ini
    |- src
        |- Controller
           |- DisplayController
        |- Model
           |- CountriesModel.php
        |- Service
           |- Router.php
        |- View
           |- Countries
              |- HtmlView.php
    |- tmpl
        |- countries
           |- default.php
           |- default.xml
```

## DisplayController.php

```php
<?php

/**
 * @package     Countrybase.Site
 * @subpackage  com_countrybase
 *
 * @copyright   (C) 2025 Clifford E Ford
 * @license     GNU General Public License version 3 or later; see LICENSE.txt
 */

namespace Cefjdemos\Component\Countrybase\Site\Controller;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\MVC\Controller\BaseController;
use Joomla\CMS\Router\Route;
use Joomla\CMS\Session\Session;

/**
 * Countrybase Component Controller
 *
 * @since  1.0.0
 */
class DisplayController extends BaseController
{
    /**
     * The default view.
     *
     * @var    string
     */
    protected $default_view = 'countries';
}
```

Teile dieser Datei werden wie folgt erklärt:

### Urheberrechtshinweis

Jede PHP-Datei sollte mit einem Copyright-Hinweis beginnen, wie der folgende:

```php
<?php

/**
 * @package     Countrybase.Site
 * @subpackage  com_countrybase
 *
 * @copyright   (C) 2025 Clifford E Ford
 * @license     GNU General Public License version 3 or later; see LICENSE.txt
 */
```

Wenn Sie häufig neue Dateien erstellen und diesen Abschnitt kopieren/einfügen, denken Sie daran, den Komponenten-Namen und den Copyright-Hinweis zu aktualisieren. Außerdem erfordert der Code-Sniffer eine Leerzeile vor einem Doc-Block.

### Namensraum und definierte Überprüfung

Nach dem Copyright-Hinweis muss jede PHP-Datei eine Zeile mit defined('_JEXEC') oder die; enthalten, mit der Ausnahme, dass Dateien mit Namensraum den Namensraum vor jedem anderen PHP-Code deklarieren müssen, also vor der defined-Überprüfung. PHP-Dateien mit Namensraum sind diejenigen, die Komponenten-PHP-Klassen im src-Ordner oder dessen Unterordnern enthalten.

```php
namespace Cefjemos\Component\Countrybase\Site\Controller;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects
```

Der `defined`-Check verhindert, dass eine PHP-Datei durch direkten Aufruf über ihre URL ausgeführt wird. Die Konstanten `_JEXEC` wird definiert, wenn die Anwendung über die root- oder administrator-index.php-Datei startet. Es ist eine wichtige Sicherheitsmaßnahme.

Der `defined`-Check ist in `phpcs:disable` und `phpcs:enable` eingebettet, um eine Verletzung des PSR12-Codierstandards zuzulassen.

Kombiniert mit dem im countrybase.xml-Manifest deklarierten Namensraum, wird Joomla nach jeder Klasse suchen, die in der aktuellen Datei in root/components/com_countrybase/src/Controller deklariert ist - in diesem Fall wird der Name dieser Datei, DisplayController.php, angehängt.

### Verwendung von Anweisungen

Die use-Anweisungen folgen normalerweise der definierten Prüfung und sind oft in alphabetischer Reihenfolge aufgelistet. Die use-Anweisungen definieren die Orte der Klassen, die von dieser PHP-Datei verwendet werden. Manchmal sind use-Anweisungen versehentlich vorhanden, werden deklariert aber nicht verwendet. Das schadet nicht, sollte aber korrigiert werden. Hier gibt es zwei ungenutzte Anweisungen:

```php
    use Joomla\CMS\Router\Route;
    use Joomla\CMS\Session\Session;
```

### Controller-Klasse

Der Anzeigesteuerung hat fast nichts zu tun, da die gesamte Arbeit in der Elternklasse erledigt wird. Das einzige Wichtige, das er tut, ist, die Standardansicht festzulegen, in diesem Fall **Länder**. Dadurch wird veranlasst, dass die Standardkomponentenansicht die Dateien Countries/HtmlView.php und tmpl/countries/default.php verwendet, um die Länderdaten anzuzeigen.

```php
/**
 * Countrybase Component Controller
 *
 * @since  1.0.0 Created for first release.
 */
class DisplayController extends BaseController
{
    /**
     * The default view.
     *
     * @var    string
     */
    protected $default_view = 'countries';
}
```

Der Dokumentationsblock (DocBlock) vor der `class`-Anweisung wird zur automatischen Dokumentation mit Tools wie *Doxygen* verwendet. Die `@since`-Anweisung ist ein *Tag*, das die Versionsnummer der Erweiterung angibt, in der dieses Element eingeführt wurde. Es kann von einer einfachen Texterklärung gefolgt werden. Jedes Element kann einen DocBlock mit einer beliebigen Anzahl von Standard-Tags haben. Es ist eine gute Praxis, den eigenen Code gut zu dokumentieren!

Vergessen Sie nicht, die \$default_view für diesen Controller festzulegen. Ohne sie wird der DisplayController die Standardansicht verwenden, die in der Komponenten-Konfigurationsdatei definiert ist, oder, falls diese nicht existiert, den Komponenten-Namen.

## src/View/Länder/HtmlView.php

Der Controller hat die Standardansicht auf **Länder** gesetzt, daher besteht der nächste Schritt darin, den entsprechenden HtmlView.php-Code zu laden. Teile dieser Datei erfordern einige Erläuterungen.

### Klassenvariablen

```php
class HtmlView extends BaseHtmlView
{
    /**
     * The model state
     *
     * @var  \Joomla\CMS\Object\CMSObject
     */
    protected $state = null;
    ...
    protected $items = null;
    ...
    protected $pagination = null;
    ...
    public $filterForm;
    ...
    public $activeFilters;
```

Die Klassenvariablen werden verwendet, um Informationen über die anzuzeigende Seite zu speichern:

- `$state` - der Modellstatus, oft durch Formulareingaben oder Abfragezeichenfolgen eingegeben.
- `$items` - die Liste der aus der Datenbank abgerufenen Länderdaten.
- `$pagination` - ein Objekt, das verwendet wird, um einen Seiten-Navigationsmechanismus anzuzeigen, wenn es mehr Seiten als das Listenlimit gibt, normalerweise 20 Einträge.
- `$filterForm` - wird normalerweise nicht auf Beitragsseiten gefunden, sondern in com_countrybase verwendet, um nach Ländername oder Veröffentlichungsstatus zu filtern.
- `$activeFilters` - wird verwendet, um zu verfolgen, welche Filter in Gebrauch sind.

### Display-Funktion

Dies ist ziemlich standardmäßig für eine Seitenansicht, abgesehen von den Teilen filterForm und activeFilters:

```php
    public function display($tpl = null)
    {
        $this->state      = $this->get('State');
        $this->items      = $this->get('Items');
        $this->pagination = $this->get('Pagination');
        $this->filterForm    = $this->get('FilterForm');
        $this->activeFilters = $this->get('ActiveFilters');

        // Flag indicates to not add limitstart=0 to URL
        $this->pagination->hideEmptyLimitstart = true;

        // Check for errors.
        if (count($errors = $this->get('Errors')))
        {
            throw new GenericDataException(implode("\n", $errors), 500);
        }

        parent::display($tpl);
    }
```

Die Anweisungen der Form `$this->get('Xxxx')` veranlassen Joomla, in `CountriesModel.php` nach einer Funktion mit dem Namen `getXxxx()` zu suchen und alle von diesem Code erzeugten Daten zur Speicherung und Verwendung in der Ansicht zurückzugeben. Oft befindet sich die Funktion im Elternteil von CountriesModel. Zum Beispiel ist die Funktion getItems nicht in CountriesModel.php vorhanden, aber sie ist im ListModel vorhanden, das es erweitert.

Zusammenfassend ruft die View-Klasse Daten vom Modell ab und ruft dann ihre Basisklasse auf, um die Daten anzuzeigen.

## Modell/LänderModell.php

Es gibt eine kleine Anzahl von Funktionen im Modell, die Sie normalerweise selbst vervollständigen müssen. Andere werden vom übergeordneten ListModel geerbt.

### __construct

Es ist üblich, im Konstruktor alle Filterfelder einzuschließen, die Sie verwenden möchten. Ohne sie kann es scheinen, als hätten Filter keine Wirkung. Sie werden oft vergessen, wenn Sie zu einem späteren Zeitpunkt einen Filter hinzufügen möchten. Jedes Feld wird mit seinem Feldnamen und einem Tabellennamen-Alias, normalerweise a, b, c und so weiter, angegeben, aber Sie können auch etwas anderes wählen, das im gesamten Code konsistent ist.

```php
       public function __construct($config = array())
        {
            if (empty($config['filter_fields']))
            {
                $config['filter_fields'] = array(
                        'id', 'a.id',
                        'title', 'a.title',
                        'iso_2', 'a.iso_2',
                        'iso_3', 'a.iso_3',
                        'country_code', 'a.country_code',
                        'region_code', 'a.region_code',
                        'state', 'a.state',
                        'subregion_code', 'a.subregion_code',
                        'phone_prefix', 'a.phone_prefix',
                        'currency_code', 'a.currency_code',
                );
            }

            parent::__construct($config);
        }
```

### populateState

Diese Funktion nimmt Eingabewerte und bereitet sie für die Verwendung in einer Datenbankabfrage vor. Einige Parameter werden im Elternteil behandelt, daher ist hier nichts weiter zu tun. Zum Beispiel, wenn das Suchfeld **title** ist, wird das vom Elternteil behandelt, ebenso wie die Felder **state** und die Paginierung **start** und **limit**.

```php
       protected function populateState($ordering = 'title', $direction = 'ASC')
        {
            // List state information.
            parent::populateState($ordering, $direction);
        }
```

### getStoreId

Diese Funktion erstellt einen Hash, um eine Abfrage zur späteren Verwendung zu speichern.

```php
       protected function getStoreId($id = '')
        {
            // Compile the store id.
            $id .= ':' . $this->getState('filter.search');
            $id .= ':' . $this->getState('filter.published');

            return parent::getStoreId($id);
        }
```

### getListQuery

Hier wird die Abfrage erstellt, um Daten aus der Datenbank zu extrahieren. Es erfordert ein gewisses Verständnis dafür, wie Abfragen aufgebaut sind.

```php
    protected function getListQuery()
    {
        // Create a new query object.
        $db = $this->getDatabase();
        $query = $db->getQuery(true);

        // Select the required fields from the table.
        $query->select(
            $this->getState(
                'list.select',
                [
                    $db->quoteName('a.title'),
                    $db->quoteName('a.iso_2'),
                    $db->quoteName('a.iso_3'),
                    $db->quoteName('a.country_code'),
                    $db->quoteName('a.region_code'),
                    $db->quoteName('a.subregion_code'),
                    $db->quoteName('a.phone_prefix'),
                    $db->quoteName('a.currency_code'),
                    $db->quoteName('a.state'),
                    $db->quoteName('b.title') . ' AS currency_title',
                    $db->quoteName('b.symbol'),
                    $db->quoteName('b.dollar_exchange_rate'),
                ]
            )
        )
            ->from($db->quoteName('#__countrybase_countries', 'a'))
            ->leftjoin($db->quoteName('#__countrybase_currencies', 'b') . 'ON a.currency_code = b.currency_code');

        // Filter by search in title.
        $search = $this->getState('filter.search');

        if (!empty($search)) {
            $search = '%' . str_replace(' ', '%', trim($search) . '%');
            $query->where($db->quoteName('a.title') . ' LIKE :search')
            ->bind(':search', $search, ParameterType::STRING);
        }

        // Filter by published state
        $published = (string) $this->getState('filter.published');

        if ($published !== '*') {
            if (is_numeric($published)) {
                $state = (int) $published;
                $query->where($db->quoteName('a.state') . ' = :state')
                    ->bind(':state', $state, ParameterType::INTEGER);
            }
        }

        // Add the list ordering clause.
        $orderCol = $this->state->get('list.ordering', 'a.title');
        $orderDirn = $this->state->get('list.direction', 'ASC');

        $query->order($db->escape($orderCol) . ' ' . $db->escape($orderDirn));
        return $query;
    }
```

Erklärung

- **getQuery(true)** erhält ein neues leeres Abfrageobjekt.
- **\$query-\>select()** fügt eine SELECT-Anweisung hinzu. Es kann mehrere SELECT-Anweisungen geben - Joomla verknüpft sie.
- **Tabellenaliase a und b** beachten Sie, dass einige Spalten aus verschiedenen Tabellen stammen.
- **-\>from()** definiert, welche Tabelle Tabelle a ist. Dies kann in einer separaten Anweisung erfolgen: \$query-\>from();
- **-\>leftjoin()** definiert Tabelle b und wie diese mit Tabelle a verknüpft werden soll.
- **\$query-\>where()** verwendet alle definierten Filter, einen für **Suche** und einen weiteren für **Status**.
- **return \$query** es gibt keinen Elternaufruf, alles in der Abfrage muss hier eingerichtet werden.

## tmpl/länder/standard.php

Dies ist der Teil des Codes, in dem der HTML-Inhalt erstellt wird. Für die Liste der Länder wird eine Tabelle mit einer Kopfzeile und einer Zeile für Daten zu jedem Land benötigt. Da es 250 Länder gibt, ist ein Paginierungsmechanismus erforderlich, um eine Teilmenge von Ländern jeweils einige anzuzeigen. Das erfordert ein Formular. In diesem Fall ist eine standardmäßige Joomla **searchtools**-Leiste nützlich. Das ist es:

```php
<?php

/**
 * @package     Countrybase.Site
 * @subpackage  com_countrybase
 *
 * @copyright   (C) 2025 Clifford E Ford
 * @license     GNU General Public License version 3 or later; see LICENSE.txt
 */

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\HTML\HTMLHelper;
use Joomla\CMS\Language\Text;
use Joomla\CMS\Layout\LayoutHelper;
use Joomla\CMS\Router\Route;

$listOrder = $this->escape($this->state->get('list.ordering'));
$listDirn  = $this->escape($this->state->get('list.direction'));

?>
<h1><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES'); ?></h1>

<form action="<?php echo Route::_('index.php?option=com_countrybase'); ?>"
    method="post" name="adminForm" id="adminForm">

<?php echo LayoutHelper::render('joomla.searchtools.default', array('view' => $this)); ?>

<div class="table-responsive">
    <table class="table table-striped">
    <caption><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_TABLE_CAPTION'); ?></caption>
    <thead>
    <tr>
        <th scope="col">
            <?php echo HTMLHelper::_(
                'searchtools.sort',
                'COM_COUNTRYBASE_COUNTRIES_COUNTRY',
                'a.title',
                $listDirn,
                $listOrder
            ); ?>
        </th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_ISO_2'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_ISO_3'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_CURRENCY_TITLE'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_CURRENCY_SYMBOL'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_CURRENCY_CODE'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_XRATE'); ?></th>
    </tr>
    </thead>
    <tbody>
    <?php foreach ($this->items as $id => $item) : ?>
    <tr>
        <td><?php echo $item->title; ?></td>
        <td><?php echo $item->iso_2; ?></td>
        <td><?php echo $item->iso_3; ?></td>
        <td><?php echo $item->currency_title; ?></td>
        <td><?php echo $item->symbol; ?></td>
        <td><?php echo $item->currency_code; ?></td>
        <td><?php echo $item->dollar_exchange_rate; ?></td>
    </tr>
    <?php endforeach; ?>
    </tbody>
    </table>
</div>

<?php echo $this->pagination->getListFooter(); ?>

<input type="hidden" name="task" value="">
<input type="hidden" name="boxchecked" value="0">
<?php echo HTMLHelper::_('form.token'); ?>

</form>
```

Zu beachtende Punkte:

- **\$listOrder** und **\$listDirection** werden verwendet, um nach Spaltenüberschrift zu ordnen. Nur der Titel wurde dafür eingerichtet.
- **Formularaktion** ist typischerweise so eingerichtet, dass sie auf sich selbst verweist.
- **LayoutHelper::render('joomla.searchtools.default',...)** erstellt eine Suchleiste vom Typ, der in Administratorlisten-Seiten zu sehen ist. **Es benötigt ein Filterformular!**
- **\$this-\>pagination-\>getListFooter()** holt den HTML-Code für das Paginierungs-Widget.
- **Aufgabe** dieses versteckte Feld wird von JavaScript ausgefüllt, wenn das Formular übermittelt wird.
- **boxchecked** dieses versteckte Feld wird verwendet, wenn eine oder mehrere Zeilen-Checkboxen für die Stapelverarbeitung ausgewählt werden. Hier nicht wirklich nötig!
- **HTMLHelper::\_('form.token');** erhält den Code für ein Formular-Token, das als Sicherheitsvorrichtung für Formularübermittlungen mit Datenübermittlung verwendet wird.

## tmpl/länder/default.xml

Diese Datei wird verwendet, um ein Menüelement zu erstellen. Sie hat denselben Namen wie die php-Datei, also default.xml in diesem Fall.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<metadata>
    <layout title="COM_COUNTRYBASE_VIEW_DEFAULT_MENU_LABEL"
        option="COM_COUNTRYBASE_VIEW_DEFAULT_OPTION">
        <help
            url="components/com_countrybase/help/en-GB/countrybase.html"
        />
        <message>
            <![CDATA[COM_COUNTRYBASE_VIEW_DEFAULT_MENU_DESC]]>
        </message>
    </layout>

    <!-- Add fields to the parameters object for the layout. -->
    <fields name="params">

        <!-- Options -->
        <fieldset name="options">
        </fieldset>

    </fields>
</metadata>
```

Hinweise:

- **Hilfe-URL** verweist auf eine Hilfedatei im Administratorordner. Sie ermöglicht es Ihnen, eigene lokale Hilfedateien zu erstellen, die vom Menü Bearbeitungsformular über die Hilfe-Schaltfläche aufgerufen werden, nachdem Sie den Menütyp "Countrybase Standardansicht" ausgewählt haben.
- **Parameter** erlauben es Ihnen, Parameter zu verwenden, zum Beispiel ob eine bestimmte Spalte in der Länderliste angezeigt werden soll oder nicht. Es wurden noch keine Parameter angegeben.
- **Schlüssel** Übersetzungen müssen in der Datei administrator/language/en-GB/countrybase.sys.ini sein. Der Schlüssel *COM_COUNTRYBASE_VIEW_DEFAULT_MENU_DESC*, übersetzt zu *Zeigt eine Länderseite.* erscheint als Beschreibung im Auswahlformular des Menüeintragstyps.

## forms/filter_countries.xml

Diese Datei wird für die Suchleiste benötigt. Ohne sie wird Joomla einen fatalen Fehler auslösen. Der Dateiname muss genau wie gezeigt sein: der Name der Ansicht, vorangestellt mit filter\_. Der Inhalt ist einfach, nur Definitionen für das Suchfeld und alle anderen Filter, die Sie verwenden möchten.

```xml
<?xml version="1.0" encoding="utf-8"?>
<form>
    <fields name="filter">
        <field
            name="search"
            type="text"
            label="COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_LABEL"
            description="COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_DESC"
            hint="JSEARCH_FILTER"
        />

        <field
            name="published"
            type="status"
            label="JOPTION_SELECT_PUBLISHED"
            onchange="this.form.submit();"
            >
            <option value="">JOPTION_SELECT_PUBLISHED</option>
        </field>

    </fields>

    <fields name="list">
        <field
            name="fullordering"
            type="list"
            label="JGLOBAL_SORT_BY"
            default="a.title ASC"
            onchange="this.form.submit();"
            >
            <option value="">JGLOBAL_SORT_BY</option>
            <option value="a.title ASC">COM_COUNTRYBASE_COUNTRIES_FILTER_COUNTRY_ASC</option>
            <option value="a.title DESC">COM_COUNTRYBASE_COUNTRIES_FILTER_COUNTRY_DESC</option>
            <option value="a.iso_2 ASC">ISO2 ASC</option>
            <option value="a.iso_2 DESC">ISO2 DESC</option>
            <option value="a.iso_3 ASC">ISO3 ASC</option>
            <option value="a.iso_3 DESC">ISO3 DESC</option>
            <option value="a.currency_code ASC">COM_COUNTRYBASE_COUNTRIES_FILTER_CURRENCY_CODE_ASC</option>
            <option value="a.currency_code DESC">COM_COUNTRYBASE_COUNTRIES_FILTER_CURRENCY_CODE_DESC</option>
            <option value="a.id ASC">JGRID_HEADING_ID_ASC</option>
            <option value="a.id DESC">JGRID_HEADING_ID_DESC</option>
        </field>

        <field
            name="limit"
            type="limitbox"
            label="JGLOBAL_LIST_LIMIT"
            default="25"
            onchange="this.form.submit();"
        />
    </fields>
</form>
```

Beachten Sie, dass alle Zeichenfolgeschlüssel, die mit J beginnen, von Joomla definiert sind und nicht in Ihre Sprachdateien aufgenommen werden sollten.

## language/de-DE/com_countrybase.ini

Joomla lädt immer zuerst die englischen Sprachschlüsselübersetzungen, bevor eine andere Sprache geladen wird. Dies stellt sicher, dass Schlüssel nicht in der Ausgabe erscheinen, wenn eine Sprache unvollständig übersetzt ist. Da die Sprachschlüssel lange Wörter im Pseudo-Englisch sind, wird es als besser erachtet, eine Mischung aus Englisch und einer anderen Sprache zu haben, als eine Mischung aus Schlüsseln und einer anderen Sprache. Wenn eine andere Sprache verwendet wird, überschreibt Joomla die englischen Zeichenfolgen mit denen der anderen Sprache.

Beachten Sie, dass es gängige Praxis ist, die Zeichenfolgen in alphabetischer Reihenfolge der Schlüssel aufzulisten:

```ini
    ; Joomla! Project
    ; (C) 2005 Open Source Matters, Inc. https://www.joomla.org
    ; License GNU General Public License version 2 or later; see LICENSE.txt
    ; Note : All ini files need to be saved as UTF-8

    COM_COUNTRYBASE_COUNTRIES_COUNTRY="Country"
    COM_COUNTRYBASE_COUNTRIES_CURRENCY_CODE="Code"
    COM_COUNTRYBASE_COUNTRIES_CURRENCY_SYMBOL="Symbol"
    COM_COUNTRYBASE_COUNTRIES_CURRENCY_TITLE="Currency"
    COM_COUNTRYBASE_COUNTRIES_FILTER_COUNTRY_ASC="Country ASC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_COUNTRY_DESC="Country DESC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_CURRENCY_CODE_ASC="Currency code ASC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_CURRENCY_CODE_DESC="Currency code DESC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_DESC="Search in Country Name"
    COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_LABEL="Search"
    COM_COUNTRYBASE_COUNTRIES_ISO_2="ISO2"
    COM_COUNTRYBASE_COUNTRIES_ISO_3="ISO3"
    COM_COUNTRYBASE_COUNTRIES_TABLE_CAPTION="Table of Country Currencies"
    COM_COUNTRYBASE_COUNTRIES_XRATE="Exchange Rate"
    COM_COUNTRYBASE_COUNTRIES="Countries"
```

## src/Service/Router.php

Der Router ist für SEO-URLs erforderlich. Ohne ihn kann ein Menülink als option=com_countrybase&view=countries erscheinen. Mit ihm erscheint ein Link als country-base.html oder unter einem beliebigen Namen, den Sie für den Beiträgetitel-Alias wählen.

```php
    public function __construct(
        SiteApplication $app,
        AbstractMenu $menu
    ) {

        $countries = new RouterViewConfiguration('countries');
        $countries->setKey('id');
        $this->registerView($countries);

        parent::__construct($app, $menu);

        $this->attachRule(new MenuRules($this));
        $this->attachRule(new StandardRules($this));
        $this->attachRule(new NomenuRules($this));
    }
```

Wenn es mehr Ansichten gibt, zum Beispiel eine Tabelle von Währungen, würden Sie jede Ansicht hier vor der parent::\_\_construct()-Anweisung definieren.

*Übersetzt von openai.com*


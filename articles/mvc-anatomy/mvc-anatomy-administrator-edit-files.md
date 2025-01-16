<!-- Filename: J4.x:MVC_Anatomy:_Administrator_Edit_Files / Display title: MVC Anatomie: Administrator Beiträge bearbeiten Dateien -->

## Länderdaten-Dateien

Die Administrator-Dateien beinhalten vier Ansichten: zwei Listenansichten für die Daten der Länder und Währungen sowie zwei Bearbeitungsansichten zur Verwaltung der Dateneingabe in jede der beiden beteiligten Tabellen. Die Listenansichten sind fast identisch mit der beschriebenen Länderlistenansicht der Seitenoberfläche und werden daher hier nicht erneut behandelt. Ähnlich verhält es sich mit den Bearbeitungsansichten, die ebenfalls fast identisch sind. Daher wird nur eine behandelt, die Dateneingabeansicht für ein einzelnes Land.

## src/Controller/LandController.php

Das ist immens einfach! Abgesehen von der Klassendeklaration ist es inhaltsleer. Alles wird von der Elternklasse übernommen.

```php
    class CountryController extends FormController
    {
    }
```

## src/View/Land/HtmlView.php

Hier werden die Daten aus dem Modell abgerufen, um im Dateneingabeformular verwendet zu werden. Es gibt einen erheblichen Unterschied zur zuvor beschriebenen Listenansicht: Es ist üblich, eine Werkzeugleiste mit Schaltflächen für Speichern, Abbrechen, Hilfe usw. zu verwenden.

Wenn Sie das Bearbeitungsformular aufrufen, indem Sie einen Listeneintragstitel auswählen, enthält die URL eine ID, zum Beispiel option=com_countrybase&view=country&layout=edit&id=1. Die ID ist die Datensatznummer der Tabelle. Bei einem neuen Datensatz, der über die Schaltfläche Neu in der Liste aufgerufen wird, fehlt die ID. Wenn sie vorhanden ist, füllt das Modell das Dateneingabeformular mit den vorhandenen Datensatzdaten aus.

```php
    public function display($tpl = null)
    {
        $this->form  = $this->get('Form');
        $this->item  = $this->get('Item');
        $this->state = $this->get('State');

        if (count($errors = $this->get('Errors')))
        {
            throw new GenericDataException(implode("\n", $errors), 500);
        }

        $this->addToolbar();

        return parent::display($tpl);
    }
```

### Die Symbolleiste

Die addToolbar()-Funktion wird typischerweise verwendet, um den Seitentitel festzulegen und geeignete Schaltflächen für die Zugriffsberechtigungen der Person hinzuzufügen, die das Formular verwendet. Beachten Sie die Verwendung der \$isNew-Variable basierend auf der Datensatz-ID, um die Ausgabe leicht zu verändern.

```php
    protected function addToolbar()
    {
        Factory::getApplication()->input->set('hidemainmenu', true);
        $isNew      = ($this->item->id == 0);

        $canDo = ContentHelper::getActions('com_countrybase');

        $toolbar = Toolbar::getInstance();

        ToolbarHelper::title(
            Text::_('COM_COUNTRYBASE_COUNTRY_PAGE_TITLE_' . ($isNew ? 'ADD' : 'EDIT'))
        );

        if ($canDo->get('core.create')) {
            if ($isNew) {
                $toolbar->apply('country.save');
            } else {
                $toolbar->apply('country.apply');
            }
            $toolbar->save('country.save');
        }
        $toolbar->cancel('country.cancel', 'JTOOLBAR_CLOSE');

        ToolbarHelper::inlinehelp();
    }
```

**Notizen:**

- **hidemainmenu** ist für alle Bearbeitungsformulare auf true gesetzt, um zu verhindern, dass Sie das Formular über das Administrator-Menü verlassen, ohne zu speichern.
- **ToolbarHelper::title()** setzt den Titel in der Seitentitelleiste, in diesem Fall auf Land bearbeiten oder Land hinzufügen.
- **toolbar->action**-Buttons, wobei action speichern, anwenden, abbrechen oder eine von vielen anderen sein kann, nehmen (controller.function) als Argumente. In diesem Fall wird bei Auswahl eines Buttons die Aktion an eine Speicher- oder Anwendungsfunktion in der Klasse CountryController übergeben.
- **cancel** nimmt ein zusätzliches Argument, den Zeichenfolgeschlüssel, der zur Beschriftung des Buttons verwendet wird.
- **inlinehelp()** zeigt einen Button, der die Feldbeschreibungen unter jedem Dateneingabefeld ein- oder ausschaltet.

## forms/land.xml

Diese Datei enthält die Spezifikation der Formularfelder, normalerweise in der Reihenfolge, wie sie im Bearbeitungsformular dargestellt werden. Die meisten Felder sind selbsterklärend, daher werden unten nur zwei gezeigt. Die Namenswerte sind die Spaltennamen, die in den Datenbanktabellen verwendet werden.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<form>
        <field
            name="title"
            type="text"
            label="COM_COUNTRYBASE_COUNTRY_TITLE"
            required="true"
            maxlength="128"
        />

        <field
            name="iso_2"
            type="text"
            label="COM_COUNTRYBASE_COUNTRY_ISO_2"
            description="COM_COUNTRYBASE_COUNTRY_ISO_2_DESC"
            required="true"
            maxlength="3"
        />
```

## src/Model/CountryModel.php

Die Funktionen in dieser Datei sind ziemlich standardmäßig. Allerdings erfordert die Funktion getTable() einige Erklärungen.

```php
    public function getTable($name = '', $prefix = '', $options = array())
    {
        $name = 'Countries';
        $prefix = 'Table';

        if ($table = $this->_createTable($name, $prefix, $options)) {
            return $table;
        }

        throw new \Exception(Text::sprintf('JLIB_APPLICATION_ERROR_TABLE_NAME_NOT_SUPPORTED', $name), 0);
    }
```

Diese Funktion bezieht sich nicht auf die Tabelle selbst! Sie bezieht sich auf den Konstruktor von src/Table/CountriesTable.php. Dies kann ein wenig verwirrend sein, da sie von der CountryModel-Klasse aufgerufen wird.

## src/Tabelle/LänderTabelle.php

Also, hier wird der Tabellenname für die Länderliste definiert.

```php
    class CountriesTable extends Table
    {
        /**
         * Constructor
         *
         * @param   DatabaseDriver  $db  Database connector object
         *
         * @since   1.6
         */
        public function __construct(DatabaseDriver $db)
        {
            parent::__construct('#__countrybase_countries', 'id', $db);

            $this->setColumnAlias('published', 'state');
        }
    }
```

Beachten Sie die Deklaration des Spaltenalias. Dies wird verwendet, weil das Formular ein Dateneingabefeld namens **published** verwendet, aber das Feld, das zur Speicherung verwendet wird, heißt **status**.

## tmpl/land/bearbeiten.php

Dies ist die Datei, die die HTML-Ausgabe erstellt. Sie beginnt mit zwei Helfern: HTMLHelper::_('behavior.formvalidator'); lädt das JavaScript, das für die clientseitige Formularvalidierung benötigt wird. Obwohl moderne Browser Formularvalidierungen anwenden, verhindert dies nicht das Absenden des Formulars. HTMLHelper::_('behavior.keepalive'); lädt JavaScript, um den Server zu pingen und so einen Sitzungsablauf zu verhindern, während komplexe Formulare ausgefüllt werden.

**Notizen:**

- LayoutHelper::render('joomla.edit.title_alias', \$this); rendert ein standardmäßiges Joomla-Titelfeld.
- \$this-\>form-\>renderField('iso_2'); rendert das angegebene Feld, in diesem Fall iso_2.
- LayoutHelper::render('joomla.edit.global', \$this); rendert die Felder auf der rechten Seite des Details-Tabs.

```php
<?php

/**
 * @package     Countrybase.Administrator
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

HTMLHelper::_('behavior.formvalidator');
HTMLHelper::_('behavior.keepalive');

?>

<form action="<?php echo Route::_('index.php?option=com_countrybase&view=country&layout=edit&id=' .
        (int) $this->item->id); ?>"
    method="post" name="adminForm" id="country-form" class="form-validate">

    <?php echo LayoutHelper::render('joomla.edit.title_alias', $this); ?>

    <div>
        <?php echo HTMLHelper::_('uitab.startTabSet', 'myTab', array('active' => 'details')); ?>

        <?php echo HTMLHelper::_('uitab.addTab', 'myTab', 'details', Text::_('COM_COUNTRYBASE_COUNTRY_TAB_DETAILS')); ?>
        <div class="row">
            <div class="col-md-9">
                <div class="row">
                    <div class="col-md-6">
                        <?php echo $this->form->renderField('iso_2'); ?>
                        <?php echo $this->form->renderField('iso_3'); ?>
                        <?php echo $this->form->renderField('country_code'); ?>
                        <?php echo $this->form->renderField('region_code'); ?>
                        <?php echo $this->form->renderField('subregion_code'); ?>
                        <?php echo $this->form->renderField('phone_prefix'); ?>
                        <?php echo $this->form->renderField('currency_code'); ?>
                        <?php echo $this->form->renderField('id'); ?>
                    </div>
                </div>
            </div>
            <div class="col-md-3">
                <div class="card card-light">
                    <div class="card-body">
                        <?php echo LayoutHelper::render('joomla.edit.global', $this); ?>
                    </div>
                </div>
            </div>
        </div>
        <?php echo HTMLHelper::_('uitab.endTab'); ?>

        <?php echo HTMLHelper::_('uitab.endTabSet'); ?>
    </div>
    <input type="hidden" name="task" value="">
    <?php echo HTMLHelper::_('form.token'); ?>
</form>
```

Du kannst durch Formular-Feldgruppen und Felder innerhalb jeder Feldgruppe wechseln. Das kann die Ausgabe von komplexen Formularen mit vielen Tabs vereinfachen.

![Land Bearbeitungsformular](../../../en/images/mvc-anatomy/com-countrybase-edit-country.png)

*Übersetzt von openai.com*


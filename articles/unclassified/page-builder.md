<!-- Filename: J4.x:Page_Builder / Display title: Page Builder -->

<span id="main-portal-heading">GSoC 2019
Joomla 4 Page Builder
Dokumentation</span> [<img
src="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/75px-Gsoc2016.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/113px-Gsoc2016.png 1.5x, https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/150px-Gsoc2016.png 2x"
data-file-width="373" data-file-height="373" width="75" height="75"
alt="Gsoc2016.png" />](https://docs.joomla.org/GSoC_2019 "GSoC 2019")
Joomla!  4.x

## Einleitung

Angenommen, jemand möchte ein Template erstellen, ohne HTML zu
programmieren. CSS sollte nur für das Styling und nicht für die
Definition des Layouts verwendet werden. Dann sind der Page-Builder und
das Frontend-Template "Apodis" nützliche Tools. Das Hauptaugenmerk liegt
auf einem Editor, der es ermöglicht, benutzerdefinierte Positionen mit
Hilfe einer Drag & Drop-GUI zu erstellen. Benutzer können vordefinierte
Elemente - Container, Raster, Spalten, Modulpositionen, sowie
benutzerdefinierte Elemente - verwenden. Entwickler von Drittanbietern
können ihre eigenen Elemente über Plugins hinzufügen.

## Erste Schritte

Um den Page-Builder zu verwenden, muss das aktivierte Frontend-Template
das Pagebuilder-Feld in der `templateDetails.xml` enthalten, wie hier
von Apodis:

```xml
<fieldset name="pagebuilder" label="TPL_APODIS_PAGEBUILDER">
   <field
		name="grid"
		type="pagebuilder"
		hidden="true"
		label="TPL_APODIS_PAGEBUILDER"
   />
</fieldset>
```

Das Template selbst muss den RenderHelper laden, der die vom Editor
befüllten Parameter rendert. Dies geschieht in den Vorlagen `index.php`.

```php
    use Joomla\Component\Templates\Administrator\Helper\RenderHelper;

    $grid = $this->params->get('grid');
```

Danach wird die Ausgabe innerhalb von HTML im Body platziert:

```php
<?php echo RenderHelper::renderElements($grid); ?>
```

## Editor

Der Editor für den Page-Builder ist in den Template-Stilen und auf der
entsprechenden Registerkarte verfügbar. Er unterstützt das Hinzufügen
neuer Elemente, das Ziehen und Ablegen, um sie nach Belieben neu
anzuordnen und zu positionieren, sowie die Möglichkeit, die
Spaltenbreite zu ändern. Die Verkettung von Elementen ist ebenfalls
verfügbar und kann durch Ändern der Plugin-Einstellungen geändert
werden. Dies schafft nahezu grenzenlose Möglichkeiten.

Page Builder Editor:

<a
href="https://docs.joomla.org/index.php?title=Special:Upload&amp;wpDestFile=Pagebuilder-editor-de.jpg"
class="new" title="File:Pagebuilder-editor-de.jpg">800px</a>

Benutzer können verschiedene Elemente zum Layout hinzufügen, indem sie
die Schaltfläche "Element hinzufügen", in den Zeilen unten, verwenden.
Dann erscheint ein modales Fenster, in dem man die verfügbaren Elemente
auswählen und weitere Optionen direkt einstellen kann.

<img
src="https://docs.joomla.org/images/9/9d/Pagebuilder-add-element-en.jpg"
decoding="async" data-file-width="800" data-file-height="480"
width="800" height="480" alt="Add elements to the layout" />

Es ist möglich, benutzerdefinierte CSS-Klassen mit Hilfe des
Einstellungs-Icons hinzuzufügen, das auf jedem Element vorhanden ist.
Hier können Benutzer auch Offsets zu Spalten hinzufügen und für jede
Modulposition Name und Modul-Chrome auswählen. Plugins sind in der Lage,
ihre eigenen Optionen für die Seitenleiste anzubieten.

Benutzer können Elemente in der Seitenleiste für Einstellungen
bearbeiten:

<img
src="https://docs.joomla.org/images/b/bc/Pagebuilder-element-settings-en.jpg"
decoding="async" data-file-width="800" data-file-height="492"
width="800" height="492" alt="Pagebuilder-element-settings-en.jpg" />

Als Ergebnis wird ein im Parameterfeld gespeichertes JSON-Objekt mit
Typ, Optionen, Größe und Unterobjekten jedes Elements ausgegeben. Der
Editor lädt automatisch das gespeicherte Layout aus dem Parameter,
sodass der Benutzer direkt mit der Bearbeitung fortfahren kann.

## Elemente

Es gibt vier Standardelemente, die mit dem Page Builder mitgeliefert
werden:

1.  Container
2.  Raster
3.  Spalte
4.  Modul-Position

Die Standardkonfiguration ist folgenden Tabelle zu sehen, die über
Plugins geändert werden kann.

|                       |              |                         |        |                               |
|-----------------------|--------------|-------------------------|--------|-------------------------------|
| Config                | Container    | Grid                    | Column | Module Position               |
| Parents allowed       | Root, Column | Root, Column, Container | Grid   | Root, Grid, Column, Container |
| Contain children      | True         | True                    | True   | False                         |
| Can contain component | True         | True                    | True   | False                         |
| Can contain message   | True         | True                    | True   | False                         |

## Komponente und Meldung

Nicht nur Modulpositionen sind wichtige Elemente im Page Builder. Die
Position der Komponente und die auftretenden Meldungen können per Drag &
Drop auf Elemente gesetzt werden, die dies akzeptieren. Das macht es
sehr einfach, die Positionen auszuwählen, die den gewohnten Seiteninhalt
ausgeben. Auf einem Element kann nur eines von beiden platziert werden.
Ein Klick auf das 'x' entfernt die Position – sie kann dann erneut
gesetzt werden.

Benutzer können die Komponenten- oder Nachrichtenposition auf Page
Builder-Elementen festlegen:

<img
src="https://docs.joomla.org/images/3/37/Pagebuilder-set-component-and-message-en.jpg"
decoding="async" data-file-width="800" data-file-height="402"
width="800" height="402"
alt="Users can set the component or message position on Page builder elements" />

## Apodis

Der Page-Builder ist in dem neuen Apodis Frontend-Template integriert,
das es den Benutzern ermöglicht, ihre eigenen Template-Stile mit Hilfe
des Editors auszuwählen und per Drag & Drop zu verschieben. Das neue
Template-Design holt sich die Page-Builder-Elemente aus der index.php
und zeigt die gerenderte Ausgabe direkt an. Dies ist für unabhängige
Tests sinnvoll, bei denen keine Stylesheets das Standardverhalten
unterbrechen können. Und so funktioniert es: Eine
com_templates-Hilfsfunktion wird in index.php aufgerufen und iteriert
rekursiv durch die JSON-Parameter aus der \#\_template_styles-Tabelle,
die Typ, Position und alle Informationen der gespeicherten
Pagebuilder-Elemente enthält. Je nach Typ und Optionen wird die
Darstellung geändert oder HTML hinzugefügt. In Zukunft wird der Renderer
auch Pagebuilder-Plugins und deren gewünschtes Rendering
berücksichtigen.

## Integration von Zustand und Methoden mit Vuex

Vuex wird verwendet, um den Status mehrerer Komponenten von einem
zentralen Vuex-Speicher abzurufen und kann reaktiv und effizient
aktualisiert werden, wenn sich der Status des Speichers ändert. Die
Verwendung von Vuex hat die Codebasis und die Architektur von
Page-Builder viel modularer, effizienter und für zukünftige
Erweiterungen erweiterbar gemacht. Es ermöglicht uns auch, Ereignisse zu
verfolgen, die den Status ändern, und hilft uns bei der Fehlersuche mit
der Vue-Devtools-Extension.

## Integration von Bootstrap 4

Wir haben unsere eigenen BS4-Dateien in den Pagebuilder-Editor
eingebunden, um der Problematik zu begegnen, wenn im Backend-Template
keine Bootstrap-Dateien enthalten sind.

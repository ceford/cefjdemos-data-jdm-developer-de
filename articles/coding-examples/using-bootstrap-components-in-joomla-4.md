<!-- Filename: J4.x:Using_Bootstrap_Components_in_Joomla_4 / Display title: Verwendung von Bootstrap-Komponenten in Joomla 4 -->

## Einführung

### Bootstrap-Komponenten

Einige der in der Bootstrap-Dokumentation beschriebenen Komponenten verwenden nur CSS. Zum Beispiel wird die Breadcrumbs-Komponente mit CSS gerendert und benötigt keine JavaScript-Unterstützung. Andere reagieren auf Benutzeraktionen wie Klicken oder Hover und benötigen JavaScript-Unterstützung. Letztere werden hier als Interaktive Komponenten bezeichnet. Dieser Beitrag erklärt, wie man sie in Beiträgen und einem benutzerdefinierten Modul verwendet.

Siehe die [Bootstrap-Dokumentation](https://getbootstrap.com/docs/5.0/getting-started/introduction/).

### Joomla 4 führt einen modularen Ansatz für interaktive Komponenten ein

- Was ist ein modularer Ansatz?
- Die Funktionalität wird in einzelne Komponenten aufgeteilt, die durch individuelle Dateien unterstützt werden. Es gibt keinen **ein-Datei**-Ansatz wie bei Bootstrap in Joomla 3. Der modulare Ansatz ist effizienter und bietet Leistungsgewinne (senden Sie nur den Code, der benötigt wird, anstatt alles zu liefern, falls eine Seite eine Komponente benötigen könnte).

### Verwendung interaktiver Komponenten: Programmierer

- Laden Sie je nach Bedarf! Es gibt Helferfunktionen, um einzelne Komponenten mit den passenden Argumenten einzurichten.
- Sehen Sie die Liste der interaktiven Bootstrap-Komponenten.

### Verwenden interaktiver Komponenten: Nicht-Coder

- Die Verwendung von Komponenten in Beiträgen ist nicht so einfach, da die Hilfsfunktionen nicht aus einem Beitrag oder einem Standard-Custom-HTML-Modul aufgerufen werden können. Später in diesem Tutorial werden drei mögliche Lösungen vorgeschlagen:
  - Ein benutzerdefiniertes Modul
  - Ein benutzerdefiniertes Plugin
  - Ein benutzerdefiniertes Modul-Override
- Überspringen oder durchsuchen Sie die Liste der Bootstrap-Interaktiven-Komponenten. Sie werden diese Funktionsaufrufe nicht direkt verwenden.

## Interaktive Bootstrap-Komponenten

### Warnung

Angenommen, Sie haben den HTML-Teil bereits in Ihrem Layout, müssen Sie auch die Interaktivität (den JavaScript-Teil) einbeziehen:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.alert', '.selector');
```

- Der **.selector** bezieht sich auf den CSS-Selektor für die Warnung. Sie können diese Funktion mehrfach mit verschiedenen CSS-Selektoren aufrufen.
- Keine zusätzlichen Optionen verfügbar

### Schaltfläche

Angenommen, Sie haben den HTML-Teil bereits in Ihrem Layout, müssen Sie auch die Interaktivität (den JavaScript-Teil) einbinden:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.button', '.selector');
```

- Der **.selector** bezieht sich auf den CSS-Selektor für den Button. Sie können diese Funktion mehrmals mit unterschiedlichen CSS-Selektoren aufrufen.
- Keine zusätzlichen Optionen verfügbar.

### Karussell

Angenommen, Sie haben den HTML-Teil bereits in Ihrem Layout, müssen Sie auch die Interaktivität (den JavaScript-Teil) einbinden:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.carousel', '.selector', []);
```

- Der **.selector** bezieht sich auf den CSS-Selektor für das Karussell. Sie können diese Funktion mit verschiedenen CSS-Selektoren mehrfach aufrufen.
- Das dritte Argument bezieht sich auf die für das Karussell verfügbaren Optionen.

Optionen für das Karussell können sein:

- **Intervall**, Zahl, Standardwert: **5000**, Die Zeitspanne, die zwischen dem automatischen Wechsel eines Beitrags verzögert wird. Wenn false, wird das Karussell nicht automatisch wechseln.
- **Tastatur**, Boolean, Standardwert: **wahr** Ob das Karussell auf Tastaturereignisse reagieren soll.
- **Pause**, Zeichenfolge\|Boolean, **hover** Pausiert das Wechseln des Karussells bei Mouseenter und setzt das Wechseln des Karussells bei Mouseleave fort.
- **Slide**, Zeichenfolge\|Boolean, Standardwert: **false** Spielt das Karussell nach dem manuellen Wechseln des ersten Beitrags automatisch ab. Wenn "carousel", spielt das Karussell beim Laden ab.

### Reduzieren

Angenommen, Sie haben den HTML-Teil bereits in Ihrem Layout, müssen Sie auch die Interaktivität (den JavaScript-Teil) einschließen:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.collapse', '.selector', []);
```

- Der **.selector** bezieht sich auf den CSS-Selektor für den Zusammenklappen. Sie können diese Funktion mehrfach mit verschiedenen CSS-Selektoren aufrufen.
- Das dritte Argument bezieht sich auf die verfügbaren Optionen für das Zusammenklappen.

Optionen für den Zusammenbruch können sein:

- **parent**, string, Standardwert:**false** Wenn ein parent angegeben wird, werden alle einklappbaren Elemente unter dem angegebenen parent geschlossen, wenn dieses einklappbare Element angezeigt wird.
- **toggle**, boolean, Standardwert:**true** Schaltet das einklappbare Element bei Aufruf um.

### Dropdown-Menü

Angenommen, Sie haben den HTML-Teil bereits in Ihrem Layout, müssen Sie auch die Interaktivität (den JavaScript-Teil) einbinden:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.dropdown', '.selector', []);
```

- Der **.selector** bezieht sich auf den CSS-Selektor für das Dropdown-Menü. Sie können diese Funktion mehrmals mit verschiedenen CSS-Selektoren aufrufen.
- Das dritte Argument bezieht sich auf die für das Dropdown-Menü verfügbaren Optionen.

Optionen für den Zusammenbruch können sein:

- **flip**, boolean, Standardwert:**true** Erlaubt dem Dropdown, sich umzuklappen, falls es zu einer Überlappung mit dem Referenzelement kommt.
- **boundary**, string, Standardwert:**scrollParent** Überlaufbegrenzung des Dropdown-Menüs.
- **reference**, string, Standardwert:**toggle** Referenzelement des Dropdown-Menüs. Akzeptiert **toggle** oder **parent**.
- **display**, string, Standardwert:**dynamic** Standardmäßig verwenden wir Popper für die dynamische Positionierung. Deaktivieren Sie dies mit **static**.

### Modal

Angenommen, Sie haben den HTML-Teil bereits in Ihrem Layout, müssen Sie auch die Interaktivität (den JavaScript-Teil) einbinden:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.modal', '.selector', []);
```

- Der **.selector** bezieht sich auf den CSS-Selektor für das Modal. Sie können diese Funktion mehrfach mit verschiedenen CSS-Selektoren aufrufen.
- Das dritte Argument bezieht sich auf die verfügbaren Optionen für das Modal.

Optionen für das Modal können sein:

- **Backdrop**, string\|boolean Standardwert:**true** Beinhaltet ein Modal-Backdrop-Element. Alternativ können Sie static angeben für ein Backdrop, das das Modal nicht beim Klicken schließt.
- **Tastatur**, boolean Standardwert:**true** Schließt das Modal, wenn die Escape-Taste gedrückt wird.
- **Fokus**, boolean Standardwert:**true** Schließt das Modal, wenn die Escape-Taste gedrückt wird.

### Offcanvas

Angenommen, Sie haben den HTML-Teil bereits in Ihrem Layout, müssen Sie auch die Interaktivität (den JavaScript-Teil) einbeziehen:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.offcanvas', '.selector', []);
```

- Der **.selector** bezieht sich auf den CSS-Selektor für das Offcanvas. Sie können diese Funktion mehrfach mit verschiedenen CSS-Selektoren aufrufen.
- Das dritte Argument bezieht sich auf die verfügbaren Optionen für Offcanvas.

Optionen für die Offcanvas können sein:

- **Hintergrund**, Boolean, Standardwert:**true** Einen Hintergrund auf den Körper anwenden, wenn das Offcanvas geöffnet ist.
- **Tastatur**, Boolean, Standardwert:**true** Schließt das Offcanvas, wenn die Escape-Taste gedrückt wird.
- **Scrollen**, Boolean, Standardwert:**false** Ermöglicht das Scrollen des Körpers, während das Offcanvas geöffnet ist.

### Popover

Angenommen, Sie haben den HTML-Teil bereits in Ihrem Layout, müssen Sie auch die Interaktivität (den JavaScript-Teil) einfügen:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.popover', '.selector', []);
```

- Der **.selector** bezieht sich auf den CSS-Selektor für das Popover. Sie können diese Funktion mehrmals mit unterschiedlichen CSS-Selektoren aufrufen.
- Das dritte Argument bezieht sich auf die verfügbaren Optionen für das Popover.

Optionen für das Popover können sein:

- **animation**, boolean, Standardwert:**true** Wendet einen CSS-Überblendungsübergang auf das Popover an.
- **container**, string\|boolean, Standardwert:**false** Hängt das Popover an ein spezifisches Element an. Z.B.: **body**.
- **content**, string, Standardwert:**null** Standardinhaltswert, wenn das data-bs-content-Attribut nicht vorhanden ist.
- **delay**, number, Standardwert:**0** Verzögert das Anzeigen und Verbergen des Popovers (ms), gilt nicht für den manuellen Auslösetyp.
- **html**, boolean, Standardwert:**true** Fügt HTML in das Popover ein. Wenn **false**, wird die innerText-Eigenschaft verwendet, um Inhalt in das DOM einzufügen.
- **placement**, string, Standardwert:**right** Wie das Popover positioniert wird - **auto** \| **top** \| **bottom** \| **left** \| **right**. Wenn auto angegeben ist, wird das Popover dynamisch neu ausgerichtet.
- **selector**, string, Standardwert:**false** Wenn ein Selektor angegeben ist, werden Popover-Objekte an die angegebenen Ziele delegiert.
- **template**, string, Standardwert:**null** Basis-HTML zur Verwendung beim Erstellen des Popovers.
- **title**, string, Standardwert:**null** Standardtitelwert, wenn das **title**-Tag nicht vorhanden ist.
- **trigger**, string, Standardwert:**click** Wie das Popover ausgelöst wird - **click** \| **hover** \| **focus** \| **manual**.
- **offset**, integer, Standardwert:**0** Versatz des Popovers relativ zu seinem Ziel.

### Scrollspy

Angenommen, Sie haben den HTML-Teil bereits in Ihrem Layout, müssen Sie auch die Interaktivität (den JavaScript-Teil) einfügen:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.scrollspy', '.selector', []);
```

- Der **.selector** bezieht sich auf den CSS-Selektor für den Scrollspy. Sie können diese Funktion mehrmals mit verschiedenen CSS-Selektoren aufrufen.
- Das dritte Argument bezieht sich auf die verfügbaren Optionen für den Scrollspy.

Optionen für den Scrollspy können sein:

- **offset** Anzahl der Pixel, um die Position beim Scrollen vom oberen Rand zu berechnen.
- **method** Zeichenkette, die herausfindet, in welchem Abschnitt sich das beobachtete Element befindet.
- **target** Zeichenkette, die das Element angibt, auf das das Scrollspy-Plugin angewendet wird.

### Tab

Angenommen, Sie haben den HTML-Teil bereits in Ihrem Layout, müssen Sie auch die Interaktivität (den JavaScript-Teil) einbeziehen:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tab', '.selector', []);
```

- Der **.selector** bezieht sich auf den CSS-Selektor für das Tab. Sie können diese Funktion mehrmals mit verschiedenen CSS-Selektoren aufrufen.
- Das dritte Argument bezieht sich auf die verfügbaren Optionen für das Tab.

Optionen für die Registerkarte können sein:

### Tooltip

Angenommen, Sie haben den HTML-Teil bereits in Ihrem Layout, müssen Sie auch die Interaktivität (den JavaScript-Teil) einfügen:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tooltip', '.selector', []);
```

- Der **.selector** bezieht sich auf den CSS-Selektor für das Tooltip. Sie können diese Funktion mehrmals mit verschiedenen CSS-Selektoren aufrufen.
- Das dritte Argument bezieht sich auf die verfügbaren Optionen für Tooltip.

Optionen für das Tooltip können sein:

- **animation**, boolean Wendet eine CSS-Übergangsanimation auf das Popover an.
- **container**, string\|boolean Hängt das Popover an ein bestimmtes Element an: { container: **body** }.
- **delay**, number\|object Verzögert das Anzeigen und Verbergen des Popovers (ms) - gilt nicht für den manuellen Auslösetyp. Wenn eine Zahl angegeben wird, wird die Verzögerung sowohl auf Ausblenden als auch Einblenden angewendet. Die Objektstruktur ist: delay: { show: 500, hide: 100 }.
- **html**, boolean Fügt HTML in das Popover ein. Wenn false, wird die text-Methode von jQuery verwendet, um Inhalte in das DOM einzufügen.
- **placement**, string\|function Positionierung des Popovers - **top** \| **bottom** \| **left** \| **right**.
- **selector** string Wenn ein Selektor angegeben wird, werden Popover-Objekte an die angegebenen Ziele delegiert.
- **template**, string Basis-HTML zur Verwendung beim Erstellen des Popovers.
- **title**, string\|function Standardwert für den Titel, wenn das **title**-Tag nicht vorhanden ist.
- **trigger**, string Wie das Popover ausgelöst wird - hover \| focus \| manual.
- **constraints**, array Ein Array von Einschränkungen - wird an Popper übergeben.
- **offset**, string Versatz des Popovers relativ zu seinem Ziel.

### Toast

Angenommen, Sie haben den HTML-Teil bereits in Ihrem Layout, müssen Sie auch die Interaktivität (den JavaScript-Teil) einbinden:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.toast', '.selector', []);
```

- Der **.selector** bezieht sich auf den CSS-Selektor für den Toast. Sie können diese Funktion mehrmals mit verschiedenen CSS-Selektoren aufrufen.
- Das dritte Argument bezieht sich auf die verfügbaren Optionen für den Toast.

Optionen für den Toast können sein:

- **animation**, boolean, Standard:**true** Wendet eine CSS-Fade-Transition auf den Beitrag an.
- **autohide**, boolean, Standard:**true** Blendet den Beitrag automatisch aus.
- **delay**, Zahl, Standard:**5000** Verzögert das Ausblenden des Beitrags (ms).

### Siehe auch

- **Akkordeon** verwendet Collapse, um Datenpanele anzuzeigen.
- **Listengruppe** kann mit Tab kombiniert werden, um geteilte Inhalte anzuzeigen.

## Verwendung von Bootstrap-Komponenten in Beiträgen

Der HTML-Code für die meisten Komponenten kann in einem Beitrag oder einem Modul enthalten sein, das selbst in einem Beitrag enthalten sein kann. Der Haken dabei ist, dass der HTMLHelper-Aufruf zur Einrichtung der JavaScript-Unterstützung dort nicht enthalten sein kann. Es gibt mehrere mögliche Ansätze für dieses Problem. Hier werden drei Ansätze vorgeschlagen: Verwendung eines benutzerdefinierten Moduls, Verwendung eines Plugins oder Verwendung eines Template-Overrides.

**Vorsicht:** Die TinyMCE- und JCE-Editoren entfernen beim Speichern Leerzeichen und erschweren das Bearbeiten von Code! Die einfache Lösung ist, oben rechts auf Ihrem Administratorbildschirm das **Benutzermenü → Konto bearbeiten** auszuwählen und den Editor auf Code Mirror zu setzen.

## Ansatz 1: Verwendung eines benutzerdefinierten Moduls

Dies ist wahrscheinlich der fehleranfälligste Ansatz, da die Bootstrap-Komponentenunterstützungsoptionen mit Kontrollkästchen ausgewählt werden. Die folgenden Schritte sind beteiligt:

- Laden Sie dieses [Custom Module](https://github.com/ceford/j4xdemos-mod-custom-bscompos/raw/master/mod_custom_bscompos.zip) herunter, installieren und aktivieren Sie es.
- Gehen Sie im Administratormenü zu **Inhalt**→** Website-Module → Neu**
- Wählen Sie **Benutzerdefinierte BS-Komponenten**
- Geben Sie einen Titel ein
- Schalten Sie den Editor in den Klartextmodus und fügen Sie den HTML-Code für die gewünschte Komponente ein oder tippen Sie ihn ein.
- Blättern Sie im Optionen-Tab zur Liste der BS-Komponenten und wählen Sie den Komponententyp in dieser Instanz des Moduls aus. Beachten Sie, dass Sie mehr als eine auswählen können, wenn Sie mehr als eine Komponente verwenden.
- Wählen Sie eine Modulposition: entweder
  - eine im Template definierte Position, wenn Sie das Modul an einem bestimmten Ort verwenden möchten, oder
  - geben Sie eine Position ein, wenn Sie das Modul innerhalb eines spezifischen Beitrags verwenden möchten: tippen Sie im Beitrag {loadposition whatever} ein
- Speichern und auf der Website testen!

### Selektoren

Für einige Komponenten wird die JavaScript-Aktion durch eine spezifische **Klasse** im HTML-Code ausgelöst. In anderen Komponenten wird die Aktion durch ein **data-bs-whatever** Attribut ausgelöst. Die folgenden sind die aktuellen Auslöser und können sich ändern:

- **Alarm** ausgelöst durch class="alert ..."
- **Schaltfläche** ausgelöst durch data-bs-toggle="button"
- **Karussell** ausgelöst durch data-bs-ride="whatever"
- **Zusammenklappen** ausgelöst durch data-bs-toggle="collapse"
- **Dropdown** ausgelöst durch data-bs-toggle="dropdown"
- **Modal** ausgelöst durch data-bs-toggle="modal"
- **Offcanvas** ausgelöst durch data-bs-toggle="offcanvas"
- **Popover** ausgelöst durch class="btn ..." oder
    - Tag (könnte in class="haspopover ..." geändert werden) UND
    - data-bs-toggle="popover"
- **Scrollspy** ausgelöst durch data-bs-spy="scroll"
- **Tab** ausgelöst durch data-bs-toggle="tab"
- **Toast** ausgelöst durch class="toast ..."
- **Tooltip** ausgelöst durch class="btn ..." oder
    - Tag (könnte in class="hastooltip ..." geändert werden) UND
    - data-bs-toggle="tooltip"

### Beispiel 1: Warnung

Benachrichtigungen können im HTML-Code ohne JavaScript-Unterstützung verwendet werden. Dies ist nur für die Schließen-Funktion erforderlich. HTML-Code-Beispiel:

```html
<div class="alert alert-warning alert-dismissible fade show" role="alert">
  <strong>Holy guacamole!</strong> You should check in on some of those fields below.
  <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
</div>
```

Beispielergebnis der Einbindung eines Moduls in einem Beitrag:

![Bootstrap-Warnung](../../../en/images/coding-examples/coding-examples-alert.png)

Beachten Sie, dass ohne JavaScript-Unterstützung der Alarm genau wie oben erscheint, jedoch ein Klick auf die Schließen-Schaltfläche \[X\] den Alarm nicht schließt. Der Alarm wird auch bei jedem Seitenladen angezeigt.

### Beispiel 2: Schaltflächen

Tasten können im HTML-Code ohne JavaScript-Unterstützung verwendet werden. Dies ist nur für die manchmal subtile Stiländerung erforderlich, die auf Tasten mit einem Zustandswechsel angewendet wird, im aktiven Stil. Bootstrap-Beispielcode:

```html
<p><button type="button" class="btn btn-primary" data-bs-toggle="button" autocomplete="off">Toggle button</button>
<button type="button" class="btn btn-primary active" data-bs-toggle="button" autocomplete="off" aria-pressed="true">Active toggle button</button>
<button type="button" class="btn btn-primary" disabled data-bs-toggle="button" autocomplete="off">Disabled toggle button</button></p>
```

```html
<p><a href="#" class="btn btn-primary" role="button" data-bs-toggle="button">Toggle link</a>
<a href="#" class="btn btn-primary active" role="button" data-bs-toggle="button" aria-pressed="true">Active toggle link</a>
<a href="#" class="btn btn-primary disabled" tabindex="-1" aria-disabled="true" role="button" data-bs-toggle="button">Disabled toggle link</a></p>
```

Mit diesem Stil in der Vorlage user.css:

```css
    .btn.btn-primary.active {
        background-color: green;
    }
```

![Bootstrap-Schaltflächen](../../../en/images/coding-examples/coding-examples-buttons.png)

Die Schaltflächen wechseln zwischen Blau und Grün.

### Beispiel 3: Karussell

Der Carousel bietet eine Diashow, die eine Reihe von Bildern oder Textfenstern durchläuft. Das folgende Beispiel verwendet Bilder aus dem Joomla 4 Sample. Bootstrap-Code:

```html
<div id="carouselExampleCaptions" class="carousel slide" data-bs-ride="carousel">
    <div class="carousel-indicators">
        <button class="active" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="0" aria-current="true" aria-label="Slide 1"></button>
        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="1" aria-label="Slide 2"></button>
        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="2" aria-label="Slide 3"></button>
    </div>
    <div class="carousel-inner">
        <div class="carousel-item active"><img class="d-block w-100" src="images/sampledata/cassiopeia/nasa1-1200.jpg" alt="..." />
            <div class="carousel-caption d-none d-md-block">
                <h5>First slide label</h5>
                <p>Some representative placeholder content for the first slide.</p>
            </div>
        </div>
        <div class="carousel-item"><img class="d-block w-100" src="images/sampledata/cassiopeia/nasa2-1200.jpg" alt="..." />
            <div class="carousel-caption d-none d-md-block">
                <h5>Second slide label</h5>
                <p>Some representative placeholder content for the second slide.</p>
            </div>
        </div>
        <div class="carousel-item"><img class="d-block w-100" src="images/sampledata/cassiopeia/nasa3-1200.jpg" alt="..." />
            <div class="carousel-caption d-none d-md-block">
                <h5>Third slide label</h5>
                <p>Some representative placeholder content for the third slide.</p>
            </div>
        </div>
    </div>
    <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="prev">
        <span class="visually-hidden">Previous</span>
    </button>
    <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="next">
        <span class="visually-hidden">Next</span>
    </button>
</div>
```

Ergebnis:

![Bootstrap-Karussell](../../../en/images/coding-examples/coding-examples-carousel.jpg)

### Beispiel 4: Einklappen

Einklappen wird in Joomla häufig verwendet, und Sie müssen möglicherweise kein Modul oder Plugin verwenden, um eine Aktion auszulösen. Ein Klick öffnet ein Fenster mit zusätzlichen Informationen. Beispiel-Bootstrap-Code:

```html
<p>
    <a class="btn btn-primary" role="button" href="#collapseExample" data-bs-toggle="collapse" aria-expanded="false" aria-controls="collapseExample"> Link with href </a>
    <button class="btn btn-primary" type="button" data-bs-toggle="collapse" data-bs-target="#collapseExample" aria-expanded="false" aria-controls="collapseExample">
        Button with data-bs-target
    </button>
</p>
<div id="collapseExample" class="collapse">
    <div class="card card-body">
        Some placeholder content for the collapse component. This panel is hidden by default but revealed when the user activates the relevant trigger.
    </div>
</div>
```

Ergebnis:

![Bootstrap collaps](../../../en/images/coding-examples/coding-examples-collapse.png)

### Beispiel 5: Dropdown

Dropdowns sind umschaltbare, kontextbezogene Overlays zur Anzeige von Listen mit Links und mehr. Beispiel für Bootstrap-Code:

```html
<div class="btn-group">
    <button class="btn btn-danger dropdown-toggle" type="button" data-bs-toggle="dropdown" aria-expanded="false"> Action </button>
    <ul class="dropdown-menu">
        <li><a class="dropdown-item" href="#">Action</a></li>
        <li><a class="dropdown-item" href="#">Another action</a></li>
        <li><a class="dropdown-item" href="#">Something else here</a></li>
        <li><hr class="dropdown-divider" /></li>
        <li><a class="dropdown-item" href="#">Separated link</a></li>
    </ul>
</div>
```

Ergebnis:

![Bootstrap-Dropdown](../../../en/images/coding-examples/coding-examples-dropdown.png)

### Beispiel 6: Modal

Der Modal-Komponente öffnet ein Dialogfeld in der Mitte des Bildschirms. Es gibt eine Reihe von Optionen, um die Größe und den Inhalt des Modals zu steuern. Weitere Details finden Sie in der Bootstrap-Dokumentation. Beispielhaftes Bootstrap-Code:

```html
<p><button class="btn btn-primary" type="button" data-bs-toggle="modal" data-bs-target="#exampleModal"> Launch demo modal </button></p>
<div id="exampleModal" class="modal fade" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h5 id="exampleModalLabel" class="modal-title">Modal title</h5>
                <button class="btn-close" type="button" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                ...
            </div>
            <div class="modal-footer">
                <button class="btn btn-secondary" type="button" data-bs-dismiss="modal">Close</button>
                <button class="btn btn-primary" type="button">Save changes</button>
            </div>
        </div>
    </div>
</div>
```

Ergebnis:

![Bootstrap-Modul](../../../en/images/coding-examples/coding-examples-modal.png)

### Beispiel 7: Offcanvas

Derzeit wird diese Komponente in Joomla nicht unterstützt. Bleiben Sie dran - kommt bald!

### Beispiel 8: Popover

Popovers sind wie Tooltips, aber mit einem Titel. Sie haben einige Zugänglichkeits- und Leistungsprobleme, daher sollten sie mit Vorsicht verwendet werden. Beispielcode für Bootstrap:

```html
<p><button class="btn btn-lg btn-danger" title="Popover title" type="button" data-bs-toggle="popover" data-bs-content="And here's some amazing content. It's very engaging. Right?">Click to toggle popover</button></p>
```

Ergebnis:

![Bootstrap-Warnung](../../../en/images/coding-examples/coding-examples-popover.png)

### Beispiel 9: Scrollspy

Beispielcode:

```html
<div class="row">
    <div class="col-4">
        <nav id="navbar-example3" class="navbar navbar-light bg-light flex-column">
            <a class="navbar-brand" href="#">Navbar</a><nav class="nav nav-pills flex-column">
            <a class="nav-link active" href="#item-1">Item 1</a>
            <nav class="nav nav-pills flex-column">
                <a class="nav-link ms-3 my-1" href="#item-1-1">Item 1-1</a>
                <a class="nav-link ms-3 my-1" href="#item-1-2">Item 1-2</a>
            </nav>
            <a class="nav-link" href="#item-2">Item 2</a>
            <a class="nav-link" href="#item-3">Item 3</a>
            <nav class="nav nav-pills flex-column">
                <a class="nav-link ms-3 my-1" href="#item-3-1">Item 3-1</a>
                <a class="nav-link ms-3 my-1" href="#item-3-2">Item 3-2</a>
            </nav>
        </nav>
    </div>
    <div class="col-8">
        <div class="scrollspy-example" tabindex="0" data-bs-spy="scroll" data-bs-target="#navbar-example3" data-bs-offset="0">
            <h4 id="item-1">Item 1</h4>
            <p>Placeholder content for the scrollspy example. This one relates to item 1. Takes you miles high, so high, 'cause she’s got that one international smile. There's a stranger in my bed, there's a pounding in my head. Oh, no. In another life I would make you stay. ‘Cause I, I’m capable of anything. Suiting up for my crowning battle. Used to steal your parents' liquor and climb to the roof. Tone, tan fit and ready, turn it up cause its gettin' heavy. Her love is like a drug. I guess that I forgot I had a choice.</p>
            <h5 id="item-1-1">Item 1-1</h5>
            <p>Placeholder content for the scrollspy example. This one relates to the item 1-1. You got the finest architecture. Passport stamps, she's cosmopolitan. Fine, fresh, fierce, we got it on lock. Never planned that one day I'd be losing you. She eats your heart out. Your kiss is cosmic, every move is magic. I mean the ones, I mean like she's the one. Greetings loved ones let's take a journey. Just own the night like the 4th of July! But you'd rather get wasted.</p>
            <h5 id="item-1-2">Item 1-2</h5>
            <p>Placeholder content for the scrollspy example. This one relates to the item 1-2. Her love is like a drug. All my girls vintage Chanel baby. Got a motel and built a fort out of sheets. 'Cause she's the muse and the artist. (This is how we do) So you wanna play with magic. So just be sure before you give it all to me. I'm walking, I'm walking on air (tonight). Skip the talk, heard it all, time to walk the walk. Catch her if you can. Stinging like a bee I earned my stripes.</p>
            <h4 id="item-2">Item 2</h4>
            <p>Placeholder content for the scrollspy example. This one relates to item 2. Don't need apologies. There is no fear now, let go and just be free, I will love you unconditionally. Last Friday night. Don't be a shy kinda guy I'll bet it's beautiful. Summer after high school when we first met. 'Cause she's the muse and the artist. What? Wait. No, no, no, no. Thought that I was the exception.</p>
            <h4 id="item-3">Item 3</h4>
            <p>Placeholder content for the scrollspy example. This one relates to item 3. Word on the street, you got somethin' to show me, me. All this money can't buy me a time machine. Make it like your birthday everyday. So we hit the boulevard. You make me feel like I'm livin' a teenage dream, the way you turn me on Skip the talk, heard it all, time to walk the walk. Word on the street, you got somethin' to show me, me. It's no big deal, it's no big deal, it's no big deal.</p>
            <h5 id="item-3-1">Item 3-1</h5>
            <p>Placeholder content for the scrollspy example. This one relates to item 3-1. Baby do you dare to do this? This is no big deal. Yeah, you're lucky if you're on her plane. Just own the night like the 4th of July! Standing on the frontline when the bombs start to fall. So just be sure before you give it all to me.</p>
            <h5 id="item-3-2">Item 3-2</h5>
            <p>Placeholder content for the scrollspy example. This one relates to item 3-2. You're original, cannot be replaced. All night they're playing, your song. California girls we're undeniable. Like a bird without a cage. There is no fear now, let go and just be free, I will love you unconditionally. I can see the writing on the wall. You could travel the world but nothing comes close to the golden coast.</p>
        </div>
    </div>
</div>
```

Ergebnis:

![Bootstrap scrollspy](../../../en/images/coding-examples/coding-examples-scrollspy.png)

Bitte übersetzen Sie den folgenden Markdown-Text aus dem Englischen ins Deutsche und verwenden Sie dabei das Wort Beiträge anstelle von Artikeln.
Auch ist etwas Styling in user.css erforderlich:

```css
    .scrollspy-example {
        height: 350px;
        overflow-y: scroll;
    }
```

Haken: Das Menü stimmt in diesem Beispiel nicht gut mit den Beiträgen überein!

### Beispiel 10: Tabulator

Tabs werden oft als Navigationselemente in Kombination mit Dropdowns verwendet. Bootstrap-Beispielcode:

```html
<ul class="nav nav-tabs">
    <li class="nav-item"><a class="nav-link active" href="#" aria-current="page">Active</a></li>
    <li class="nav-item dropdown"><a class="nav-link dropdown-toggle" role="button" href="#" data-bs-toggle="dropdown" aria-expanded="false">Dropdown</a>
        <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Action</a></li>
            <li><a class="dropdown-item" href="#">Another action</a></li>
            <li><a class="dropdown-item" href="#">Something else here</a></li>
            <li><hr class="dropdown-divider" /></li>
            <li><a class="dropdown-item" href="#">Separated link</a></li>
        </ul>
    </li>
    <li class="nav-item"><a class="nav-link" href="#">Link</a></li>
    <li class="nav-item"><a class="nav-link disabled" tabindex="-1" href="#" aria-disabled="true">Disabled</a></li>
</ul>
```

Ergebnis:

![Bootstrap-Tab](../../../en/images/coding-examples/coding-examples-tab.png)

Denken Sie daran, sowohl die Tab- als auch die Dropdown-Optionen zu überprüfen, damit der Dropdown-Teil funktioniert.

### Beispiel 11: Toast

Toasts sind leichte Benachrichtigungen, die entwickelt wurden, um die Push-Benachrichtigungen zu imitieren, die durch mobile und Desktop-Betriebssysteme populär gemacht wurden. Sie sind mit Flexbox gebaut, sodass sie einfach auszurichten und zu positionieren sind. Beispiel-Boostrap-Code:

```html
<div class="toast fade show" role="alert" aria-live="assertive" aria-atomic="true">
    <div class="toast-header">
        <img class="rounded me-2" src="..." alt="..." />
        <strong class="me-auto">Bootstrap</strong> <small>11 mins ago</small>
        <button class="btn-close" type="button" data-bs-dismiss="toast" aria-label="Close"></button>
    </div>
    <div class="toast-body">Hello, world! This is a toast message.</div>
</div>
```

Ergebnis:

![Bootstrap-Toast](../../../en/images/coding-examples/coding-examples-toast.png)

Beachten Sie, dass die Bootstrap-Demo, die einen Button verwendet, um die Toast-Nachricht anzuzeigen, zusätzliches JavaScript benötigt. Es scheint, dass dieser Bestandteil einen Programmierer benötigt, um ihn gut zu nutzen!

### Beispiel 12: Tooltip

Ein Tooltip ist ein kleines Stück Text, das beim Überfahren eines Buttons oder Link-Elements erscheint, um zu erklären, was es ist oder tut. Der Tooltip kann oberhalb, unterhalb, links oder rechts des Elements positioniert werden. Wenn keine spezielle Position angegeben wird, ist die Standardposition oben. Der Tooltip wechselt zu einer anderen Position, wenn nicht genügend Platz in der angegebenen Position vorhanden ist. Beispiel Bootstrap-Code:

```html
<p><button class="btn btn-secondary" title="Tooltip on left" type="button" data-bs-toggle="tooltip" data-bs-placement="left"> Tooltip on left </button>
<button class="btn btn-secondary" title="Tooltip" type="button" data-bs-toggle="tooltip"> Tooltip </button>
<button class="btn btn-secondary" title="Tooltip on top" type="button" data-bs-toggle="tooltip" data-bs-placement="top"> Tooltip on top </button>
<button class="btn btn-secondary" title="Tooltip on right" type="button" data-bs-toggle="tooltip" data-bs-placement="right"> Tooltip on right </button>
<button class="btn btn-secondary" title="Tooltip on bottom" type="button" data-bs-toggle="tooltip" data-bs-placement="bottom"> Tooltip on bottom </button>
<button class="btn btn-secondary" title="<em>Tooltip</em> <u>with</u> <b>HTML</b>" type="button" data-bs-toggle="tooltip" data-bs-html="true"> Tooltip with HTML </button></p>
<p>Tooltip triggered by class: <button class="btn btn-warning" title="Tooltip Message">Alert!</button></p>
```

Ergebnis:

![Bootstrap Tooltip](../../../en/images/coding-examples/coding-examples-tooltip.png)

## Ansatz 2: Verwendung eines Inhalts-Plugins

Die Schritte umfassen:

- Laden Sie dieses Plugin herunter, installieren und aktivieren Sie es: [](https://github.com/ceford/j4xdemos-plg-bscompos/raw/master/plg_j4xdemos_bscompos.zip)
- Fügen Sie im Beitrag den Text hinzu, auf den das Plugin reagiert, zum Beispiel {bscompos modal carousel}, was das Laden des notwendigen JavaScripts zum Unterstützen eines modalen Dialogs und eines Karussells auslösen wird. Das Plugin entfernt den auslösenden Text und die umgebenden (jetzt) leeren Tags.
- Fügen Sie den HTML-Code der Bootstrap-Komponente direkt im Beitrag oder in einem im Beitrag enthaltenen Modul ein. Unten finden Sie Beispiel-HTML-Code für ein einfaches Modal und ein Modal, das ein Karussell enthält. Beachten Sie, dass dies nicht funktioniert, wenn sich der HTML-Code in einem Modul in einer Template-Position befindet.
- Dies funktioniert auch für ein Standard-Custom-Modul, wenn die Option "Inhalt vorbereiten" auf Ja gesetzt ist.
- Testen Sie es!

## Ansatz 3: Verwendung einer Vorlagen-Überschreibung

Die Schritte, die beteiligt sind:

- Erstellen Sie eine mod_custom-Template-Überschreibung.
- Fügen Sie ein mod_custom-Modul hinzu, das das Komponenten-Markup und die Triggerklassen enthält.
- Schließen Sie das Modul in einem Beitrag ein.

### Die mod_custom Template-Override

- Gehen Sie in der Administratoroberfläche zu **System → Site-Vorlagen → Cassiopeia-Details und Dateien**.
- Wählen Sie **Overrides erstellen **→** mod_custom **→** default.php**.
- Fügen Sie in der Zeile nach defined('\_JEXEC') or die; den folgenden Code ein:

```php
    $module_class = $params->get('moduleclass_sfx');
    if (!empty($module_class))
    {
        $classes = explode(' ', $module_class);
        foreach ($classes as $class)
        {
        switch ($class)
            {
              case 'bs-alert':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.alert', '.alert');
                break;
              case 'bs-button':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.button', '.btn');
                break;
              case 'bs-carousel':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.carousel', '.selector', []);
                break;
              case 'bs-collapse':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.collapse', '.selector', []);
                break;
              case 'bs-dropdown':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.dropdown', '.selector', []);
                break;
              case 'bs-modal':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.modal', '.selector', []);
                break;
              case 'bs-offcanvas':
                // Not Found
                //\Joomla\CMS\HTML\HTMLHelper::_('bootstrap.offcanvas', '.btn', []);
                break;
              case 'bs-popover':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.popover', '.btn', []);
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.popover', 'a', []);
                break;
              case 'bs-scrollspy':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.scrollspy', '.selector', []);
                break;
              case 'bs-tab':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tab', '.selector', []);
                break;
              case 'bs-tooltip':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tooltip', '.btn', []);
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tooltip', 'a', []);
                break;
              case 'bs-toast':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.toast', '.selector', []);
                break;
              default:
                // do nothing
            }
        }
    }
```

Dieser Code sucht nach Klassennamen, die in mod_custom festgelegt sind, und führt den Aufruf von HTMLHelper aus, um die JavaScript-Unterstützung einzurichten. Beachten Sie, dass das letzte Element in jedem Aufruf ein Selektor ist, der möglicherweise verwendet wird oder nicht, um eine Aktion auszulösen. Viele der Komponenten werden durch Datenattribute im Mark-up ausgelöst und verwenden die Selektoren hier nicht. Für einige ist der Selektor jedoch notwendig. Zum Beispiel macht es Sinn, die **.btn**-Klasse und das **a**-Tag zu verwenden, um Tooltips auszulösen.

### Ein mod_custom Modul für eine Modal-Komponente

- Gehe zu **Inhalte**→** Seitenmodule**→** Neu**.
- Wähle das **Benutzerdefiniert**-Modul aus.
- Fülle das Formular aus:
  - Titel: Demo Modal
  - Im Positionsfeld **demomodal** eingeben zur Nutzung in einem Beitrag;
  - Modulinhalt: Editor umschalten für die Eingabe von einfachem Text.
  - Füge den folgenden Code aus der Bootstrap-Dokumentation ein:

```html
<h2>Modal</h2>
<!-- Button trigger modal -->
<p><button class="btn btn-primary" type="button" data-bs-toggle="modal" data-bs-target="#exampleModal"> Launch demo modal </button></p>
<!-- Modal -->
<div id="exampleModal" class="modal fade" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h5 id="exampleModalLabel" class="modal-title">Modal title</h5>
                <button class="btn-close" type="button" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                ...
            </div>
            <div class="modal-footer">
                <button class="btn btn-secondary" type="button" data-bs-dismiss="modal">Close</button>
                <button class="btn btn-primary" type="button">Save changes</button>
            </div>
        </div>
    </div>
</div>
```

- Wählen Sie die Registerkarte Erweitert und geben Sie im Feld Modulklasse **bs-modal** ein.
- Optional: Setzen Sie den Titel auf Ausblenden, um das H2 im eingefügten Code zu verwenden.
- Speichern und schließen (machen Sie sich keine Sorgen, dass das Modal im Editor falsch aussieht).

### Erstellen Sie einen Beitrag und ein Menüelement

- Erstellen Sie einen neuen Beitrag, Demo Modal, und setzen Sie im Nur-Text-Eingabemodus den Inhalt auf

```html
<div>{loadposition demomodal}</div>
```

- Erstellen Sie ein Menüelement für einen einzelnen Beitrag.
- Testen Sie es:

![Bootstrap-Modul-Modul in Beiträge](../../../en/images/coding-examples/coding-examples-modal-module.png)

### Eine Modalkomponente mit einem Karussell

- Erstellen Sie ein neues benutzerdefiniertes Modul mit einem neuen Titel: Demo Modal Karussell
- Position: demomodalcarousel
- **Erweitert-Registerkarte**→**Modulklasse**: bs-modal bs-carousel
- Benutzerdefinierter Modulinhalt im einfachen Text:

```html
<h2>Modal with Carousel</h2>
<div class="mod-custom custom">
    <!-- Button trigger modal -->
    <button class="btn btn-primary" type="button" data-bs-toggle="modal" data-bs-target="#exampleModal"> Launch demo modal </button>
</div>
<div id="exampleModal" class="modal fade" style="display: none;" tabindex="-1" aria-hidden="true">
    <div class="modal-dialog" role="document">
        <div class="modal-content">
            <div class="modal-header">
                <button class="close" type="button" data-bs-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">×</span>
                </button>
            </div>
            <div class="modal-body">
                <div id="carouselExampleCaptions" class="carousel slide" data-bs-ride="carousel">
                    <div class="carousel-indicators">
                        <button class="active" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="0" aria-current="true" aria-label="Slide 1"></button>
                        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="1" aria-label="Slide 2"></button>
                        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="2" aria-label="Slide 3"></button>
                    </div>
                    <div class="carousel-inner">
                        <div class="carousel-item active">
                            <img class="d-block w-100" src="images/sampledata/cassiopeia/nasa1-1200.jpg" alt="..." />
                            <div class="carousel-caption d-none d-md-block">
                                <h5>First slide label</h5>
                                <p>Some representative placeholder content for the first slide.</p>
                            </div>
                        </div>
                        <div class="carousel-item">
                            <img class="d-block w-100" src="images/sampledata/cassiopeia/nasa2-1200.jpg" alt="..." />
                            <div class="carousel-caption d-none d-md-block">
                                <h5>Second slide label</h5>
                                <p>Some representative placeholder content for the second slide.</p>
                            </div>
                        </div>
                        <div class="carousel-item">
                            <img class="d-block w-100" src="images/sampledata/cassiopeia/nasa3-1200.jpg" alt="..." />
                            <div class="carousel-caption d-none d-md-block">
                                <h5>Third slide label</h5>
                                <p>Some representative placeholder content for the third slide.</p>
                            </div>
                        </div>
                    </div>
                    <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="prev">
                        <span class="visually-hidden">Previous</span>
                    </button>
                    <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="next">
                        <span class="visually-hidden">Next</span>
                    </button>
                </div>
            </div>
        </div>
    </div>
</div>
```

- Erstellen Sie einen neuen Beitrag mit {loadposition demomodalcarousel} im Inhalt.
- Erstellen Sie einen neuen Menüpunkt für einen einzelnen Beitrag: Demo Modal Carousel
- Testen Sie es:

![Bootstrap Modal-Karussell](../../../en/images/coding-examples/coding-examples-modal-carousel.png)

*Übersetzt von openai.com*

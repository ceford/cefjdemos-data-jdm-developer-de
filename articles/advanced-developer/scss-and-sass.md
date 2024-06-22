<!-- Filename: J4.x:SCSS_and_Sass / Display title: SCSS und Sass -->

Die meisten der Standard-Stylesheets von Joomla 4.x sind mit Hilfe von
<a
href="https://en.wikipedia.org/wiki/https://en.wikipedia.org/wiki/Sass_(stylesheet_language))"
class="extiw"
title="wikipedia:https://en.wikipedia.org/wiki/Sass (stylesheet language))">SCSS</a>
geschrieben und werden dann compiliert, um die
<a href="https://en.wikipedia.org/wiki/CSS" class="extiw"
title="wikipedia:CSS">CSS</a> Dateien zu erstellen.

## Wo können die .scss-Stylesheets und der Compiler gefunden werden?

Die `.scss` Kern-Dateien befinden sich in verschiedenen Verzeichnissen.
Im Einzelnen sind das die Dateien:

- templates/cassiopeia/scss/template.scss
- administrator/templates/atum/scss/bootstrap.scss
- administrator/templates/atum/scss/font-awesome.scss
- administrator/templates/atum/scss/template.scss
- administrator/templates/atum/scss/template-rtl.scss
- media/plg_installer_webinstaller/scss/client.scss

Das CSS-Generierungsskript, der SCSS-Compiler und andere ähnliche
Build-Tools befinden sich im Verzeichnis `build/` des Joomlaǃ
Quellcodes, der sich auf
<a href="https://github.com/joomla/joomla-cms" class="external text"
target="_blank" rel="nofollow noreferrer noopener">GitHub</a> befindet.
Weitere Informationen zur Verwendung von GitHub finden Sie unter [Git
für
Entwickler](https://docs.joomla.org/Git_for_Coders "Special:MyLanguage/Git for Coders").
Das Build-Verzeichnis ist nur im Quellcode von Joomlaǃ verfügbar, es ist
nicht in einer offiziellen Version von Joomlaǃ enthalten.

## Voraussetzungen

Das Kompilieren eigener SCSS-Dateien mit dem Joomlaǃ Kernprogramm
erfordert, dass auf dem lokalen Rechner Node.js installiert ist. Um
Node.js zu installieren, geht man auf die offizielle Website
<a href="https://nodejs.org/en/" class="external text" target="_blank"
rel="nofollow noreferrer noopener">Node.js</a>, lädt das entsprechende
Setup für sein Betriebssystem herunter und installiert es, wobei man dem
Installationsassistenten folgt. Der NPM (Node Package Manager) wird zur
Verwaltung und Einrichtung der JavaScript-Testumgebung verwendet.

## Abhängigkeiten (Pakete) installieren

Ein Terminal bzw. Eingabeaufforderung öffnen und zum Root-Verzeichnis
von Joomla 4.x wechseln.

Den folgenden Befehl ausführen `npm install`

    $ npm install
    ...
    added 1354 packages in 193.687s
    $

Dadurch werden alle Abhängigkeiten in das Verzeichnis `/node_modules`
installiert. Wenn der Ordner `/node_modules` nicht existiert, wird der
Ordner automatisch von npm erstellt. Es ist in Ordnung, wenn einige
Warnungen angezeigt werden, aber man sollte eigentlich keine
Fehlermeldung sehen.

## Erneute Generierung der CSS-Stylesheets

Um alle CSS-Dateien aus einer Joomlaǃ Kerndistribution neu zu
generieren, müssen Sie die Generierungsskripte auf der
<a href="https://en.wikipedia.org/wiki/command-line_interface"
class="extiw" title="wikipedia:command-line interface">Kommandozeile</a>
ausführen.

### Alle Joomlaǃ Kern-SCSS-Dateien

Sie können alle Joomlaǃ Kern-CSS-Dateien mit dem Befehl
`npm run build:css` kompilieren. Zum Beispiel:

    $ npm run build:css
    Prefixing for:  last 1 version
    Without `from` option PostCSS could generate wrong source map and will not find Browserslist config. Set it to CSS file path or to `undefined` to prevent this warning.
    template.css was updated.
    Prefixing for:  last 1 version
    template-rtl.css was updated.
    Prefixing for:  last 1 version
    font-awesome.css was updated.
    Prefixing for:  last 1 version
    client.css was updated.
    Prefixing for:  last 1 version
    bootstrap.css was updated.
    Prefixing for:  last 1 version
    template.css was updated.
    $

### Alle Dateien eines Verzeichnisses

Sie können alle Dateien in einem Verzeichnis mit dem Befehl
`node build --compile-css path-to-directory` kompilieren. Zum Beispielː
`node build --compile-css templates/cassiopeia/scss`

    $ node build --compile-css templates/cassiopeia/scss
    Prefixing for:  last 1 version
    Without `from` option PostCSS could generate wrong source map and will not find Browserslist config. Set it to CSS file path or to `undefined` to prevent this warning.
    template.css was updated.

### Eine bestimmte Datei

Sie können eine einzelne Datei mit dem Befehl
`node build --compile-css pathtofile` kompilieren. Zum Beispielː
`node build --compile-css templates/cassiopeia/scss/template.scss`

    $ node build --compile-css templates/cassiopeia/scss/template.scss
    Prefixing for:  last 1 version
    Without `from` option PostCSS could generate wrong source map and will not find Browserslist config. Set it to CSS file path or to `undefined` to prevent this warning.
    template.css was updated.

**Hinweis:**

- Die Dateierweiterung .scss muss kleingeschrieben sein.
- Die CSS-Datei muss angelegt werden.

## Kompilieren eigener SCSS-Dateien für Ihr Template

Um SCSS-Dateien für Ihre eigene Vorlage zu kompilieren, muss eine Kopie
des Skripts `JOOMLA4/build/build-modules-js/compilecescss.js` verwendet
werden und an Ihre Systemumgebung angepasst werden.

Alternativ können Sie ein SCSS-Compiler-Plugin verwenden, das Ihre
`.scss` Dateien automatisch beim erneuten Laden der Seite kompiliert. Im
Joomlaǃ Extension Directory JED finden Sie ein Plugin für diesen Zweckː
<a
href="https://extensions.joomla.org/extensions/extension/?searchall=scss&amp;filter%5Btags%5D%5B%5D=&amp;filter%5Bcore_catid%5D=&amp;filter%5Bincludes%5D=&amp;filter%5Bversions%5D=&amp;filter%5Btype%5D=&amp;filter%5Bhasdemo%5D=&amp;filter%5Bnewupdated%5D=&amp;filter%5Bscore%5D=&amp;dir=DESC&amp;limitstart=&amp;controller=filter&amp;view=extension&amp;layout=list&amp;Itemid=145&amp;clearorders=0&amp;clearfilters=1"
class="external text" target="_blank" rel="noreferrer noopener">Joomlaǃ
Extension Directory</a>

Weitere Alternativen und Tools finden Sie auf
<a href="http://sass-lang.com/install" class="external free"
target="_blank"
rel="nofollow noreferrer noopener">http://sass-lang.com/install</a>

Der für den Kern von Joomlaǃ 4.x verwendete SCSS-Compiler ist
<a href="https://github.com/sass/node-sass" class="external text"
target="_blank" rel="nofollow noreferrer noopener">node-sass</a>. Im
Kern von Joomlaǃ finden Sie ihn im Verzeichnis
`JOOMLA4/node_modules/node-sass`.

## Unterschiede zwischen CSS, SCSS und Sass

`SCSS` oder `Sass` vereinfacht die CSS-Schreibweise und definiert
Variablen. Dadurch wird eine übersichtlichere Darstellung, weniger Code
und eine leichtere Überarbeitung durch die Verwendung von Variablen
ermöglicht.

### CSS

CSS-Code schreibt man wie unten, in vollständiger Längeː

        #header {
          margin: 0;
          border: 1px solid blue;
        }
        #header p {
          font-size: 14px;
          color: blue;
        }
        #header a {
          text-decoration: none;
        }

### Sass

Sass ist ein CSS-Präprozessor mit verbesserter Syntax. Stylesheets
werden in der erweiterten Syntax vom Präprozessor abgearbeitet und in
reguläre CSS-Stylesheets umgewandelt.

Es sind zwei mögliche Syntaxen für Sass verfügbar: `SCSS` und `Sass`

#### SCSS

`SCSS` wird im Joomlaǃ Kern verwendet. `SCSS` ist eine Erweiterung der
Syntax von CSS.

        $color:    blue;
        #header {
          margin: 0;
          border: 1px solid $color;
          p {
            color: $colorRed;
            font: {
              size: 14px;
            }
          }
          a {
            text-decoration: none;
          }
        }

#### Sass

Die ältere Syntax von `Sass` erlaubt eine kompaktere Schreibweise von
CSS.

        $color:    blue
        #header
          margin: 0
          border: 1px solid $color
          p
            color: $colorRed
            font:
              size: 14px
          a
            text-decoration: none

Weitere Informationen finden Sie in der <a
href="http://sass-lang.com/documentation/file.SASS_REFERENCE.html#syntax"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">Sass Dokumentation</a>.

## Von vorhandenem CSS zu SCSS / CSS-Dateien importieren

Möglicherweise möchten Sie Ihre bestehenden CSS-Dateien und Klassen zu
Ihrem SCSS-basierten Cassiopeia-Template hinzufügen oder mit dem
beginnen, was Sie schon vorliegen haben. Alle CSS-Deklarationen sind mit
SCSS kompatibel, sodass Sie Ihre `.css` Dateien einfach in `.scss`
umbenennen und sie kompilieren und verwenden können. Sie können dann
Schritt für Schritt die Funktionen nutzen, die SCSS zu bieten hat:

- Erweiterungen der Sprachfunktion wie Variablen, Verschachtelungen und
  Mixins
- Viele nützliche Funktionen zur Manipulation von Farben und anderen
  Werten
- Verbesserte Funktionen wie Kontrollrichtlinien für Libraries
- Gut formatierte, anpassbare Daten-Ausgabe

Siehe
<a href="https://sass-lang.com/" class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://sass-lang.com/</a>

**Hinweis:** Wenn Sie das Template Cassiopeia anpassen möchten, sollte
man das Template kopieren und anschließend anpassen – damit
Aktualisierungen von Joomla! die eigenen Anpassungen nicht
überschreiben.

### Eigene .css-Dateien als .scss-Dateien importieren

Nehmen wir nun an, Sie möchten Ihre angepassten Dateien einbinden und
sie durch den SCSS-Compiler parsen lassen – Sie brauchen Ihre .css-Datei
**nicht** in SCSS umzuschreiben, weil einfaches `.css` auch
funktioniert. Das Einzige, was Sie tun müssen, ist

- umbenennen der `.css` Dateien in `.scss` und
- ein `_` als Präfix voranstellen und
- eine `@import`-Anweisung in die oben erwähnte Datei
  `templates/cassiopeia/scss/template.scss` einfügen

Angenommen, Sie stellen Ihre benutzerdefinierte Datei `mystyles.css`,
die in `_mystyles.scss` umbenannt wurde, in den Ordner
`/templates/cassiopeia/scss`. Jetzt öffnen Sie
`/templates/cassiopeia/scss/template.scss` und am Ende der Datei fügen
Sie mit der @import-Anweisung die `_mystyles.scss` Datei an, sodass sie
die bestehende Deklarationen überschreibt:

    // Bootstrap functions
    @import "../../../media/vendor/bootstrap/scss/functions";


    // Variables
    @import "variables";

    // Flying Focus
    @import "../../../media/vendor/flying-focus-a11y/scss/flying-focus";

    // Bootstrap
    @import "../../../media/vendor/bootstrap/scss/variables";
    @import "../../../media/vendor/bootstrap/scss/bootstrap";

    // FontAwesome
    @import "../../../media/vendor/font-awesome/scss/font-awesome";

    // B/C for Icomoon
    @import "../../../media/system/scss/icomoon";

    // Alert
    @import "../../../media/system/scss/jalert";

    // Blocks
    @import "blocks/global"; // Leave this first
    @import "blocks/alerts";
    @import "blocks/banner";
    @import "blocks/demo-styling";
    @import "blocks/footer";
    @import "blocks/form";
    @import "blocks/frontend-edit";
    @import "blocks/header";
    @import "blocks/icons";
    @import "blocks/iframe";
    @import "blocks/layout";
    @import "blocks/modals";
    @import "blocks/modifiers";
    @import "blocks/utilities";
    @import "blocks/css-grid"; // Last to allow fallback

    // Vendor overrides
    @import "vendor/awesomplete";
    @import "vendor/bootstrap/buttons";
    @import "vendor/bootstrap/card";
    @import "vendor/bootstrap/custom-forms";
    @import "vendor/bootstrap/collapse";
    @import "vendor/bootstrap/dropdown";
    @import "vendor/bootstrap/lists";
    @import "vendor/bootstrap/modal";
    @import "vendor/bootstrap/nav";
    @import "vendor/bootstrap/pagination";
    @import "vendor/bootstrap/table";
    @import "vendor/chosen";
    @import "vendor/dragula";
    @import "vendor/minicolors";
    @import "vendor/tinymce";

    @import "mystyles";

Wenn man nun die Hauptdatei template.scss kompiliert, erhält man am Ende
eine optimierte und verkleinerte Hauptdatei template.css. Außerdem
verringern sich die http-Anfragen und die Seite sollte schneller geladen
werden.

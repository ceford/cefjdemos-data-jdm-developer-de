<!-- Filename: J4.x:SCSS_and_Sass / Display title: Testen von CSS und JavaScript -->

## Einführung

In einer produktiven Joomla-Seite liefert Joomla komprimierte CSS- und Javascript-Dateien (die mit `.min` im Dateinamen). Server senden die gezippten Versionen, wenn sie verfügbar sind (die mit einem `.gz` Suffix enden). Diese Dateien werden aus unkomprimierten Quellen erstellt. Im Fall von CSS-Dateien werden sie oft durch die Verarbeitung von SCSS-Vorläufern erstellt. Zum Testen von Code, der neuen oder aktualisierten CSS- oder JavaScript-Code enthält, ist es notwendig, die CSS-Dateien sowie die komprimierten und minimierten Versionen aus den geänderten Quellen neu zu erstellen.

## Testinstallation

Für Testzwecke benötigen Sie einen Testserver, auf dem Git, Node.js und Composer installiert sind. Gehen Sie zum [Joomla CMS](https://github.com/joomla/joomla-cms) Repository auf GitHub und befolgen Sie die Anweisungen unter *So erhalten Sie eine funktionierende Installation aus dem Quellcode* nahe dem Ende des README-Textes.

Nach Abschluss der Joomla-Installation löschen Sie nicht das Installationsverzeichnis! Dadurch wird auch das `build`-Verzeichnis gelöscht, das zum Neubauen von CSS- und JavaScript-Dateien benötigt wird.

Die wichtigsten `.scss`-Dateien befinden sich in den folgenden Verzeichnissen:

- media/vorlagen/seite/cassiopeia/scss
- media/vorlagen/administrator/atum/scss

Das Skript zur CSS-Erstellung, der SCSS-Compiler und andere ähnliche Build-Tools befinden sich im Verzeichnis `build/`. Das Build-Verzeichnis ist nur aus der Joomlaǃ-Quelle verfügbar, es ist nicht in einer offiziellen Joomlaǃ-Veröffentlichung enthalten.

Die Installationsanweisungen umfassen Folgendes:

```sh
git checkout 5.2-dev (or whatever branch you wish to work with)
composer install
npm ci
```

Der Befehl `npm ci` ist ein Synonym für `npm clean-install` und wird in jeder Situation verwendet, in der sichergestellt werden soll, dass eine saubere Installation der Abhängigkeiten durchgeführt wird.

## Alltäglicher Gebrauch

Es gibt alternative Befehle für Situationen, in denen nur SCSS- oder JavaScript-Dateien geändert wurden:

```sh
npm run build:css¶
    This command compiles SCSS files to CSS and also creates the minified files.

npm run build:js¶
    This command compiles and transpiles the JavaScript files to the correct format and creates minified files.
```

Die Befehle durchforsten die Medien- und Vorlagenverzeichnisse und erstellen die endgültigen Dateiversionen, die für eine Joomla-Installation erforderlich sind.

## Sass, SCSS und CSS

> Sass ist eine Stylesheet-Sprache, die in CSS kompiliert wird. Sie ermöglicht die Verwendung von Variablen, verschachtelten Regeln, Mixins, Funktionen und mehr, alles mit einer vollständig CSS-kompatiblen Syntax. Sass hilft dabei, große Stylesheets gut organisiert zu halten und erleichtert das Teilen von Design innerhalb und über Projekte hinweg. Sass-Dateien haben das Suffix `scss`.

### CSS

CSS-Code wird wie folgt dargestelltː

```css
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
```

### SCSS

`SCSS` ist eine Erweiterung der Syntax von CSS. Es wird im Joomlaǃ-Kern verwendet.

```css
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
```

Ein älterer `Sass`-Syntax bietet eine prägnantere Möglichkeit, CSS zu schreiben.

```css
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
```

Weitere Informationen finden Sie in der [Sass-Dokumentation](http://sass-lang.com/documentation/syntax/).

## Von bestehendem CSS zu SCSS

Wenn Sie das Cassiopeia-Template anpassen möchten, ist es eine gute Idee, das Template zu kopieren und es anschließend anzupassen, damit Joomla!-Updates Ihre Anpassungen nicht überschreiben. Um eigene CSS-Dateien zu einer Kopie des Cassiopeia-Templates hinzuzufügen, benennen Sie einfach Ihre `.css`-Dateien in `.scss` um. Dann können Sie die Funktionen nutzen, die SCSS bietet:

- Sprachliche Erweiterungen wie Variablen, Verschachtelungen und Mixins
- Viele nützliche Funktionen zur Manipulation von Farben und anderen Werten
- Erweiterte Funktionen wie Steuerdirektiven für Bibliotheken
- Gut formatiertes, anpassbares Ausgabeformat

Siehe die [Sass-Website](https://sass-lang.com/) für vollständige Details.

### Importieren Sie Ihre .css als .scss Dateien

Angenommen, Sie möchten Ihre benutzerdefinierten Dateien einbinden und vom SCSS-Compiler parsen lassen - Sie müssen Ihr .css nicht in SCSS umschreiben, da auch einfaches `.css` funktioniert. Was Sie tun müssen:

- Benennen Sie Ihre `.css`-Dateien in `.scss` um
- Fügen Sie ihnen ein `_` als Präfix hinzu
- Fügen Sie am Ende von `media/templates/site/copy_of_cassiopeia/scss/template.scss` eine `@import`-Anweisung hinzu, damit vorhandene Deklarationen überschrieben werden:

```css
// Bootstrap functions
@import "../../../media/vendor/bootstrap/scss/functions";
//...
// 50 or more lines of import statements
// Finally add the cassiopeia colours
:root {
  @each $color, $value in $cassiopeia-colors {
    --#{$color}: #{$value};
  }
// More lines containing scss color statements, then your own styles:
@import "mystyles";
}
```

Wenn Sie nun Ihre main template.scss-Datei kompilieren, erhalten Sie eine optimierte und minimierte main template.css. Sie haben auch Ihre HTTP-Anfragen reduziert und die Seite sollte schneller laden.

*Übersetzt von openai.com*


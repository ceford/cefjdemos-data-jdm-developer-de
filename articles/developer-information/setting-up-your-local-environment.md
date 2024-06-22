<!-- Filename: J4.x:Setting_Up_Your_Local_Environment / Display title: Joomla! an die eigene Umgebung anpassen -->

Tutorial - Wie eine lokale Entwicklungsumgebung für Joomla! 4
eingerichtet wird Joomla!  4.x

Mit Joomla! 4 wurde der Entwicklungsprozess verändert. Es ist nicht
länger möglich, das Repository zu klonen und so Joomla! zu installieren.
Im Folgenden ist nach aktuellen Erfahrungswerten ein funktionaler Weg
zur Installation beschrieben.

## Schnellstart-Anweisungen

Abhängig vom Betriebssystem unterscheidet sich die Einrichtung der
Entwicklungsumgebung. Es ist leider nicht möglich, den Vorgang für die
verschiedenen Betriebssysteme zu dokumentieren. Die bevorzugte
Suchmaschine wird hier weiterhelfen.

### Benötigte Werkzeuge

1.  PHP - im Wesentlichen dasselbe wie beim Betreiben einer
    Joomla!-Seite, allerdings brauchst du eine spezielle Version, die
    PHP CLI (Kommandozeileneingabe) (beachte auch die Seite [Einrichten
    eines LAMPP-Servers für die
    PHP-Entwicklung](https://docs.joomla.org/Configuring_a_LAMPP_server_for_PHP_development "Special:MyLanguage/Configuring a LAMPP server for PHP development"))
2.  Composer - um die Abhängigkeiten von Joomla!s PHP zu verwalten.
    Hilfe bei der Installation von Composer bietet die Dokumentation auf
    <a href="https://getcomposer.org/doc/00-intro.md" class="external free"
    target="_blank"
    rel="nofollow noreferrer noopener">https://getcomposer.org/doc/00-intro.md</a>
3.  Node.js - um JavaScript- und SASS-Dateien für Joomla! zu
    kompilieren - Hilfe bei der Installation von Node.js bieten die
    Anweisungen, die auf
    <a href="https://nodejs.org/en/" class="external free" target="_blank"
    rel="nofollow noreferrer noopener">https://nodejs.org/en/</a>
    erhältlich sind. Hinweis: Für die Installation von Joomla wird
    mindestens NodeJS 12 benötigt.
4.  Git - für die Versionsverwaltung.

### Schritt für Schritt zur Installation der lokalen Entwicklungsumgebung

1.  Das Repository klonen
2.  Zum Branch *4.1-dev* wechseln (checkout)
3.  Die Installation von `composer install` im Stammverzeichnis des
    Git-Repositorys ausführen. (Man kann *--ignore-platform-reqs*
    hinzufügen, wenn man das PHP-LDAP nicht lokal installiert hat und es
    nicht benötigt.)
4.  Im Stammverzeichnis des Git-Repositorys `npm ci` ausführen. (Bitte
    beachten, dass dafür npm 6.13.4 oder höher benötigt wird. Um die
    npm-Version auf die LTS-Version zu aktualisieren, sollte
    `npm install -g npm@lts` ausgeführt werden.)

Linux und OSX Benutzer können den folgenden Bash-Alias nutzen und ihn in
der *~/.bashrc*-Datei setzen:

    alias jclean="rm -rf administrator/templates/atum/css; \
    rm -rf templates/cassiopeia/css; \
    rm -rf administrator/templates/system/css; \
    rm -rf templates/system/css; \
    rm -rf media/; \
    rm -rf node_modules/; \
    rm -rf libraries/vendor/; \
    rm -f administrator/cache/autoload_psr4.php; \
    rm -rf installation/template/css"
    alias jinstall="jclean; composer install; npm ci"

Das wird alle kompilierten Dateien im System löschen und eine neue
Installation über den Befehl `jinstall` innerhalb der
Joomla-Installation starten. Man kann auch den Befehl `jclean`
verwenden, um zu einem Joomla 3.x Branch zurückzuwechseln

## Die etwas längere Startanleitung

Joomla ähnelt vielen anderen Web-Tools dieser Tage. Es hat einen großen
PHP-Anteil und es hat mehr und mehr JavaScript-Code. Während die
PHP-Codierung nicht so viel Vorbereitung benötigt, muss man bei
JavaScript eine Menge Tools verwenden. Der Hauptgrund ist, dass niemand
Code so schreibt, dass ihn jeder Browser versteht, also muss der Code
z.B. von ES6 in eine kompatible Version von JavaScript umgesetzt werden.
Das gleiche gilt für CSS. Für Joomla verwenden wir SASS und das wird in
natives CSS umgewandelt, sodass jeder Browser es versteht. Auf der
anderen Seite ist das Einrichten einer Entwicklungsumgebung etwas
komplizierter, aber das Instrumentarium macht das Programmieren
bequemer. Dank der Wächter und dem automatischen Reload des Browsers,
können Änderungen in Echtzeit verfolgt werden.

### PHP

Es sollte reichen, `composer install` auszuführen. Dies wird die
PHP-Abhängigkeiten, die in der Datei composer.lock gespeichert sind,
installieren. Man kann dies beliebig oft wiederholen, es werden nur neue
Pakete installiert, wenn die composer.lock Datei verändert wird. Bitte
`composer update` nicht ausführen, da dadurch alle Pakete auf neuere
Versionen und die composer.lock Datei aktualisiert werden.

**Hinweis:** Möglicherweise muss `composer install` mit der Option
`--ignore-platform-reqs` ausgeführt werden, um die in Composer
angegebenen Plattformanforderungen zu ignorieren. Das gilt, falls die
PHP-LDAP-Erweiterung nicht installiert ist.

### Node/npm Skripte

Node.js wird mit dem Paketmanager „NPM“ ausgeliefert (gewissermaßen
vergleichbar zu Composer). NPM hat einen `run`-Befehl und wir haben
einige Skripte vorbereitet, um die Arbeit zu erleichtern. Die Befehle
müssen im Stammverzeichnis des Repositorys ausgeführt werden, sobald JS-
oder SASS-Dateien geändert wurden. Zuvor muss man einmalig `npm ci`
ausführen, um die Abhängigkeiten zu installieren.

#### npm run build:css

Kompiliert SASS Dateien nach CSS und erzeugt die komprimierten Dateien.

#### npm run build:js

Es kompiliert und übersetzt die JavaScript-Dateien in das richtige
Format und erstellt sogenannte Minified-Dateien.

#### npm run watch

Dieser Befehl entspricht dem Befehl `build:js`, erkennt jedoch
Änderungen und erstellt automatisch aktualisierte Dateien im
Medienverzeichnis. SASS-Dateien sind noch nicht enthalten.

#### npm run lint:js

Damit wird eine Syntaxprüfung aller ES6-JavaScript-Dateien gegen den
JavaScript-Codestandard durchgeführt (für weitere Informationen zum
Joomla 4-Codestandard, bitte das <a
href="https://developer.joomla.org/coding-standards/introduction.html%7C"
class="external text" target="_blank"
rel="noreferrer noopener">Codestandard-Handbuch</a> lesen).

#### npm run test

Dies wird eine JavaScript Testsuite ausführen.

## Mögliche Probleme

Beim Installieren des Composers kann es zu folgenden Fehlern kommen:

    Problem 1
        - Installation request for joomla/ldap 2.0.0-beta -> satisfiable by joomla/ldap[2.0.0-beta].
        - joomla/ldap 2.0.0-beta requires ext-ldap * -> the requested PHP extension ldap is missing from your system.
    Problem 2
        - Installation request for symfony/ldap v5.1.5 -> satisfiable by symfony/ldap[v5.1.5].
        - symfony/ldap v5.1.5 requires ext-ldap * -> the requested PHP extension ldap is missing from your system.

Die Lösung ist, die Composer-Installation mit der Option
`--ignore-platform-reqs` auszuführen, um die in Composer angegebenen
Plattformanforderungen zu ignorieren. Das heißt, wenn die
LDAP-Erweiterung von PHP nicht installiert ist.

    composer install --ignore-platform-reqs

Wenn ein Anmeldefehler, wie unten abgebildet, angezeigt wird, sollte die
Datei `administrator/cache/autoload_psr4.php`, wie im zweiten Bild
gezeigt, gelöscht werden.

<img
src="https://docs.joomla.org/images/thumb/b/b3/Install-error.png/400px-Install-error.png"
class="thumbborder" decoding="async"
srcset="https://docs.joomla.org/images/thumb/b/b3/Install-error.png/600px-Install-error.png 1.5x, https://docs.joomla.org/images/thumb/b/b3/Install-error.png/800px-Install-error.png 2x"
data-file-width="1920" data-file-height="1080" width="400" height="225"
alt="Login After Install Error" />

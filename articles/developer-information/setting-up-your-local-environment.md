<!-- Filename: J4.x:Setting_Up_Your_Local_Environment / Display title: Einrichten einer lokalen Umgebung -->

## Schnellanleitung

Für eine Testeinrichtung benötigen Sie einen Software-Stack, der Apache, MySQL und PHP umfasst. Die erforderlichen Schritte sind an anderer Stelle in diesem Handbuch behandelt. Im Zweifelsfall nutzen Sie einen für Ihre Plattform geeigneten Software-Stack.

## Zusätzliche Werkzeuge

Für das Testen von Joomla Pull Requests und das Beitragen zum Joomla-Core-Code benötigen Sie zusätzliche Werkzeuge, die alle einfach installiert werden können, oft mit einem Einzeiler:

1. **Composer** - zum Verwalten von Joomlas PHP-Abhängigkeiten. Lesen Sie die [Composer-Dokumentation](https://getcomposer.org/doc/00-intro.md) für Hilfe bei der Installation.
2. **Node.js** - zum Kompilieren von Joomlas JavaScript- und SASS-Dateien. Lesen Sie die [Node.js-Dokumentation](https://nodejs.org/en/) für Hilfe bei der Installation.
3. **Git** - für das Versionsmanagement. Lesen Sie die [Git-Dokumentation](https://git-scm.com/) für Hilfe bei der Installation.

## Schritte zur Einrichtung einer lokalen Testseite

1. Gehen Sie zum [Joomla! Repository auf GitHub](https://github.com/joomla/joomla-cms).
2. Wählen Sie den Joomla-Zweig aus, an dem Sie arbeiten möchten. Dadurch werden die entsprechenden README-Anweisungen ausgewählt.
3. Scrollen Sie auf der Seite nach unten zum Abschnitt **Schritte zur Einrichtung der lokalen Umgebung:** des README-Textes.
4. Befolgen Sie die dort aufgeführten Schritte. Sie können den Namen des geklonten joomla-cms-Ordners ändern, sodass Sie so viele Klone wie gewünscht für verschiedene Zwecke erstellen können.
5. Erstellen Sie eine Datenbank und installieren Sie Joomla, aber löschen Sie den Installationsordner am Ende des Installationsprozesses nicht.

## Aktualisierung von Joomla

Von Zeit zu Zeit müssen Sie möglicherweise einen Joomla-Klon aktualisieren. Dies ist ein Einzeilenbefehl, der normalerweise schnell ist. Es ist jedoch in der Regel notwendig, die CSS- und JavaScript-Dateien neu zu erstellen, was ein komplizierterer und längerer Prozess ist.

Linux- und OSX-Benutzer können das folgende Bash-Alias einrichten, indem sie Folgendes in die *~/.bashrc-Datei* einfügen:

```sh
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
```

Dadurch werden alle kompilierten Dateien gelöscht und eine Neuinstallation als ein Befehl ausgeführt, indem `jinstall` innerhalb Ihrer Joomla-Installation aufgerufen wird. Sie können auch den Befehl `jclean` verwenden, um zu einem anderen Joomla-Zweig zurückzukehren.

**Hinweis:** Möglicherweise müssen Sie `composer install` mit der Option `--ignore-platform-reqs` ausführen, um die in Composer angegebenen Plattformanforderungen zu ignorieren. Das ist der Fall, wenn Sie die PHP-LDAP-Erweiterung nicht installiert haben.

### Datenbankänderungen

Wenn ein Joomla-Update Datenbankänderungen beinhaltet, müssen Sie möglicherweise die einzelnen Änderungen finden und manuell ausführen oder mit einem neuen Klon neu beginnen.

## Node/npm-Skripte

Die Installation von Joomla aus einem Repository-Klon erfolgte mit zwei Befehlen:

- **composer install** Installieren Sie alle benötigten Composer-Pakete.
- **npm ci** Installieren Sie alle benötigten npm-Pakete.

Node.js kommt mit einem Paketmanager namens NPM, der einen `run` Befehl hat. Einige Skripte stehen zur Verfügung, um den Build-Prozess zu beschleunigen, wenn nur CSS- oder JavaScript-Dateien geändert wurden.

### npm run build:css

Dieser Befehl kompiliert SASS-Dateien zu CSS und erstellt außerdem die minimierten Dateien.

### npm run build:js

Dieser Befehl kompiliert und transpiliert die JavaScript-Dateien in das richtige Format und erstellt minimierte Dateien.

### npm run watch

Dieser Befehl ist derselbe wie der `build:js` Befehl, überwacht jedoch Änderungen und erstellt automatisch aktualisierte Dateien im Medienverzeichnis. SASS-Dateien sind noch nicht enthalten.

### npm run lint:js

Dieser Befehl führt eine Syntaxprüfung für alle ES6-JavaScript-Dateien gemäß dem JavaScript-Code-Standard durch. Weitere Informationen finden Sie im [Joomla Coding Standards Handbuch](https://developer.joomla.org/coding-standards/introduction.html).

### npm run test

Dieser Befehl wird eine JavaScript-Testsuite ausführen.

## Mögliche Probleme

Beim Ausführen von composer install können Sie auf diese Fehler stoßen

```sh
    Problem 1
        - Installation request for joomla/ldap 2.0.0-beta -> satisfiable by joomla/ldap[2.0.0-beta].
        - joomla/ldap 2.0.0-beta requires ext-ldap * -> the requested PHP extension ldap is missing from your system.
    Problem 2
        - Installation request for symfony/ldap v5.1.5 -> satisfiable by symfony/ldap[v5.1.5].
        - symfony/ldap v5.1.5 requires ext-ldap * -> the requested PHP extension ldap is missing from your system.
```

Die Lösung besteht darin, den Composer-Installationsbefehl mit der Option `--ignore-platform-reqs` auszuführen, um die in Composer angegebenen Plattformanforderungen zu ignorieren. Das heißt, wenn Sie die LDAP-Erweiterung von PHP nicht installiert haben.

```sh
    composer install --ignore-platform-reqs
```

Wenn Sie einen Anmeldefehler erhalten, löschen Sie die Datei `administrator/cache/autoload_psr4.php`.

*Übersetzt von openai.com*


<!-- Filename: J4.x:Developer:_Required_Software / Display title: Erforderliche Software -->

## Einführung

Dieses Tutorial richtet sich an Neulinge in der Joomla-Code-Entwicklung, die Erweiterungen auf einem lokalen Computer vorbereiten möchten. Es spielt keine Rolle, ob Sie Windows, Mac oder Linux verwenden. Alle erforderlichen Softwarepakete sind für all diese Plattformen verfügbar. Um auf Ihrem eigenen Laptop oder Desktop-Computer loszulegen, der normalerweise den Domainnamen *localhost* hat, müssen Sie ein standardmäßiges Set aus separaten Softwareelementen installieren.

- **Apache**, ein Webserver. Es gibt auch andere Webserver, aber Neulinge sollten bei Apache bleiben.
- **MySQL** oder **MariaDB**, Datenbankserver. PostgreSQL wird ebenfalls unterstützt, ist aber nicht für Neulinge geeignet.
- **PHP**, die neueste von Joomla empfohlene Version oder die Mindestversion für Ihre Plattform.
- **phpMyAdmin**, ein Tool zur Verwaltung von MySQL- und MariaDB-Datenbanken.
- **xDebug**, eine Erweiterung für PHP, die zum Debugging verwendet wird.
- Eine IDE wie **VSCode**. Andere sind verfügbar und werden in einem separaten Beitrag behandelt.

## Software-Stacks

Die ersten vier Elemente in dieser Liste werden oft als Stack bezeichnet und können LAMP, MAMP oder WAMP genannt werden, wobei die Buchstaben im Akronym Folgendes bedeuten:

- **L, M oder W** Plattform. L für Linux, M für Mac und W für Windows.
- **A: Apache** Webserver.
- **M: MySQL oder MariaDB** Datenbank. Die beiden sind austauschbar.
- **P: PHP** Skriptsprache. Eine weit verbreitete Skriptsprache. Es gibt keine Alternative, da Joomla in PHP kodiert ist.

## Verpackte Beiträge

Ein guter Weg, um anzufangen, ist die Verwendung eines Pakets, das die wesentliche Software kombiniert:

- **WAMP** für Windows ist kostenlos von der [Wampserver](https://www.wampserver.com/en/) Seite erhältlich.
- **Bearsampp** für Windows ist kostenlos von der [Bearsampp](https://bearsampp.com/) Seite erhältlich. Es bietet mehr Werkzeuge.
- **XAMPP** für Windows, Mac und Linux ist kostenlos von der [Apache Friends](https://www.apachefriends.org/) Seite erhältlich. Es gibt ein lokales Tutorial für [XAMPP](jdocmanual?article=user/hosting/local-hosting-with-xampp).
- **MAMP** für Mac und Windows ist in kostenlosen und kommerziellen Versionen von der [MAMP](https://www.mamp.info/en/mac/) Seite erhältlich.

## Keine Stacks

Wenn Sie einen Linux- oder Macintosh-Computer haben, werden Sie feststellen, dass Sie jedes der erforderlichen Software-Elemente unabhängig von den externen Repositories, die Ihr Betriebssystem unterstützen, installieren können. Es ist möglich, dass sie bereits installiert und einsatzbereit sind. Um dies zu testen, öffnen Sie Ihren bevorzugten Webbrowser und geben Sie **localhost** in die URL-Leiste ein. Sie werden eine Platzhalterseite oder eine Fehlerseite zur Verbindung sehen.

## Webserver-Dokumentstammverzeichnis

Bei der Installation hat Ihr Apache-Server ein standardmäßiges Dokumentstammverzeichnis festgelegt. Der Standort variiert je nach Plattform und Sie müssen entweder wissen, wo es sich befindet, oder einen virtuellen Host erstellen, um es dort zu platzieren, wo Sie es möchten. Beispiel für Standardstandorte:

- Mac OS: "/Library/WebServer/Documents"
- Linux: /var/www/html
- Windows: ...

Um spätere Probleme mit Dateiberechtigungen zu vermeiden, ist es oft sinnvoll, einen virtuellen Host zu erstellen, der auf das Verzeichnis public_html Ihres eigenen Dateispeichers verweist. Das könnte /home/username/public_html auf Linux oder /Users/username/Sites auf Mac sein.

Dies ist ein Beispiel für einen virtuellen Mac-Host-Eintrag in der Datei /etc/apache2/vhosts/localhost.conf:

```bash
<VirtualHost *:80>
        DocumentRoot "/Users/username/Sites"
        ServerName localhost
        ErrorLog "/private/var/log/apache2/localhost-error_log"
        CustomLog "/private/var/log/apache2/localhost-access_log" common
        <Directory "/Users/username/Sites">
            AllowOverride All
            Require all granted
        </Directory>
</VirtualHost>
```

Alternativ können Sie möglicherweise einen symbolischen Link vom standardmäßigen Dokumentenstamm zum public_html-Ordner Ihres persönlichen Dateispeichers erstellen.

*Übersetzt von openai.com*

Bitte übersetzen Sie den folgenden Markdown-Text aus dem Englischen ins Deutsche. Bitte verwenden Sie das Wort Beiträge anstelle von Artikel.


<!-- Filename: Setting_up_Eclipse_PDT_2020_and_Git_for_Pulls / Display title: Einrichten von Eclipse PDT 2020 und Git für Pull-Anfragen -->

## Einführung

Ich begann vor vielen Jahren erstmals Eclipse für ein privates Joomla-Projekt und ein privates Remote-Git-Repository, das nicht auf Github war, zu nutzen. Ich las die verfügbaren Tutorials und arbeitete glücklich vor mich hin, bis ich dieses Jahr versuchte, bei den Kern-Tests und der Behebung von Bugs für Joomla! 4 zu helfen. Und dann einige Pull-Requests. Da wurde mir klar, dass ich nicht genau verstanden hatte, was ich tat. Schließlich beschloss ich, von vorne zu beginnen und den Prozess für andere halb-erfahrene Entwickler zu dokumentieren, die neu bei Joomla sind, insbesondere die Teile, die ich beim ersten Mal nicht vollständig erfasst hatte. Der erste Punkt auf der Liste:

## Dateistruktur

Es ist nicht offensichtlich, bevor Sie Eclipse herunterladen und installieren, dass es mindestens zwei Orte gibt, an denen Daten gespeichert werden, abgesehen von dem Ort, an dem sich der Eclipse-Code befindet.

### Der Arbeitsbereich

Dies ist der Ort, an dem Eclipse seine eigenen Daten für jedes Projekt speichert. Es sollte nicht im Web-Verzeichnisbaum sein! Da ich meistens auf einem Mac-Laptop arbeite, ist der Standardstandort für mich **/Users/username/eclipse-workspace**. Es gibt einen Unterordner für jedes Projekt. So könnte einer **/Users/username/eclipse-workspace/joomla-cms** sein. Er enthält einen Ordner: .metadata. Linux-Nutzer werden wahrscheinlich wissen, dass sie "home" anstelle von "Users" verwenden müssen.

### Das lokale Git-Repository und die Test-Website

Hier wird der Projektcode gespeichert. Er könnte im lokalen Webverzeichnis sein, wenn Sie ihn zum Testen verwenden möchten. Für mich ist es **/Users/username/Sites/joomla-cms-4**. Linux-Benutzer wissen wahrscheinlich, dass sie public_html anstelle von Sites verwenden müssen. Achten Sie auf die zusätzlichen Schritte, die erforderlich sind, damit das lokale Repository als Joomla-Seite funktioniert.

### Eine separate Test-Website

Dies ist optional, erfordert jedoch, dass das Git-Repository eine benutzerdefinierte build.xml-Datei hat, zum Beispiel build-local.xml, wie im Anhang beschrieben.

## Erforderliche Software

Um eine funktionierende Entwicklungs- und Test-Website einzurichten, müssen Sie zunächst die folgende Software installieren:

- Apache
- MySQL oder Maridb
- PHP
- phpMyAdmin - [PhpMyAdmin Startseite](https://www.phpmyadmin.net/)
- Composer - [Composer Startseite](https://getcomposer.org/)
- Node.js - [Node.js Startseite](https://nodejs.org/en/)
- Eclipse PDT - [Eclipse PHP-Entwicklungstools](https://www.eclipse.org/pdt/)
- git (optional für Befehlszeilen-git-Nutzer) - [git Startseite](https://git-scm.com/)
- phing (optional für benutzerdefinierte Builds) - [phing Startseite](https://www.phing.info/)

Die ersten drei oder vier kommen oft als Paket für Ihre Plattform, bekannt als LAMP-, WAMP- oder XAMP-Stack. Verwenden Sie einfach das, was Ihre Plattform bietet, oder schauen Sie sich Apache Friends [XAMPP](https://www.apachefriends.org/) an.

Lassen Sie uns annehmen, dass Sie alles außer Eclipse installiert haben.

## Fork joomla-cms auf Github

Es gibt einen Workflow, der in [Mein erster Pull-Request zu Joomla! auf Github](https://docs.joomla.org/My_first_pull_request_to_Joomla!_on_Github) beschrieben ist, den ich nicht genug loben kann. Er zeigt genau, was Sie tun müssen:

![Github-Arbeitsablauf](../../../en/images/getting-started/core-work-flow-joomla.png)

Schritte

- Gehe zu [Github](https://github.com/) und erstelle ein Konto, kostenlos und schnell.
- Gehe in deinem Github-Konto zum joomla/joomla-cms Repository: Tippe joomla/joo... in das Suchfeld oben links und wähle, wenn die Optionen erscheinen.
- Klicke im joomla/joomla-cms Repository auf die Fork-Schaltfläche oben rechts. Das gibt dir eine geforkte Kopie des gesamten Codes für Joomla 4, Joomla 5, ..., alles, in deinem eigenen Github-Konto.

Zurück zu deinem Arbeitsplatz.

## Eclipse installieren

Ich habe bereits zwei Versionen von Eclipse installiert, Oxygen von vor ein paar Jahren und Cocoa von 2020. Ab hier installiere ich eine zweite Instanz von Cocoa. Mal sehen, was passiert:

- Gehe zur [Eclipse-Seite](https://www.eclipse.org/pdt/) und lade die Version für deine Plattform herunter.
- Folge der Installationsanleitung und starte schließlich die Eclipse-Anwendung, bei mir **Eclipse 2.app**.
- Warnung: *Eclipse 2.app* ist eine App, die aus dem Internet heruntergeladen wurde. Sind Sie sicher, dass Sie sie öffnen möchten? **Öffnen**
- Wähle ein Verzeichnis als Arbeitsbereich - Standard ist /Users/username/eclipse-workspace. **Durchsuchen** und erstelle einen joomla-cms-4 Unterordner.
- Starten: Eclipse ist installiert und zeigt die Willkommensseite an.
- Überprüfe die IDE-Konfigurationseinstellungen. Setze die Punkte 1, 2, 5 und 6.
- Wähle das Workbench-Symbol oben rechts aus.

Anstatt eine andere Version von Eclipse zu installieren, hätte ich einen neuen leeren Arbeitsbereich öffnen können.

Bisher so gut!

## Fork in Eclipse importieren

In Ihrer neuen lokalen Installation von Eclipse:

- Öffnen Sie Einstellungen und setzen Sie Team / Git / Standard-Repository-Ordner auf /Users/username/git (Sie können diesen Speicherort verwenden oder nicht)
- Öffnen Sie Datei / Importieren / Git / Projekte von Git (mit Smart Import). Weiter ...
- URI klonen. Weiter ...
- Kopieren Sie die URL-Leiste von **Ihrem Github-Fork** und fügen Sie sie in das URL-Feld ein. Sie müssen keine Authentifizierungsdaten hinzufügen. Weiter ...
- Zu importierende Zweige ... ähm ... Wahrscheinlich am besten, alles zu importieren. Weiter ...
- Verzeichnis. Hier könnten Sie einen anderen Speicherort wählen, um den Code zu speichern. Ich habe nach /Users/username/public_html/joomla-cms-4 gesucht und diesen, falls erforderlich, erstellt.
- Initialer Zweig - wenn Sie alle Zweige importiert haben. Ich arbeite an Joomla 4, also habe ich 4.0-dev ausgewählt. Und ich habe bemerkt, dass mein Klon **origin** sein wird. Weiter ...
- Ein Schluck Kaffee. Das Klonen wird ein paar Minuten dauern.
- Quellcode importieren. Die Quelle sollte der Code-Speicherort sein. Fertigstellen.
- Eclipse zeigt jetzt die Wurzel des Code-Baums im Projekt-Explorer-Bereich an. Klicken Sie, um mehr vom Baum zu sehen. Beachten Sie, dass dieser Code noch nicht als Joomla-Seite funktionieren wird. Unter anderem gibt es keinen Medienordner.

## Original joomla-cms-Repository zu Eclipse hinzufügen

- Zeige das Fenster Git-Repositories an: Wähle Fenster / Ansicht anzeigen / Andere
- Wähle Git / Git-Repositories - das Fenster erscheint unten auf dem Bildschirm.
- Erweitere joomla-cms / Remotes / origin - wenn du Änderungen an deinem Code vornimmst und zu origin pushst, ist dies der Ort, an den es geht.
- Klicke mit der rechten Maustaste auf Remotes und wähle Remote erstellen...
- Stelle den Remotenamen auf **joomla-origin** und wähle **Abrufen konfigurieren**.
- In Abrufen konfigurieren wähle **Ändern**.
- In URI auswählen füge die URL des ursprünglichen joomla-cms Repositorys ein: `https://github.com/joomla/`
- Lass Anmeldeinformationen leer. Fertigstellen.
- in Abrufen konfigurieren: **Speichern und abrufen**.

## Erstellen Sie eine funktionierende Website

Ihre Kopie des joomla-cms-Codes benötigt weitere Schritte, um als Website verwendbar zu sein.

- Öffnen Sie ein Terminal und wechseln Sie in den Ordner, der Ihren geklonten Code enthält.
- Führen Sie composer install aus:
  - Linux- und OSX-Nutzer können den folgenden Bash-Alias einrichten, indem sie das Folgende in die `~/.bash_profile` oder `~/.zsh` einfügen (`\$ source ~/.bash_profile`, um es sofort zu aktivieren):
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
    alias jinstall="jclean; composer install --ignore-platform-reqs; npm ci"
```
- Composer ist nicht in meinem Pfad, also habe ich php ~/composer/composer.phar ersetzt.
- jinstall
- das alle Joomla PHP-Abhängigkeiten, Javascript-Abhängigkeiten abruft, alle ES6-Javascript kompiliert und die Dateien an den entsprechenden Orten ablegt.
- ein Schluck Kaffee, während die Abhängigkeiten heruntergeladen und Mediendateien erstellt werden.
- In Eclipse mit der rechten Maustaste auf das Projekt-Root klicken und Aktualisieren wählen. Sie werden sehen, dass Ihr Code jetzt einen Medienordner enthält.
- Wenn Sie interessiert sind, verwenden Sie einen Dateimanager, um zu sehen, dass das Root-Verzeichnis auch eine Datei namens .gitignore enthält und der Medienordner darin erwähnt wird. Das bedeutet, dass er nicht in Ihre lokalen oder entfernten geforkten Git-Repositories aufgenommen wird.

Sie sind nun bereit für eine Joomla-Installation:

- Erstellen Sie eine Datenbank mit phpMyAdmin (oder der MySQL-Befehlszeile, wenn Sie dies bevorzugen).
- Ich habe eine Datenbank namens joomla-cms-4 mit der Kollation utf8mb4-unicode-ci erstellt.
- Erstellen Sie einen neuen Benutzer: Mein Benutzername ist jcms4 mit einem generierten zufälligen Passwort (GAOC26r77bBLkkdA). Sie müssen das Passwort notieren, um es während der Installation zu verwenden. Es wird im Klartext in der Konfigurationsdatei gespeichert.
- Gewähren Sie diesem Benutzer alle Privilegien auf der Datenbank - die Standardeinstellung.
- Gehen Sie in Ihrem bevorzugten Browser zum Root der neuen Seite: localhost/joomla-cms-4/

### Auf meinem Mac mit macOS Catalina

- Umgebungseinrichtung unvollständig: Weitere Details. Bah - beheben Sie es einfach und überprüfen Sie, ob die URL-Leiste die von Ihnen eingegebene URL enthält - bei mir wurden irgendwie zusätzliche Zeichen angehängt.
- Andernfalls führt es zu einer funktionierenden Seite - löschen Sie nicht den Installationsordner.

### Auf meinem Linux-Arbeitsplatz mit Linux Mint 20

Hoppla! Etwas ist schiefgelaufen!

Der Haken ist, dass ich meinen Joomla-Code in meinem persönlichen Dateibereich für Lese-/Schreibzugriff durch Eclipse habe. Der Webserver benötigt ebenfalls Schreibzugriff, um Konfigurations-, Cache- und Protokolldateien zu schreiben, wird jedoch als Benutzer und Gruppe mit niedrigen Berechtigungen ausgeführt. Da ich mich in einem privaten Heimnetzwerk befinde, habe ich /etc/apache2/apache2.conf bearbeitet, um User \${APACHE_RUN_USER} und Group \${APACHE_RUN_GROUP} auszukommentieren und User myusername und Group mygroupname hinzuzufügen. Apache neu starten, dann...

- Es führt zu einer funktionierenden Seite - löschen Sie nicht den Installationsordner.

## Code-Überarbeitung

Kurz gesagt:

- Von joomla-origin abrufen, um sicherzustellen, dass mein lokaler Klon auf dem neuesten Stand ist.
- Team / Zusammenführen / Zweig zum Zusammenführen auswählen - Vorsicht - Ich habe joomla-origin/4.0-dev ausgewählt.
- Auf origin pushen, um sicherzustellen, dass mein Remote-Fork auf dem neuesten Stand ist.
- Einen Zweig erstellen für einige Codeänderungen, die ich vornehmen möchte.
- Die Codeänderungen vornehmen.
- Wenn die Änderungen an CSS- oder JS-Quelldateien (diese in sass oder es6) sind, ein Terminalfenster öffnen und jinstall erneut ausführen.
- Testen Sie Ihre lokale Installation, um festzustellen, ob es Probleme gibt.
- Die Codeänderungen committen.
- Auf origin pushen, um meinen Remote-Fork mit meinen Änderungen zu aktualisieren.
- Gehen Sie zu Ihrem eigenen Konto auf Github und wählen Sie den Zweig aus, den Sie mit dem aktualisierten Code erstellt haben. Etwas Beängstigendes: Es heißt **Dieser Zweig ist 11084 Commits voraus, 134 Commits hinter joomla:staging.** Habe ich etwas falsch gemacht? Anscheinend nicht!
- Wählen Sie die Schaltfläche Pull Request. Stellen Sie sicher, dass Sie den richtigen joomla-Zweig zum Zusammenführen ausgewählt haben. Für mich ist dies 4.0-dev. Und stellen Sie sicher, dass Sie Ihren Zweig mit dem geänderten Code ausgewählt haben. Los geht's!
- Der Pull-Request muss getestet und genehmigt werden, was Tage, Wochen oder Monate dauern kann. Und die Änderung kann abgelehnt werden!

## Inzwischen

Zurück an Ihrem Arbeitsplatz:

- Wechsle zurück zu deinem ursprünglichen Branch, für mich: Team / Wechsel zu / 4.0-dev
- Neu erstellen: für mich Projekt / Projekt erstellen
- Sieh zu, wie die zuvor geänderten Dateien erneut auf deine Arbeitsseite kopiert werden.
- TEST: Deine Arbeitsseite ist wieder so, wie sie vor deinen Codeänderungen war.

Sie sind nun bereit, einen weiteren Branch für ein weiteres Set von Codeänderungen zu erstellen.

## Wenn eine Katastrophe eintritt

In einem Stadium wurde mein lokaler Klon irgendwie beschädigt, und ich hatte keine Ahnung, wie ich ihn reparieren sollte. Also löschte ich meinen lokalen Klon und alle zugehörigen Dateien, leerte die Datenbank und kehrte dann zum oben genannten **Fork in Eclipse importieren** Schritt zurück. Das brachte meinen lokalen Klon in Einklang mit meinem entfernten Fork, einschließlich aller Zweige, die ich für Pull-Anfragen erstellt hatte. Die neue Installation funktionierte reibungslos und ich war glücklich!

## Weitere Ressourcen

- [Konfiguration von Eclipse für die Joomla-Entwicklung](https://docs.joomla.org/Configuring_Eclipse_for_joomla_development) (2012-2013) Aber holen Sie sich die neueste Eclipse PDT-Version!
- [Mein erster Pull-Request an Joomla! auf Github](https://docs.joomla.org/My_first_pull_request_to_Joomla!_on_Github) (2011-2014) Guter Überblick, wenn auch etwas veraltet.
- [Arbeiten mit git und github](https://docs.joomla.org/Working_with_git_and_github) (2011-2015)
- [Einrichten Ihrer lokalen Umgebung](https://docs.joomla.org/J4.x:Setting_Up_Your_Local_Environment) (2018-2020)
- [Konfiguration von Eclipse und Xdebug](https://docs.joomla.org/Configuring_Eclipse_and_Xdebug) (2013) Alles über Debugging.
- [Arbeiten mit Git und Eclipse](https://docs.joomla.org/Working_with_Git_and_Eclipse) (2014) Methode für alte Ausgaben von Eclipse.
- [Automatisierte Tests von Eclipse ausführen](https://docs.joomla.org/Running_Automated_Tests_from_Eclipse) (2020) Für fortgeschrittene Benutzer?
- [Xdebug für PHP-Entwicklung/Linux konfigurieren](https://docs.joomla.org/Configuring_Xdebug_for_PHP_development/Linux) (2016) Installieren und konfigurieren.
- [Eclipse IDE für PHP-Entwicklung/Linux konfigurieren](https://docs.joomla.org/Configuring_Eclipse_IDE_for_PHP_development/Linux) (2019) Für Linux installieren und konfigurieren.
- [Xdebug für PHP-Entwicklung konfigurieren](https://docs.joomla.org/Configuring_Xdebug_for_PHP_development) (2014) Schritte für Linux, Windows und Mac OS X.
- [Webinar: Eclipse für Joomla!-Entwicklung verwenden](https://docs.joomla.org/Webinar:_Using_Eclipse_for_Joomla!_Development) (2014) Dieses 45-minütige Video-Webinar, aufgenommen am 30. April 2009, bietet einen Überblick über Eclipse-Funktionen für die Joomla!-Entwicklung.
- [Einrichten Ihrer Workstation für die Joomla-Entwicklung](https://docs.joomla.org/Setting_up_your_workstation_for_Joomla_development) (2020) Kurzer Überblick über die erforderliche Software und alternative IDEs.

## Anhang

Zu einem Zeitpunkt hatte ich mein lokales Git-Repository außerhalb meines Webroots und musste alle Änderungen, die ich vorgenommen hatte, auf eine separat installierte lokale Seite kopieren. Dafür benötigte ich eine benutzerdefinierte Build-Datei, die hier als Referenz gezeigt wird:

### Erstellen Sie eine build-local.xml Datei

Der Eclipse-Klon enthält eine build.xml-Datei, aber diese wird verwendet, um eine herunterladbare Zip-Datei für eine neue Installation zu testen und zu erstellen. Was ich tun möchte, ist, alle Änderungen, die ich an meinem geklonten Code vornehme, auf meine Testseite auf meinem Laptop zu kopieren. Beachten Sie, dass ich nur Änderungen am PHP-Code vornehmen möchte und nicht an Javascript oder CSS. Dafür habe ich eine separate Datei namens build-local.xml im Stammverzeichnis des Projekts erstellt:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="joomla-cms" basedir="." default="main">
    <property file=".project" />

    <property name="joomladir" value="/Users/username/public_html/joomla-cms"  override="true" />

    <property name="srcdir" value="${project.basedir}" override="true" />

    <!-- Fileset for all files -->
    <fileset dir="${srcdir}" id="allfiles">
        <include name="administrator/**" />
        <include name="api/**" />
        <include name="cli/**" />
        <include name="components/**" />
        <include name="images/**" />
        <include name="includes/**" />
        <include name="language/**" />
        <include name="layouts/**" />
        <include name="libraries/**" />
        <include name="modules/**" />
        <include name="plugins/**" />
        <include name="templates/**" />
        <include name="index.php" />

        <exclude name="**/.*" />
    </fileset>

    <!-- ============================================  -->
    <!-- (DEFAULT) Target: main                        -->
    <!-- ============================================  -->
    <target name="main" description="main target">
        <copy todir="${joomladir}">
            <fileset refid="allfiles" />
        </copy>
    </target>
</project>
```

### Ein Build-Tool hinzufügen

- Klicken Sie mit der rechten Maustaste auf das Projektverzeichnis und wählen Sie Eigenschaften.
- Wählen Sie Ersteller / Neu / Programm / OK.
- Name: Lokales Build mit phing
- Ort: Wo auch immer phing installiert ist. Für mich ist es /usr/local/php5/bin/phing, obwohl ich PHP 7.4 verwende.
- Arbeitsverzeichnis: Arbeitsbereich durchsuchen und joomla-cms auswählen - wird als \${workspace_loc:/joomla-cms} angezeigt.
- Argumente: -f build-local.xml
- Anwenden / OK
- In Erstellern: Anwenden und Schließen
- Projekt / Projekt erstellen
- Sehen Sie, was passiert:

```sh
    Buildfile: /Users/username/git/joomla-cms/build-local.xml
     [property] Loading /Users/username/git/joomla-cms/.project

    joomla-cms > main:

    BUILD FINISHED

    Total time: 0.2121 seconds
```

*Übersetzt von openai.com*

<!-- Filename: J4.x:CLI_Update / Display title: CLI Update -->

<span id="main-portal-heading">GSoC 2018
CLI Update
Dokumentation</span> [<img
src="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/75px-Gsoc2016.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/113px-Gsoc2016.png 1.5x, https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/150px-Gsoc2016.png 2x"
data-file-width="373" data-file-height="373" width="75" height="75"
alt="Gsoc2016.png" />](https://docs.joomla.org/GSOC_2018 "GSOC 2018")
Joomla!  4.x

## Einleitung

Das Ausführen einiger kleinerer Aufgaben auf dem
Joomla!-Administrator-Dashboard kann umständlich und langsam erscheinen.
Das CLI-Update bietet eine schnellere Möglichkeit, einige dieser
Operationen über die Befehlszeile auszuführen. Es bietet eine
Möglichkeit, Dinge schneller in die Joomla!-App zu integrieren. Damit
können kleine Tests schneller durchgeführt werden, ohne den Prozess des
Web-Dashboards durchlaufen zu müssen. Es ist eine CLI-Anwendung für das
Joomla! CMS, die es dem Benutzer ermöglicht, Updates im CMS
durchzuführen.

Diese Dokumentation zeigt, wie das CLI-Update verwendet werden kann und
wie man seine Funktionalität als Entwickler erweitern kann.

### Voraussetzungen

Wie bei allen anderen CLI (Befehlszeilen)-Apps, ist der Zugriff auf eine
Joomla! Installation über die Kommandozeile erforderlich, um die durch
das CLI Update zur Verfügung gestellten Werkzeuge zu benutzen.
Möglicherweise könnte man einen SSH-Zugang zum Live-Server in Betracht
ziehen.

### Begriffe und Definitionen

- CLI - Command Line Interface
- SSH - Secure SHell

## Das CLI Update benutzen

Um die Werkzeuge, die das CLI Update bietet, zu benutzen, wie bereits
vorher erwähnt, wird der Zugriff auf das Kommandozeileninterface im
Wurzelverzeichnis der Joomla! Installation benötigt. Oder, um es anders
auszudrücken, musst du dich im Root-Verzeichnis deines Joomla! Content
Management Systems (CMS) befinden, weil alle Kommandos von dort
ausgeführt werden.

<img
src="https://docs.joomla.org/images/thumb/e/ee/All_commands.png/800px-All_commands.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/e/ee/All_commands.png/1200px-All_commands.png 1.5x, https://docs.joomla.org/images/thumb/e/ee/All_commands.png/1600px-All_commands.png 2x"
data-file-width="1980" data-file-height="1460" width="800" height="590"
alt="All commands.png" />

## Allgemeine Kommandonutzung

In diesem Abschnitt wollen wir über die Nutzung von Kommandos sprechen,
wir werden über verschiedene Kommandos, was diese sind und wie du sie
einsetzen kannst. Wir werden auch etwas über vorhandene Kommandos im CLI
Update herausfinden.

### Vorhandene Kommandos

Die Anzahl der vorhandenen und verwendbaren Kommandos im CLI Update ist
sehr umfangreich. Diese Kommandos sind gruppiert, je nach Bereich des
Joomla! CMS, den sie berühren.

#### Nach Kern-Updates suchen

Das CLI Update bietet ein Kommando, um gezielt nach Updates für Joomla!
zu suchen, dabei funktioniert das genau so, wie die Internetversion von
`com_joomlaupdate`, es ruft Updates ab und meldet, ob ein Update
vorhanden ist oder nicht. Um die Updateprüfung auszuführen, werden wir
den Befehl wie gesagt aus dem Wurzelverzeichnis heraus erteilen.
`php cli/joomla.php core:check-updates`
Das Kommando wird ausgeführt und die Ausgabe zeigt eine verfügbare
Updateversion an, falls vorhanden, oder sagt andernfalls, dass die
derzeitige Installation auf dem neusten Stand ist.

<img
src="https://docs.joomla.org/images/thumb/4/4f/Check_updates.png/800px-Check_updates.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/4/4f/Check_updates.png/1200px-Check_updates.png 1.5x, https://docs.joomla.org/images/thumb/4/4f/Check_updates.png/1600px-Check_updates.png 2x"
data-file-width="2330" data-file-height="676" width="800" height="232"
alt="Check updates.png" />

#### Erweiterungskommandos

Die nächste Gruppe von Kommandos lassen uns die Erweiterungen im Joomla!
Kern verwalten, das CLI Update wird mit Kommandos ausgeliefert, mit
denen sich Erweiterungen installieren, in einer Liste aufführen und
entfernen lassen.

###### Auflisten von installierten Erweiterungen

Dieses Kommando erlaubt uns alle installierten Erweiterungen in einer
Liste aufzuführen, es kann auf zwei verschiedene Arten ausgeführt
werden, diese sind:

- *Allgemeine Auflistung*: Listet alle installierten Erweiterungen auf,
  dass heißt alle Komponenten, Sprachen, Zusatzprogramme und Module
  werden zusammen aufgelistet, wenn der Befehl erteilt wird. So wird das
  Kommando aufgerufen:`php cli/joomla.php extension:list` <img
  src="https://docs.joomla.org/images/thumb/7/71/Extension_list.png/800px-Extension_list.png"
  decoding="async"
  srcset="https://docs.joomla.org/images/thumb/7/71/Extension_list.png/1200px-Extension_list.png 1.5x, https://docs.joomla.org/images/thumb/7/71/Extension_list.png/1600px-Extension_list.png 2x"
  data-file-width="1980" data-file-height="1460" width="800" height="590"
  alt="Extension list.png" />
- *Auflistung nach Typ*: Die allgemeine Auflistung kann langwierig sein
  wenn man nach einer bestimmten Erweiterung innerhalb dieser riesigen
  Liste sucht, allerdings kann man die Anzahl auf ein erträgliches Maß
  senken, wenn man den Typ der Erweiterung genauer spezifiziert. Dies
  geschieht, indem an das Kommando eine Option angehängt wird. So wird
  das gemacht: `php cli/joomla.php extension:list --type=language`<img
  src="https://docs.joomla.org/images/thumb/c/ca/Extension_list_type.png/800px-Extension_list_type.png"
  decoding="async"
  srcset="https://docs.joomla.org/images/thumb/c/ca/Extension_list_type.png/1200px-Extension_list_type.png 1.5x, https://docs.joomla.org/images/thumb/c/ca/Extension_list_type.png/1600px-Extension_list_type.png 2x"
  data-file-width="1980" data-file-height="648" width="800" height="262"
  alt="Extension list type.png" />

Es werden hier nur sprachbasierte Erweiterungen aufgelistet, man kann
auch noch genauer nach Modulen, Zusatzprogrammen und Komponenten suchen.

###### Eine Erweiterung installieren

Joomla! bietet die Installation einer Erweiterung über eine URL auf die
gezippte Datei oder den Pfad zu dieser Datei, das CLI Update bietet auch
dieses beiden Möglichkeiten, wie man das verwenden kann wird im
folgenden beschrieben.

- *Über eine URL installieren*: Um eine Erweiterung über eine URL zu
  installieren, wollen wir das Kommando wie unten ausführen und ein
  Argument und eine Option an das `extension:install` Kommando anhängen.
  `php cli/joomla.php extension:install url --url=`<a
  href="https://github.com/joomla-extensions/patchtester/releases/download/3.0.0-beta3/com_patchtester.zip"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener"><code>https://github.com/joomla-extensions/patchtester/releases/download/3.0.0-beta3/com_patchtester.zip</code></a>
  Auf diese Art und Weise wird die `com_patchtester` Erweiterung
  heruntergeladen und installiert.

<img
src="https://docs.joomla.org/images/thumb/4/49/Extension_install_url.png/800px-Extension_install_url.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/4/49/Extension_install_url.png/1200px-Extension_install_url.png 1.5x, https://docs.joomla.org/images/thumb/4/49/Extension_install_url.png/1600px-Extension_install_url.png 2x"
data-file-width="1758" data-file-height="328" width="800" height="149"
alt="Extension install url.png" />

- *Über eine Pfad installieren*: Um eine Erweiterung über einen Pfad zu
  installieren, wird das selbe `extension:install` Kommando ausgeführt
  und das Argument für Pfade wie diesen genauer
  angegeben:`php cli/joomla.php extension:install --path=/Users/bosunski/docs/com_pathtester.zip`
  <img
  src="https://docs.joomla.org/images/thumb/e/eb/Extension_install_path.png/800px-Extension_install_path.png"
  decoding="async"
  srcset="https://docs.joomla.org/images/thumb/e/eb/Extension_install_path.png/1200px-Extension_install_path.png 1.5x, https://docs.joomla.org/images/thumb/e/eb/Extension_install_path.png/1600px-Extension_install_path.png 2x"
  data-file-width="2050" data-file-height="1208" width="800" height="471"
  alt="Extension install path.png" />

Somit wird die Erweiterung geladen und die Installatioin wie erwartet
durchgeführt.

###### Eine Erweiterung entfernen

Das letzte Kommando in dieser Kategorie ist das `extension:remove`
Kommando. Es wird benutzt um Erweiterungen aus dem Joomla! CMS zu
entfernen. Dieses Kommando verwendet die `extension_id` als Argument,
man kann auch immer `extension:list` verwenden, um die Erweiterungs ID
zu erfahren. Hier wird eine Erweiterung mit der ID `803` entfernt.
`php cli/joomla.php extension:remove 803`

<img
src="https://docs.joomla.org/images/thumb/5/50/Extension_Remove.png/800px-Extension_Remove.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/5/50/Extension_Remove.png/1200px-Extension_Remove.png 1.5x, https://docs.joomla.org/images/thumb/5/50/Extension_Remove.png/1600px-Extension_Remove.png 2x"
data-file-width="1980" data-file-height="1208" width="800" height="488"
alt="Extension Remove.png" />

#### Den Joomla! Kern auf den neusten Stand bringen

Ebenso sehr wie das CLI Update einen Befehl bietet um nach Updates zu
suchen, bietet es darüber hinaus ein Kommando, ein Update durchzuführen
falls vorhanden. Um ein Update auszuführen führen wir nur folgendes
aus:
`php cli/joomla.php core:update`
Dieses Kommando führt ein Update des Joomla! Kerns durch und bringt es
auf die neuste verfügbare Version.

<img
src="https://docs.joomla.org/images/thumb/a/ac/Core_update.png/800px-Core_update.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/a/ac/Core_update.png/1200px-Core_update.png 1.5x, https://docs.joomla.org/images/thumb/a/ac/Core_update.png/1600px-Core_update.png 2x"
data-file-width="2330" data-file-height="984" width="800" height="338"
alt="Core update.png" />

#### Joomla! über das CLI installieren

Als nächstes folgt das `core:install` Kommando, welches die Installation
des Joomla! CMS über das CLI ermöglicht. Das Kommando kann in zwei
verschiedenen Modi arbeiten: Den interaktiven und den nicht-interaktiven
Modus.

##### Interaktive Installation

Um eine frische Joomla! Installation mit Hilfe des interaktiven Modus zu
erhalten werden wir einfach das `extension:install` Kommando so
ausführen:
`php cli/joomla.php extension:install`

Der standardmäßige Installationsmodus wird gestartet und wird danach
fragen, die Einstellungen, die benötigt werden, einzugeben. Erst wenn
alle Auswahlmöglichkeiten eingegeben und bestätigt wurden, wird die
Installation gestartet und Joomla! installiert.

<img
src="https://docs.joomla.org/images/thumb/e/ec/Core_install_interactive.png/800px-Core_install_interactive.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/e/ec/Core_install_interactive.png/1200px-Core_install_interactive.png 1.5x, https://docs.joomla.org/images/thumb/e/ec/Core_install_interactive.png/1600px-Core_install_interactive.png 2x"
data-file-width="2442" data-file-height="1852" width="800" height="607"
alt="Core install interactive.png" />

##### Nicht-interaktive Installation

Der nächste Installationstyp ist der nicht-interaktve oder stille Modus.
In diesem Modus wirst du nicht danach gefragt werden, alle
Auswahlmöglichkeiten einzugeben. Stattdessen werden alle
Installationsoptionen aus einer Datei gelesen. Das Kommando wird um eine
Option ergänzt, die den Pfad zu der Datei angibt, aus der die
Installationsoptionen ausgelesen werden, das sieht folgendermaßen aus:
`php cli/joomla.php core:install --file=/path/to/config.json`

<img
src="https://docs.joomla.org/images/thumb/d/dd/Core_install_file.png/800px-Core_install_file.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/d/dd/Core_install_file.png/1200px-Core_install_file.png 1.5x, https://docs.joomla.org/images/thumb/d/dd/Core_install_file.png/1600px-Core_install_file.png 2x"
data-file-width="1980" data-file-height="760" width="800" height="307"
alt="Core install file.png" />

Ein Beispiel für den Inhalt der `config.json` kann sein:

```json
    {
      "language":"en-GB",
      "site_name":"Joomla",
      "admin_email":"email@example.com",
      "admin_user":"user",
      "admin_password":"password",
      "db_type":"mysql",
      "db_host":"localhost",
      "db_user":"root",
      "db_pass":"xcdxcx",
      "db_name":"jtest",
      "db_prefix":"efs0k_",
      "db_old":"remove",
      "helpurl":"https://joomla.org"
    }
```

ODER als Muster einer `config.ini`

```ini
    site_name="gsoc"
    admin_email="user@example.com"
    admin_user="user"
    admin_password="secret"
    db_type="mysql"
    db_host="localhost"
    db_user="root"
    db_pass=""
    db_name="joomla"
    db_prefix="prefix_"
```

Die definierten Optionen werden geladen und valdiert, anschließend wird
die Installation ausgeführt.

#### Kommandos die einer Konfiguration zugrunde liegen

Der nächste Satz Kommandos den wir betrachten ist für das Setzen und die
Besichtigung von Konfigurationsoptionen innerhalb der Joomla!
Installation:

##### Konfigurationen einstellen

Das CLI Update bietet einen Befehl, der den Wert einer bestehenden
Option in der Konfiguration ändert. Unter Verwendung des `config:get`
Kommandos kann man jede beliebige Konfigurationsoption, die sich
innerhalb der `configuration.php` befindet, ändern. So wird dieses
Kommando verwendet:
`php cli/joomla.php config:set mailer=mail`

<img
src="https://docs.joomla.org/images/thumb/6/6e/Config_set_single.png/800px-Config_set_single.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/6/6e/Config_set_single.png/1200px-Config_set_single.png 1.5x, https://docs.joomla.org/images/thumb/6/6e/Config_set_single.png/1600px-Config_set_single.png 2x"
data-file-width="1980" data-file-height="648" width="800" height="262"
alt="Config set single.png" />

Es ist sogar mögliche, viele Paare von Optionswerten wie diese
hinzuzufügen:
`php cli/joomla.php config:set mailer=mail fromname=Joomla! sitename="Joomla Site"`
Auf diese Weise werden Optionen auf einmal geändert.

<img
src="https://docs.joomla.org/images/thumb/8/8b/Config_set_multiple.png/800px-Config_set_multiple.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/8/8b/Config_set_multiple.png/1200px-Config_set_multiple.png 1.5x, https://docs.joomla.org/images/thumb/8/8b/Config_set_multiple.png/1600px-Config_set_multiple.png 2x"
data-file-width="1980" data-file-height="984" width="800" height="398"
alt="Config set multiple.png" />

##### Konfigurationen erhalten

Als nächsten Befehl in dieser Kategorie sei das `config:get` Kommando
genannt, wie der Name schon vermuten lässt liefert es den Wert einer
Option der sich innerhalb der `configuration.php` befindet, ohne
zusätzliches Argument zeigt es alle verfügbaren Optionen in
tabellarischer Form an. Jetzt zu diesem Befehl:
`php cli/joomla.php config:get sitename`

Hier wird der Wert der Option `sitename` ausgegeben.

<img
src="https://docs.joomla.org/images/thumb/3/3b/011-cli.png/800px-011-cli.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/3/3b/011-cli.png/1200px-011-cli.png 1.5x, https://docs.joomla.org/images/thumb/3/3b/011-cli.png/1600px-011-cli.png 2x"
data-file-width="2050" data-file-height="564" width="800" height="220"
alt="011-cli.png" />

`php cli/joomla.php config:get`

Nun werden alle Werte der verfügbaren Optionen ausgeben.

<img
src="https://docs.joomla.org/images/thumb/d/d9/Config_get_all.png/800px-Config_get_all.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/d/d9/Config_get_all.png/1200px-Config_get_all.png 1.5x, https://docs.joomla.org/images/thumb/d/d9/Config_get_all.png/1600px-Config_get_all.png 2x"
data-file-width="1980" data-file-height="956" width="800" height="386"
alt="Config get all.png" />

##### Konfigurationen gruppiert erhalten

Das CLI Update fügt eine Option für den `config:get` Befehl hinzu, der
es uns ermöglicht, logisch gruppierte Optionen zu erhalten, sagen wir
zum Beispiel man möchte alle datenbankspezifischen Optionen oder alle
E-Mail bezogenen Optionen anzeigen lassen. Um das zu erreichen bietet
das CLI Update eine wählbare Möglichkeit, eine genauer spezifizierte
Gruppe von Wahlmöglichkeiten anzeigen zu lassen, und so wird das
gemacht:

`php cli/joomla.php config:get --group=mail`

<img
src="https://docs.joomla.org/images/thumb/f/fb/Config_get_group.png/800px-Config_get_group.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/f/fb/Config_get_group.png/1200px-Config_get_group.png 1.5x, https://docs.joomla.org/images/thumb/f/fb/Config_get_group.png/1600px-Config_get_group.png 2x"
data-file-width="1980" data-file-height="816" width="800" height="330"
alt="Config get group.png" />

Der Befehl verwendet die `group` Option und erhält den Wert `mail`. CLI
Update unterstüzt derzeit die Gruppenwerte: mail, db und Sitzung
(session).

#### Befehle für Webseite online und offline

Wie der Name schon suggeriert, erlauben es diese Kommandos eine von
Joomla! betriebene Website flugs in den offline- oder online-Modus zu
versetzen, so wird dieser Befehl verwendet:
`php cli/joomla.php site:up`

<img
src="https://docs.joomla.org/images/thumb/d/d2/Site_up.png/800px-Site_up.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/d/d2/Site_up.png/1200px-Site_up.png 1.5x, https://docs.joomla.org/images/thumb/d/d2/Site_up.png/1600px-Site_up.png 2x"
data-file-width="1980" data-file-height="816" width="800" height="330"
alt="Site up.png" />

`php cli/joomla.php site:down`

<img
src="https://docs.joomla.org/images/thumb/8/82/Site_down.png/800px-Site_down.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/8/82/Site_down.png/1200px-Site_down.png 1.5x, https://docs.joomla.org/images/thumb/8/82/Site_down.png/1600px-Site_down.png 2x"
data-file-width="1980" data-file-height="816" width="800" height="330"
alt="Site down.png" />

Die zweite Instruktion versetzt die Website offline, während die erste
Anweisung die Website online stellt.

## Zum CLI Update als Entwickler beitragen

### Mehr Instruktionen hinzufügen

Das CLI Update wird mit einigen Befehlen ausgeliefert, die hinzugefügt
werden basierend auf der Notwendigkeit die die Zeit mit sich bringt. Wie
auch immer, dem CLI Update können andere Kommandos einfach hinzugefügt
werden, damit beschäftigt sich dieser Abschnitt, der ein Videotutorial
(siehe unten) beinhaltet.

### Andere Kommandos innhalb von Kommandoklassen aufrufen

Sollte es für der Fall sein, dass man einen Befehl, den man für das CLI
Update schreiben will, es erfordert, dass ein bestehendes Kommando
intern aufgerufen werden muss, so kann dies innerhalb einer anderen
Befehlsklasse aus geschehen:

```php
<?php
class RunHelloCommand extends AbstractCommand {
    ...

    public function execute(): int {
        $command = $this->getApplication()->getCommand('say:hello');
        $code = $command->execute();

        if($code === 0) {
            // command ran successfully, do something
        }
    }

    ...
}
```

Hier versuchen wir, dass `say:hello` Kommando aufzurufen, dass in obigem
Video erschaffen wurde.

### Einige wichtige Sachen zum Recherchieren

- Symfony Style Dokumentation:
  <a href="https://symfony.com/doc/current/console/style.html"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">How to Style a Console Command</a>
- Eine CLI Anwendung mit Joomla 4 entwickeln [J4.x:Writing A CLI
  Application](https://docs.joomla.org/J4.x:Writing_A_CLI_Application "J4.x:Writing A CLI Application")

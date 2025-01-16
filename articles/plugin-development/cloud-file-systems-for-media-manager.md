<!-- Filename: J4.x:Cloud_File_Systems_for_Media_Manager / Display title: Cloud-Dateisysteme für Media-Manager -->

<span id="main-portal-heading">GSoC 2017
Cloud-Dateisysteme für den Medienmanager
Dokumentation</span> [<img
src="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/75px-Gsoc2016.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/113px-Gsoc2016.png 1.5x, https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/150px-Gsoc2016.png 2x"
data-file-width="373" data-file-height="373" width="75" height="75"
alt="Gsoc2016.png" />](https://docs.joomla.org/GSOC_2017 "GSOC 2017")
Joomla!  4.x

## Einführung

**Joomla! 4.x** wird standardmäßig mit Cloud-Dateisystemen für den **Medien-Manager** geliefert. Mit der vorherigen API war das Erstellen benutzerdefinierter Dateisysteme eine schwierige Aufgabe. Dank der neuen API ist es nun einfach, ein benutzerdefiniertes Dateisystem zu erstellen. Wenn Sie einen Cloud-Dienst mit dem neuen Medien-Manager verwenden möchten, wird die Verwendung von **OAuth2.0** empfohlen.

Dieses Dokument führt Sie durch wichtige Schritte zur Erstellung Ihres eigenen **Dateisystem-Plugins**, um den **Media-Manager** zu erweitern. Bevor Sie fortfahren, stellen Sie bitte sicher, dass Sie über grundlegende Kenntnisse zur Entwicklung eines Plugins für Joomla verfügen. [Dieses Tutorial](https://docs.joomla.org/J3.x:Creating_a_Plugin_for_Joomla "Special:MyLanguage/J3.x:Creating a Plugin for Joomla") sollte hilfreich sein.

## Erstellen Sie Ihr Dateisystem-Plugin

### Konfiguration

Zuerst müssen wir ein **Filesystem**-Plugin erstellen, um den Medien-Manager zu erweitern. Dieses Plugin sollte einige wichtige Attribute enthalten, damit es mit dem Medien-Manager funktionieren kann.

Stellen Sie sicher, dass Ihr Plugin `group="filesystem"` enthält. In Ihrer `[plugin-name].xml` sollten Sie Folgendes haben:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension version="4.0" type="plugin" group="filesystem" method="upgrade">
    <name>plg_filesystem_myplugin</name>
    <author>Joomla! Project</author>
    <creationDate>April 2017</creationDate>
    <copyright>Copyright (C) 2005 - 2017 Open Source Matters. All rights reserved.</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <authorEmail>admin@joomla.org</authorEmail>
    <authorUrl>www.joomla.org</authorUrl>
    <version>__DEPLOY_VERSION__</version>
    <description>Description</description>
    <files>
        <filename plugin="myplugin">myplugin.php</filename>
        <folder>SomeFolder</folder>
    </files>

    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="display_name"
                    type="text"
                    label="YOUR_LABEL"
                    description="YOUR_DESCRIPTION"
                    default="DEFAULT_VALUE"
                />
            </fieldset>
        </fields>
    </config>
</extension>
```

Der **display_name**-Parameter hilft dem Medienmanager, den Namen Ihres **Dateisystems** als Wurzelknoten im Dateibrowser anzuzeigen. Jeder Adapter, der zu Ihrem Dateisystem gehört, wird als untergeordneter Knoten unterhalb der Wurzel angezeigt.

Bitte verwenden Sie alle erforderlichen Parameter, die Sie benötigen, wie zum Beispiel `App Secret` mit geeigneten Formulareingabefeldern.

### Plugin-Ereignisse

- `onFileSystemGetAdapters()`
- `onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $Ereignis)`

#### onFileSystemGetAdapters()

**Jedes** Filesystem-Plugin sollte ein Ereignis namens `onFileSystemGetAdapters()` für die Funktionalität enthalten. Dieses Ereignis sollte ein **Array** von `Joomla\Component\Media\Administrator\Adapter\AdapterInterface` zurückgeben, wenn es aufgerufen wird. Das Ereignis wird ausgelöst, wenn Sie den Medienmanager öffnen. Jedes `AdapterInterface` wird unter dem Stammknoten Ihres Dateisystems eingehängt.

#### onFileSystemOAuthCallback()

Dieses Ereignis wird ausgelöst, wenn Sie den **OAuthCallbackHandler** des Media Managers für den im Dokument unten beschriebenen OAuth2.0-Autorisierungs- und Authentifizierungsprozess verwendet haben. Das Ereignis wird nur beim angeforderten Plugin ausgelöst. Es ist daher nicht notwendig, in einem typischen Szenario auf `$context` zu prüfen.

Ein Beispiel für die Verwendung des Ereignisses sieht folgendermaßen aus:

```php
    public function onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)
    {
        // Your context
        $context = $event->getContext();

        // Get the input
        $data = $event->getInput();

        // Your code goes here

        // Set result to be returned
        $result = [
            "action" => "control-panel"
        ];

        // Pass back the result to event
        $event->setArgument('result', $result);
    }
```

**OAuthCallbackEvent** enthält die an den Media Manager OAuthCallback-URI weitergeleiteten Eingaben. `$event->getInput()` gibt ein Input-Objekt zurück.

`$result` definiert, wie Joomla nach einer Weiterleitung zur Rückrufaktion verhalten soll. Es gibt mehrere mögliche Lösungen:

- `aktion`
  - schließen: Schließt ein Fenster, das von einem JavaScript geöffnet wurde **mithilfe von window.open()**
  - weiterleiten: Leitet zur Seite um, die durch die **redirect_uri** angegeben ist
  - kontroll-panel: Weiterleitung zum Kontrollpanel
  - medien-manager: Weiterleitung zum Medienmanager
- `redirect_uri`
  - Wird mit **weiterleiten** Aktion verwendet (erforderlich)
- `nachricht`
  - Nachricht muss nach einer Aktion angezeigt werden
- `nachricht_typ`
  - warnung
  - hinweis
  - fehler
  - nachricht (oder leer lassen)

Nachdem Sie alles eingestellt haben, setzen Sie das `$result`-Argument des `$event`, um es wieder an den Aufrufer zurückzugeben.

Ein Beispiel, um eine Nachricht an den Benutzer anzuzeigen, wäre:

```php
    $result = [
        "action" => "media-manager",
        "message" => "Some message",
        "message-type" => "notice"
    ];
```

Dadurch wird auf den Media Manager umgeleitet und eine Benachrichtigung mit dem Text im **Nachricht** angezeigt.

Diese Methode kann typischerweise verwendet werden, um Berechtigungscodes für den **OAuth2.0**-Prozess zu erhalten. Im Falle eines **Fehlers** wird Joomla! auf das Kontrollpanel zurückfallen und eine Fehlermeldung anzeigen.

## Authentifizierung und Autorisierung

Joomla! 4.x rät Ihnen, OAuth2.0 für diesen Prozess zu verwenden. Es ist ein häufiges Szenario, dass OAuth2.0 eine **Umleitungs-URL** benötigt, um Authentifizierungsdaten an die Anwendung weiterzugeben. Dies wird typischerweise von einem Rückruf-Handler erledigt. Für Ihr Plugin müssen Sie das Rad nicht neu erfinden, Sie können den **Callback-Handler** des **Medienmanagers** verwenden, um die Aufgabe zu erfüllen.

Alles, was Sie tun müssen, ist, die **Redirect URI** im OAuth2.0-Anbieter auf Folgendes festzulegen und die `onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)` in Ihrem Plugin zu implementieren.

`[site-name]/administrator/index.php?option=com_media&task=plugin.oauthcallback&plugin=[your-plugin-name]`

In diesem Beispiel:  
`[site-name]/administrator/index.php?option=com_media&task=plugin.oauthcallback&plugin=myplugin`

Nun, wenn Ihr Anbieter eine Weiterleitung durchführt, sobald die bereitgestellte URL erreicht wird, stellt der **Controller** für den Medien-Manager sicher, dass Ihr Plugin `onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)` implementiert, bevor irgendwelche Daten übergeben werden. Andernfalls werden Sie mit einer Fehlermeldung zum Control Panel weitergeleitet.

Wenn Sie alle übergebenen Eingaben implementiert haben, wird der Cloud-Anbieter in das `$event` einbezogen. Sie können darauf mit `$event->getInput()` zugreifen und es wie eine übliche `input` in Joomla behandeln.

Nachdem Sie das `input` erhalten haben, können Sie es verwenden, um die Details Ihres Cloud-Dienstes wie **Access Token**, **Refresh Token** usw. zu erhalten.

Wenn Sie mehrere Konten unterscheiden möchten, können Sie dafür `Session` verwenden.

**Hinweis**: Es wird empfohlen, vor dem Fortsetzen Ihrer Anfrage den **CSRF-Token** zu überprüfen. Die meisten Cloud-Anbieter unterstützen den `state`-Parameter, der zur Erfüllung der Aufgabe verwendet werden kann.

### Häufige Fallen

Es ist erforderlich, Ihre Redirect-URI zu `urlencode()`, wenn Sie diese an den Cloud-Anbieter senden. Bitte verwenden Sie es, um Fehlverhalten Ihrer Cloud zu vermeiden.

## Datei-Serve von Ihrem Adapter

Um Ihre Mediendateien vom Medienmanager an die **Joomla! Seite** auszuliefern, hilft Ihnen `Joomla\Component\Media\Administrator\Adapter\AdapterInterface`. Diese Schnittstelle enthält eine spezielle Methode namens `getUrl($path)`.

`$path : Der Pfad ist relativ zu Ihrem Root-Verzeichnis`

Die Absicht der Methode besteht darin, eine **öffentliche absolute URL** für die Datei bereitzustellen, die Sie auf der Website einfügen möchten. Angenommen, Ihr Dateipfad lautet `/path/to/me.png` auf dem Cloud-Server. Eine öffentliche URL könnte dann etwa so aussehen: `mycloud.com/share/u/myusername/path/to/me.png`. Wie diese bereitgestellt wird, liegt ganz bei Ihnen. Wenn ein Benutzer eine durch einen Mediengenerator erstellte URL einfügen möchte, wird diese Methode verwendet.

Weitere Einzelheiten zur `Adapter-Schnittstelle` finden Sie in `administrator/componenents/com_media/Adapter/AdapterInterface.php`.

*Übersetzt von openai.com*


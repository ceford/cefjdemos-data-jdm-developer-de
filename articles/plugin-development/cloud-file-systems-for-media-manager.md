<!-- Filename: J4.x:Cloud_File_Systems_for_Media_Manager / Display title: Cloud-Dateisysteme für den Medien-Manager -->

<span id="main-portal-heading">GSoC 2017
Cloud-Dateisysteme für den Medien-Manager
Dokumentation</span> [<img
src="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/75px-Gsoc2016.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/113px-Gsoc2016.png 1.5x, https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/150px-Gsoc2016.png 2x"
data-file-width="373" data-file-height="373" width="75" height="75"
alt="Gsoc2016.png" />](https://docs.joomla.org/GSOC_2017 "GSOC 2017")
Joomla!  4.x

## Einführung

**Joomla! 4.x** wird standardmäßig mit Cloud-Dateisystemen für den
**Medien-Manager** ausgestattet. Mit der vorherigen API war das
Erstellen von benutzerdefinierten Dateisystemen eine schwierige Aufgabe.
Dank der neuen API ist es jetzt einfach, ein benutzerdefiniertes
Dateisystem zu erstellen. Wenn ein Cloud-Dienst mit dem neuen
Medien-Manager genutzt werden soll, ist es ratsam, **OAuth2.0** zu
verwenden.

Dieses Dokument beschreibt die wichtigsten Schritte zur Erstellung eines
eigenen **Dateisystem-Plugins** zur Erweiterung des **Medien-Managers**.
Es ist wichtig, dass das Grundwissen vorhanden ist, wie man ein Plugin
für Joomla entwickelt. [Dieses
Tutorial](https://docs.joomla.org/J3.x:Creating_a_Plugin_for_Joomla "Special:MyLanguage/J3.x:Creating a Plugin for Joomla")
sollte dabei helfen.

## Dateisystem-Plugin erstellen

### Konfiguration

Als erstes muss ein **Dateisystem**-Plugin erstellt werden, das den
Medien-Manager erweitert. Dieses Plugin sollte einige wichtige Attribute
enthalten, damit es mit dem Medien-Manager zusammenarbeiten kann.

Das Plugin muss die Option `group="filesystem"` enthalten. In der Datei
`[plugin-name].xml` sollte daher Folgendes enthalten sein:

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

Der Parameter **display_name** hilft dem Medien-Manager, den Namen des
**Dateisystems** als Root-Knoten im Datei-Browser anzuzeigen. Alle
Adapter, die zum Dateisystem gehören, werden als untergeordnete Knoten
unter der Wurzel angezeigt.

Eventuell benötigte Parameter wie `App Secret` mit den passenden
Formularfeldern einbinden.

### Plugin-Events

- `onFileSystemGetAdapters()`
- `onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)`

#### onFileSystemGetAdapters()

**Jedes** Dateisystem-Plugin sollte ein Event mit dem Namen
`onFileSystemGetAdapters()` für die Funktionalität enthalten. Dieses
Event sollte ein **Array** von
`Joomla\Component\Media\Administrator\Adapter\AdapterInterface`
zurückgeben, wenn es aufgerufen wird. Das Event wird ausgelöst, wenn der
Medien-Manager geöffnet wird. Jedes `AdapterInterface` wird unter dem
Root-Knoten des Dateisystems eingehängt.

#### onFileSystemOAuthCallback()

Dieses Event wird ausgelöst, wenn der **OAuthCallbackHandler** des
Medien-Managers für den unten im Dokument beschriebenen
OAuth2.0-Autorisierungs- und Authentifizierungsprozess verwendet wird.
Das Event wird nur auf das angeforderte Plugin übertragen. Es ist in
einem typischen Szenario also nicht nötig, nach `$context` zu suchen.

Ein Anwendungsbeispiel für das Event sieht so aus:

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

**OAuthCallbackEvent** enthält die an den Medien-Manager
OAuthCallback-URI weitergeleitete Eingabe. `$event->getInput()` gibt ein
Input-Objekt zurück.

`$result` definiert, wie sich Joomla nach einer Weiterleitung zum
Callback verhält. Es stehen mehrere Lösungsmöglichkeiten zur Verfügung:

- `action`
  - close: Schließt ein Fenster, das durch ein JavaScript **mittels
    window.open()** geöffnet wurde
  - redirect: Umleitung auf die in der **redirect_uri** angegebene Seite
  - control-panel: Umleitung zum Kontrollpanel
  - media-manager: Umleitung zum Medien-Manager
- `redirect_uri`
  - Verwendung mit der Aktion **redirect** (erforderlich)
- `message`
  - Nachricht muss nach einer Aktion angezeigt werden
- `message_type`
  - Warnung
  - Hinweis
  - Fehler
  - Nachricht (oder leer lassen)

Nachdem alles gesetzt wurde, das Argument `$result` von `$event`
verwenden, um es an den Aufrufenden zurückzugeben.

Eine Nachricht an den Benutzer könnte beispielsweise folgendermaßen
aussehen:

```php
    $result = [
        "action" => "media-manager",
        "message" => "Some message",
        "message-type" => "notice"
    ];
```

Diese leitet auf den Medien-Manager um und gibt eine Meldung mit dem
Text in **message** aus.

Diese Methode kann typischerweise dazu verwendet werden, um
Autorisierungscodes für den **OAuth2.0**-Prozess zu erhalten. Im Falle
eines **Fehlers** greift Joomla! auf das Kontrollzentrum zurück und
zeigt eine Fehlermeldung an.

## Authentifizierung und Autorisierung

Joomla! 4.x empfiehlt, OAuth2.0 für diesen Prozess zu verwenden. Es ist
ein häufiges Szenario, dass OAuth2.0 eine **redirect url** benötigt, um
Authentifizierungsdaten an die Anwendung zu übergeben. Dies wird
typischerweise durch einen Callback-Handler erledigt. Für das Plugin
muss das Rad nicht neu erfunden werden. Der **Callback Handler** des
**Medien-Managers** kann diese Aufgabe übernehmen.

All you have to do is to set the **Redirect URI** to following in your
OAuth2.0 Provide and Implement the
`onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)`
in your plugin.

`[site-name]/administrator/index.php?option=com_media&task=plugin.oauthcallback&plugin=[your-plugin-name]`

In this example:
`[site-name]/administrator/index.php?option=com_media&task=plugin.oauthcallback&plugin=myplugin`

Now when you do a redirect from your provider once it reaches the URL
provided, the **Controller** for Media Manager will ensure that your
plugin implements
`onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)`
before passing any data to it. If not, you will be redirected to the
Control Panel with an error message.

If you have implemented all the inputs that are passed with, the Cloud
Provider will be included in the `$event`. You can access them using
`$event->getInput()` and treat it as an usual `input` to Joomla.

After you received the `input`, you can use it to obtain the details of
your cloud service such as **Access Token**, **Refresh Token** etc.

If you want to distinguish multiple accounts, you can use `Session` for
that.

**Note**: It is advised to check against **CSRF token** before you
proceed your request. Most cloud providers support `state` parameter
that can be used to fulfill the task.

### Common Pitfalls

It is required to `urlencode()` your redirect uri when you're sending it
to the cloud provider. Please use it to avoid misbehaviours of your
cloud.

## File Serve from your Adapter

To serve your media files from the Media Manager to **Joomla! Site**
`Joomla\Component\Media\Administrator\Adapter\AdapterInterface` helps
you. This Interface contains a special method called `getUrl($path)`.

`$path : Path is relative to your root`

The intention of the method is to provide a **Public Absolute URL** for
the file you requested to insert in the site. For example assume your
file path is `/path/to/me.png` in the cloud server. Now a public URL
might be something like `mycloud.com/share/u/myusername/path/to/me.png`.
How it is being served, is completely up to you. When an user wants to
insert a media generated URL, this method will be used.

More details about `Adapter Interface` can be found in
`administrator/componenents/com_media/Adapter/AdapterInterface.php`

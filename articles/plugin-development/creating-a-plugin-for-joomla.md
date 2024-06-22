<!-- Filename: J4.x:Creating_a_Plugin_for_Joomla / Display title: Ein Plugin für Joomla! erstellen -->

<span id="main-portal-heading">Anleitung
Leitfaden für die Erstellung eines Plugins für Joomla! 4</span> Joomla! 
4.x series

Die Pluginstruktur für Joomla! 1.5, 2.5 und 3.x war sehr flexibel und
mächtig. Plugins können nicht nur verwendet werden um Events, die durch
die Hauptanwendung und durch Erweiterungen ausgelöst wurden, zu
verarbeiten, sondern auch, um Erweiterungen von Drittanbietern
erweiterbar und leistungsfähig zu gestalten. In Joomla! 4.x haben wir
eine Menge Code der zugrunde liegenden Prozessverwaltung umgeschrieben,
um die Flexibilität zu erhöhen, wenn man die Parameter, die als
Ereignisse übergeben werden, verändert, während zeitgleich die
Leistungsfähigkeit der Plugins erhöht wurde.

Zweck dieser Anleitung ist es, dich mit den Grundlagen vertraut zu
machen, die man für die Entwicklung eines eigenen Plugins benötigt. Die
meisten Plugins bestehen nur aus einer einzigen Code-Datei. Um es
allerdings richtig zu installieren, muss der Plugin-Code in eine
Installationsdatei gepackt werden, die vom Joomla! Installer verarbeitet
werden kann.

### Die Installationsdatei erstellen

Wie bei allen anderen Erweiterungen von Joomla!, werden Plugins einfach
mit einer .zip-Datei installiert (.tar.gz wird auch unterstützt),
allerdings muss eine einwandfreie XML-Datei enthalten sein.
Als ein Beispiel ist hier die XML-Installationsdatei für das Plugin des
Such-Plugins für Kategorien.

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="plugin" group="search" method="upgrade">
	<name>plg_search_categories</name>
	<author>Joomla! Project</author>
	<creationDate>November 2005</creationDate>
	<copyright>Copyright (C) 2005 - 2018 Open Source Matters. All rights reserved.</copyright>
	<license>GNU General Public License version 2 or later; see LICENSE.txt</license>
	<authorEmail>admin@joomla.org</authorEmail>
	<authorUrl>www.joomla.org</authorUrl>
	<version>3.0.0</version>
	<description>PLG_SEARCH_CATEGORIES_XML_DESCRIPTION</description>
	<files>
		<filename plugin="categories">categories.php</filename>
	</files>
	<languages>
		<language tag="en-GB">en-GB.plg_search_categories.ini</language>
		<language tag="en-GB">en-GB.plg_search_categories.sys.ini</language>
	</languages>
	<config>
		<fields name="params">

			<fieldset name="basic">
				<field
					name="search_limit"
					type="number"
					label="JFIELD_PLG_SEARCH_SEARCHLIMIT_LABEL"
					default="50"
				/>

				<field
					name="search_content"
					type="radio"
					label="JFIELD_PLG_SEARCH_ALL_LABEL"
					layout="joomla.form.field.radio.switcher"
					default="0"
					>
					<option value="1">JYES</option>
					<option value="0">JNO</option>
				</field>

				<field
					name="search_archived"
					type="radio"
					label="JFIELD_PLG_SEARCH_ARCHIVED_LABEL"
					layout="joomla.form.field.radio.switcher"
					default="0"
					>
					<option value="1">JYES</option>
					<option value="0">JNO</option>
				</field>
			</fieldset>

		</fields>
	</config>
</extension>
```

Wie du sehen kannst, ähnelt der Aufbau anderen Joomla!
XML-Installationsdateien. Achte nur auf den Eintrag `group="xxx"` im Tag
und die erweiterten Informationen im Tag . Die Informationen sagen
Joomla!, in welchen Ordner die Datei kopiert werden soll und zu welcher
Gruppe von Plugins es hinzugefügt werden soll.

Falls man ein Plugin kreiert, das auf bestehende Kern-Events reagiert,
würde sich das Attribut `group="xxx"` ändern, um die Bezeichnung des
bestehenden Pluginordners für den Ereignistypen, den man zu erweitern
wünscht, wiederzugeben. Beispielsweise `group="Authentifizierung"` oder
`group="User"`. Beachte auch
[Plugin/Ereignisse](https://docs.joomla.org/Plugin/Events "Special:MyLanguage/Plugin/Events")
für eine vollständige Liste von bestehenden Kategorien von Kern-Events.
Bei der Gestaltung eines neuen Plugins, welches auf Kern-Events
reagiert, ist es wesentlich, dass dein Pluginname eindeutig ist und
nicht mit irgendeinem der anderen Plugins kollidiert, die ebenfalls auf
das Kernereignis reagieren können, von dem du auch profitieren willst.

Falls man jedoch ein Plugin entwirft, das auf System-Events außerhalb
des Joomla-Kerns reagiert, sollte sich die Wahl des Tags `group="xxx"`
von den bestehenden Kernkategorien unterscheiden.

**Tipp:** *Wird das Attribut `method="upgrade"` dem* Tag *hinzugefügt,
kann dieses Plugin installiert werden, ohne eine vorherige Version
entfernen zu müssen. Alle bestehenden Dateien werden überschrieben, aber
alte Dateien werden nicht gelöscht.*

### Das Plugin erstellen

In Joomla 4 kannst du auch Namensraum-Code in deinem Plugin verwenden –
bitte lese [Namensraum-Konventionen in
Joomla](https://docs.joomla.org/J4.x:Namespace_Conventions_In_Joomla "Special:MyLanguage/J4.x:Namespace Conventions In Joomla").
Beachte, dass die eigentliche Hauptdatei keinen Namensraum hat, aber die
Felder und jeder andere unterstützende Code schon.

Die objekt-orientierte Vorgehensweise zur Erstellung von Plugins
erfordert das Verfassen einer Unterklasse von <a
href="https://api.joomla.org/cms-4/classes/Joomla.CMS.Plugin.CMSPlugin.html%7C"
class="external text" target="_blank"
rel="noreferrer noopener">CMSPlugin</a>, eine Grundlagenklasse, welche
die grundlegenden Eigenschaften von Plugins umsetzt. In deinen Methoden
sind die folgenden Eigenschaften verfügbar:

- `$this->params`: die
  [Parameter](https://docs.joomla.org/Parameter "Special:MyLanguage/Parameter"),
  die für dieses Plugin vom Administrator gesetzt werden
- `$this->_name`: der Name des Plugins
- `$this->_type`: die Gruppe (Art) des Plugins
- `$this->db`: das Datenbankobjekt
- `$this->app`: das Anwendungsobjekt

**Tipp:** *Um `$this->db` und `$this->app` zu verwenden, testet
`CMSPlugin`, ob das Objekt existiert und nicht privat ist. Falls die
Standardobjekte verwendet werden sollen, erstelle nicht instantiierte
Eigenschaften in der Plugin-Objektklasse (dass heißt
`protected $db; protected $app;` ist im selben Bereich wie
`protected $autoloadLanguage = true;`). Die Eigenschaften werden nicht
existieren, es sei denn, sie werden ausdrücklich erstellt.*

Im folgenden Programmcodebeispiel vertritt die Gruppe (Typ) des Plugins,
und stellt den Namen dar. Es sollte beachtet werden, dass bei Klassen-
und Funktionsnamen in PHP nicht zwischen Groß- und Kleinbuchstaben
unterschieden wird.

Wir haben hier auch das <a
href="https://api.joomla.org/framework-2/classes/Joomla.Event.SubscriberInterface.html"
class="external text" target="_blank"
rel="noreferrer noopener">SubscriberInterface</a> implementiert, welches
den größten Umbruch seit Joomla! 1.5-3.x darstellt. Anstatt den
Funktionsnamen, der auch dem Ereignisnamen entspricht, automatisch
aufspüren zu lassen, erlaubt dies, angepasste Funktionsnamen zu
verwenden. Daher sind wir in der Lage mitzuteilen, welche Plugins welche
Funktionen implementieren und da die Syntaxprüfung öffentlicher Methoden
im PHP-Kode langsam ist, ergibt das einen gewaltigen Leistungsschub.

Beachte, dass es in der gesamten Joomla! 4 Serie möglich ist, Plugins
mit der alten Namensstrategie von Pluginnamen, die mit Ereignisnamen
übereinstimmen, zu verwenden, falls das SubscriberInterface nicht
implementiert ist.

```php
<?php
// no direct access
defined( '_JEXEC' ) or die;

use Joomla\CMS\Plugin\CMSPlugin;
use Joomla\Event\Event;
use Joomla\Event\SubscriberInterface;

class Plg<PluginGroup><PluginName> extends CMSPlugin implements SubscriberInterface
{
	/**
	 * Load the language file on instantiation
	 *
	 * @var    boolean
	 * @since  3.1
	 */
	protected $autoloadLanguage = true;

	/**
	 * Returns an array of events this subscriber will listen to.
	 *
	 * @return  array
	 */
	public static function getSubscribedEvents(): array
	{
		return [
			'<EventName>' => 'myFunctionName',
		];
	}

	/**
	 * Plugin method is the array value in the getSubscribedEvents method
	 * The plugin then modifies the Event object (if it's not immutable)
	 */
	 public function myFunctionName(Event $event)
	 {
		/*
		 * Plugin code goes here.
		 * You can access parameters via $this->params
		 */
		return true;
	}
}
?>
```

### Plugins im eigenen Code verwenden

Falls ein Plugin für ein neues, den Kern nicht betreffendes Ereignis
erstellt wird, muss das Plugin nach der Installation aktiviert werden.
Vor jedem Bezug auf ein neues Plugin sollte die Anweisung
`JPluginHelper::importPlugin()` verwendet werden.

Nach der Erstellung eines neuen Plugins soll es wahrscheinlich im
Programmcode aufgerufen werden. **Hinweis:** Der Joomla! Kern enthält
eine Vielzahl von eingebauten Ereignissen, für welche der neue
Plugincode vielleicht registriert werden soll (in diesem Fall kann der
folgende Abschnitt ignoriert werden).

#### Das neue Verfahren für Joomla! 4

Die neue Verfahrensweise in Joomla! 4 ist es, den Dispatcher abzurufen
und einen benannten Event zu übermitteln (zu "dispatchen").

```php
    use Joomla\CMS\Event\AbstractEvent;
    use Joomla\CMS\Factory;

    $dispatcher = Factory::getApplication()->getDispatcher();

    // Here we create an event however as long as you implement EventInterface you can create your own
    // custom classes
    $event = AbstractEvent::create(
        '',
        [
            'name' => $value,
        ]
    );

    $eventResult = $dispatcher->dispatch('', $event);
```

Falls der Benutzer die Erlaubnis erhält, Werte zu verändern, kann man
das Ergebnis des Events verwenden und Ergebnisse daraus zurückgewinnen.
Du kannst folgendes anschauen:

```php
    defined('_JEXEC') or die;

    use BadMethodCallException;
    use Joomla\CMS\Event\AbstractImmutableEvent;
    use Joomla\CMS\Table\TableInterface;

    /**
     * Event class for an event
     */
    class MyCustomEvent extends AbstractImmutableEvent
    {
        /**
         * Constructor.
         *
         * @param   string  $name       The event name.
         * @param   array   $arguments  The event arguments.
         *
         * @throws  BadMethodCallException
         */
        public function __construct($name, array $arguments = array())
        {
            if (!array_key_exists('myProperty', $arguments))
            {
                throw new BadMethodCallException("Argument 'myProperty' is required for event $name");
            }

            parent::__construct($name, $arguments);
        }

        /**
         * Setter for the myProperty argument
         *
         * @param   mixed  $value  The value to set
         *
         * @return  mixed
         *
         * @throws  BadMethodCallException  if the argument is not of the expected type
         */
        protected function setMyProperty($value)
        {
            if (!empty($value) && !is_object($value) && !is_array($value))
            {
                throw new BadMethodCallException("Argument 'src' of event {$this->name} must be empty, object or array");
            }

            return $value;
        }
    }
```

Warum haben wir diese Namensklasse für Parameter eingeführt? Nun, sie
macht es einfacher, angepasste setters und getters für die Eigenschaften
einzuführen - momentan kann ein Plugin eine Eigenschaft komplett
verändern - für einen Verfasser gibt es keinen Weg, dies zu begrenzen.
Zusätzlich erleichtert es den Entwicklern das Hinzufügen und Entfernen
von Parametern in einem Ereignis, ohne dass es zu größeren b/c-Problemen
kommt (da jetzt definierte Methoden aufgerufen werden und Entwickler
nicht von einer Eigenschaft abhängig sind, die das zweite Argument der
Funktion ist).

#### Wie die maximale Kompatibiltät mit Joomla! 3 erreicht wird

Falls man ein Ereignis auf eine ähnliche Weise wie im entfernten J3.x
JEventDispatcher auslösen will, kann folgender Code verwendet werden:

```php
    $results = \Joomla\CMS\Factory::getApplication()->triggerEvent( '',  );
```

Es ist wichtig darauf hinzuweisen, dass sich die Parameter in einem
Array befinden müssen. Die Funktion des Plugins selbst wird die
Parameter als Ereignisobjekt erhalten, falls das SubscriberInterface
implementiert und als einzelne Werte, falls nicht. Trotzdem wird diese
Vorgehensweise immer einen Array ausgeben, das das Plugin zurückgibt.

Hinweis: Falls IRGENDEIN Plugin in einer Gruppe das SubscriberInterface
nicht implementiert, wird die Ergebniseigenschaft (als sowohl ein
benannter Parameter und als Ergebnis von einem Plugin) als eine
spezielle Eigenschaft verwendet und ist somit nutzlos.

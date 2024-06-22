<!-- Filename: Deploying_an_Update_Server / Display title:  Einen Update-Server bereitstellen -->

Joomla!  2.5 Joomla!  3.x Joomla!  4.x

## Einleitung

Dieses Tutorial soll Entwicklern helfen, einen Update-Server für die
Integration mit dem in Joomla! eingeführten Update-System zu erstellen.
Durch Hinzufügen eines Update-Server-Verzeichnisses zur Manifest-Datei
der Erweiterung können Benutzer ihre Erweiterungen mit nur wenigen
Klicks über die Ansicht „Update“ des Erweiterungs-Managers (siehe
[Joomla
3.x](https://docs.joomla.org/Help31:Extensions_Extension_Manager_Update "Special:MyLanguage/Help31:Extensions Extension Manager Update")-Hilfebildschirm)
aktualisieren.

## Einen Update-Server festlegen

Um diese Funktion nutzen zu können, muss ein Update-Server in der
Manifest-Datei der Erweiterung definiert sein. Diese Definition kann in
allen zu Joomla! 2.5 (und neuer) kompatiblen Erweiterungen verwendet
werden. Sie ist jedoch **nicht** für Templates verfügbar. Es gibt zwei
Optionen für den Server-Typ: „collection“ oder „extension“. Eine
ausführliche dazu Beschreibung folgt im nächsten Abschnitt. Dieser Code
sollte in der Manifest-Datei der Erweiterung innerhalb des Root-Elements
*extension* eingefügt werden. Der Update-Server wird so für jeden Typ
definiert:

```xml
<extension>
<...>
<updateservers>
  <server type="collection">https://example.com/list.xml</server>
  <server type="extension" priority="2" name="My Extension's Updates">http://example.com/extension.xml</server>
</updateservers>
</extension>
 ```

Innerhalb des Tags können mehrere Server definiert werden. Wenn mehrere
Update-Server vorhanden sind, kann für jeden eine andere Priorität
festgelegt werden. Auf diese Weise kann man die Reihenfolge steuern, in
der die einzelnen Update-Server geprüft werden.

## Server Typen

### Collection

Der Servertyp `"collection"` erlaubt Entwicklern in der Manifest-Datei
einer Erweiterung zu definieren, Aktualisierungen aus einer
Auswahl-Liste abzurufen. Dieser Servertyp kann verwendet werden, wenn
der Entwickler alle Aktualisierungen seiner Erweiterung in einer
einzigen Datei festlegen möchte (nicht empfohlen) oder wenn seine
Erweiterung mehrere Untererweiterungen enthält, die nicht gleichzeitig
verteilt oder aktualisiert werden sollen (z.B. ein
Paket-Erweiterungstyp). Das folgende Beispiel zeigt die
`"collection"`-Definition, die der Updater bei der Verarbeitung der
Joomla! Aktualisierung nutzt:

```xml
<extensionset name="Joomla Core" description="Joomla! Core">
    <extension name="Joomla" element="joomla" type="file" version="1.7.0" detailsurl="https://update.joomla.org/core/extension.xml"/>
</extensionset>
```

Alle Definitionen müssen zwischen -Tags in der „collection“
Manifest-Datei definiert werden. Das -Tag verfügt über zwei optionale
Parameter: „name“ und „description“. Für jede Erweiterung, auf die diese
„Sammlung“ verweist, ist ein separates -Tag erforderlich. Das -Tag
verfügt über die folgenden Parameter, die alle für die ordnungsgemäße
Verarbeitung von Updates erforderlich sind:

- **name** – Der Name der Erweiterung
- **element** – Der nicht übersetzte Erweiterungsname, d.h. mod_custom
- **type** – Der Erweiterungs-Typ (component, module, plugin, usw.)
- **version** – Die neueste Version der Erweiterung
- **detailsurl** – Die URL der XML-Datei, die die individuellen
  Aktualisierungsdefinitionen dieser Erweiterung enthält.

### Extension

Der Server-Typ `"extension"` erlaubt Entwicklern in der Manifest-Datei
einer Erweiterung zu definieren, um Aktualisierungen aus der
Manifest-Datei einer einzelnen Erweiterung abzurufen. Alle
Collection-Manifest-Dateien verweisen schließlich auf diese XML-Datei.
Alle Aktualisierungen in dieser Datei müssen nach dem Tag am Anfang der
Datei definiert werden. Das folgende Beispiel ist die Update-Definition
für die Joomla! 3.9.6 Veröffentlichung:

```xml
<updates>
	<update>
		<name>Joomla! 3.9</name>
		<description>Joomla! 3.9 CMS</description>
		<element>joomla</element>
		<type>file</type>
		<version>3.9.6</version>
		<infourl title="Joomla!">https://www.joomla.org/announcements/release-news/5765-joomla-3-9-6-release.html</infourl>
		<downloads>
			<downloadurl type="full" format="zip">https://downloads.joomla.org/cms/joomla3/3-9-6/Joomla_3.9.6-Stable-Update_Package.zip</downloadurl>
			<downloadsource type="full" format="zip">https://github.com/joomla/joomla-cms/releases/download/3.9.6/Joomla_3.9.6-Stable-Update_Package.zip</downloadsource>
			<downloadsource type="full" format="zip">https://update.joomla.org/releases/3.9.6/Joomla_3.9.6-Stable-Update_Package.zip</downloadsource>
		</downloads>
		<tags>
			<tag>stable</tag>
		</tags>
		<sha256>05157273aadd3045564ee44373ea3643b437fa5321d17993a3119b38b04578e2</sha256>
		<sha384>ebd9b0666fbe84e20a420a4bcd6c10d306fc4dee4edbbe8e2133c85f0fb84e59be5a50aa97cb38c068b77f77f6bbc091</sha384>
		<sha512>a4c47644ceeaeec28944e0c74160203cf12037e0ea1439022e95055dfb6716de172667ce6d9164f12bb519d9cfcf1fdc728abea00f853b41debc7d2740f2b711</sha512>
		<maintainer>Joomla! Production Department</maintainer>
		<maintainerurl>https://www.joomla.org</maintainerurl>
		<section>STS</section>
		<targetplatform name="joomla" version="3.[789]" />
		<php_minimum>5.3.10</php_minimum>
</update>
</updates>
```

Der folgende Abschnitt beschreibt die Elemente einer einzelnen
Update-Entität.

- **name** – Der Name der Erweiterung, der Name wird in der
  „name“-Spalte der Update-Ansicht des Erweiterungs-Managers angezeigt
  (erforderlich)
- **description** – Eine kurze Beschreibung der Erweiterung (optional) –
  Wenn Sie verwenden, wird die HTML-Formatierung durch doppelte
  Anführungszeichen unterbrochen. Verwenden Sie deshalb einfache
  Anführungszeichen für Ihre HTML-Entitäten.
- **element** – Der installierte Name der Erweiterung (erforderlich).
  Bei Plugins muss dies mit dem Plugin-Attributwert für die Hauptdatei
  im Plugin-Manifest übereinstimmen. Für pluginname.php sollte der
  Elementwert **pluginname** sein.
- **type** – Der Erweiterungs-Typ (component, module, plugin, usw.)
  (erforderlich)
- **folder** – Speziell für Plugins beschreibt dieses Tag den Typ des zu
  aktualisierenden Plugins (content, system, usw.) (für Plugins
  erforderlich)
- **client** – Die Client-ID der Erweiterung. Für Module und Templates
  ab 3.2.0 erforderlich. Mögliche Werte sind derzeit "site" oder
  "administrator". **Warnung!** Plugins und Front-End-Module werden
  automatisch mit dem Client "site" installiert, aber man muss den
  Client in einer Aktualisierung angeben, sonst wird er standardmäßig
  auf "administrator" gesetzt und die gefundene Aktualisierung wird
  nicht angezeigt, weil sie keiner Erweiterung entspricht. Komponenten
  werden automatisch mit dem Client "administrator" installiert, der
  Standardeinstellung des Systems.
  - **Warnung**: Ab Joomla! 4.0 ist nur noch ein String erlaubt. Die
    Verwendung von Zahlen für Client ist seit 2012 veraltet und wurde in
    Joomla! 4.0 entfernt (0 muss mit *site* und 1 mit *administrator*
    ausgetauscht werden).
  - **Warnung**: Der Name des Tags ist für Joomla! 2.5 \<**client**\>
    und \<**client_id**\> für 1.6 und 1.7. (statt ) wird auf einer 2.5
    Site ignoriert, falls es benutzt wird.
- **version** – Die Version des Releases (erforderlich)
- **infourl** – Eine URL, über die Benutzer darauf hingewiesen werden,
  Informationen zum Update zu enthalten (optional) (In CMS 2.5 wird
  diese URL, sofern festgelegt, in der Update-Ansicht angezeigt.)
- **downloads** – Der Abschnitt, in dem alle Download-Orte aufgelistet
  sind
  - **downloadurl** – Die URL zum Herunterladen der Erweiterung; das
    -Tag hat zwei erforderliche Parameter:
    - **type** – Der Paket-Typ (full oder upgrade)
    - **format** – Das Paket-Format (zip, tar, usw.)
  - **downloadsource** – Optional. Seit Joomla 3.8.3. Alternative URL
    zum Herunterladen der Erweiterung wenn die Verbindung zu
    fehlschlägt. Es sind mehrere -Tags zulässig. Das -Tag erwartet zwei
    erforderliche Parameter:
    - **type** – Der Paket-Typ (full oder upgrade)
    - **format** – Das Paket-Format (zip, tar, usw.)
  - **NB** – es darf kein Zeilenumbruch vor oder nach der URL geben; es
    muss sich alles in einer Zeile befinden oder Sie erhalten einen
    Fehler beim Verbinden mit dem Server: beschädigt, wenn das Update
    ausgeführt wird.
- **changelogurl** - Ein Link zu einer xml-Datei mit dem Changelog. Ab
  Joomla 4.0 kann eine Schaltfläche angezeigt werden, die das
  Änderungsprotokoll auf der Seite des Erweiterungs-Updaters anzeigt.
  Mehr Details über die Verwendung ist hier [„Hinzufügen von Changelog
  zu Ihrer
  Manifestdatei“](https://docs.joomla.org/Adding_changelog_to_your_manifest_file/de "Adding changelog to your manifest file/de")
  zu finden.
- **tags** – Eine Liste der Tags, die für diese Version relevant sind.
  Joomla! 3.4 und höher verwendet dieses Tag, um die Stabilitätsstufe
  des Updates zu bestimmen. Die gültigen Tags sind:
  - *dev*: Entwickler-Versionen, sehr instabil und Pre-Alpha (z.B.
    nightly builds)
  - *alpha*: Alpha Software-Qualitätsniveau (Funktionen nicht
    implementiert, Blockieren durch gravierende Fehler)
  - *beta*: Beta Software-Qualitätsniveau (alle Funktionen
    implementiert, Fehler blockieren ist möglich, Sicherheit bei
    kleineren Fehlern)
  - *rc*: Release Candidate Software-Qualitätsniveau (keine gravierenden
    Fehler, kleinere Fehler noch möglich)
  - *stable*: Software-Qualitätsniveau Produktion. Alle anderen Tags
    werden derzeit ignoriert. Wenn Sie mehr als ein Tag angeben, das
    eines der oben genannten Stabilitäts-Schlüsselwörter enthält, wird
    nur das letzte Tag berücksichtigt. Wenn Sie keine Tags angeben, geht
    Joomla! davon aus, dass es sich um eine stabile Version handelt..
- **maintainer** – Der Name des Erweiterungs-Betreuers (ähnlich dem -Tag
  in einer Manifest-Datei) (optional)
- **maintainerurl** – Die Website des Betreuers der Erweiterung (ähnlich
  dem -Tag in einer Manifest-Datei) (optional)
- **section** – Optional (unbekannte Verwendung)
- **targetplatform** – Ein Tag zum Definieren der
  Plattform-Anforderungen erfordert die folgenden Elemente: (ab
  <img src="https://docs.joomla.org/images/5/55/Compat_icon_3_10.png"
  decoding="async" data-file-width="40" data-file-height="17" width="40"
  height="17" alt="Joomla 3.10" /> wird dies auch verwendet, um
  Erweiterungskompatibilität für die Joomla-Update-Komponente zu
  erkennen), erfordert folgende Elementeː
  - **name** – Der Name der Plattform-Abhängigkeit. Zum Zeitpunkt der
    Erstellung dieses Textes sollte es **nur** „joomla“ sein.
  - **version** – Die Version von Joomla!, die die Erweiterung
    unterstützt
  - **min_dev_level** and **max_dev_level** – Diese Attribute wurden in
    3.0.1 hinzugefügt, damit Sie eine Zielplattform basierend auf der
    Entwicklerebene auswählen können („z“ in x.y.z). Sie sind optional.
    Sie können eine oder beide angeben. Wenn nichts angegeben ist,
    werden alle Entwicklerebenen abgeglichen. Das folgende Beispiel
    stimmt mit den Versionen 4.0.0 und 4.0.1 überein.
    - **Hinweis:** Wenn Ihre Erweiterung mit Joomla! 2.5 und/oder 3.1
      kompatibel ist, müssen Sie separate -Definitionen für jede Version
      haben, da der Updater die Version überprüft, wenn Sie eine Nummer
      angeben. Um Ihre Erweiterung jedoch auf allen Joomla-Versionen
      anzuzeigen, die automatische Updates unterstützen, fügen Sie hinzu
      (und damit als kompatibel mit allen zukünftigen unveröffentlichten
      Versionen von Joomla in Joomla Update zu markieren). Wenn Ihre
      Erweiterung auf allen
      <img src="https://docs.joomla.org/images/4/4d/Compat_icon_3_x.png"
      decoding="async" data-file-width="40" data-file-height="17" width="40"
      height="17" alt="Joomla 3.x" />-Versionen angezeigt werden soll,
      anstatt eine Version im Versions-Tag anzugeben, fügen Sie hinzu.
      Dadurch wird das Update auf allen 3.x-Versionen von Version 3.0
      bis 3.5 angezeigt. Wenn Sie Version 3.10 einbinden möchten, können
      Sie ein `|` wie dieses verwenden: . Wenn Sie die Updates für alle
      3.8.x-Versionen und alle 3.10.x-Versionen anzeigen möchten, können
      Sie verwenden.
- **php_minimum** – Von Version 3.2.2 an kann eine „Mindest“-PHP-Version
  in den Update-Stream eingetragen werden, ab der Unterstützung besteht.
  Wenn der Server das Minimum nicht erreicht, wird dem Benutzer eine
  Meldung angezeigt, dass ein Update verfügbar ist, aber aufgrund nicht
  unterstützter Anforderungen nicht installiert werden kann.
- **supported_databases** – Ab Version 3.7 kann eine unterstützte
  Datenbank + Mindestversion im Update-Stream eingetragen werden. Wenn
  der Server die Mindestanforderungen nicht erfüllt, wird dem Benutzer
  eine Meldung angezeigt, dass ein Update verfügbar ist, das jedoch
  aufgrund nicht unterstützter Anforderungen nicht installiert werden
  kann. Anmerkung:
  <a href="https://github.com/joomla/joomla-cms/pull/26079"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Ab 3.9.12</a> gibt es auch einen
  Sonderfall für mariadb, so dass sie jetzt auch dafür eine bestimmte
  Mindestversion festgelegt werden kann.
  - Ein Beispiel könnte so aussehen:
- **sha256**, **sha384**, **sha512** – Optional. Seit Joomla 3.9.0
  können Sie Dateiprüfsummen in diesen Hash-Formaten hinzufügen.
  Beachten Sie, dass in Joomla 3 nur ein Hinweis auf **Updates**
  angezeigt wird, wenn eine Prüfsumme nicht korrekt ist. Das ist alles –
  das Update wird trotzdem fortgesetzt. In Joomla 4 werden Updates und
  Installationen angehalten, wenn eine bereitgestellte Prüfsumme nicht
  übereinstimmt.

Für jede Version Ihrer Erweiterung, die Sie freigeben, ist eine separate
-Definition erforderlich.

Die Werte von **element**, **type**, **client_id** und **folder**
sollten mit denen in der Tabelle \#\_extensions übereinstimmen.

**Wichtig für Plugins:** Plugins müssen die Elemente und enthalten,
damit sie ordnungsgemäß funktionieren.

## Fehlersuche und -behebung

- **SQL Update-Skript wird während des Updates nicht ausgeführt.**

Wenn das SQL-Update-Skript (z.B. im Ordner `sql/updates/mysql`) während
des Aktualisierungsprozesses nicht ausgeführt wird, kann es daran
liegen, dass es für diese Erweiterung *vor dem Update* in der Tabelle
`#_schemas` keine Versionsnummer gibt. Dieser Wert wird durch den
letzten Skriptnamen im Ordner SQL-Updates bestimmt. Wenn dieser Wert
leer ist, werden während dieses Aktualisierungszyklus keine SQL-Skripte
ausgeführt. Um sicherzustellen, dass dieser Wert korrekt eingestellt
ist, stellen Sie sicher, dass Sie in diesem Ordner ein SQL-Skript mit
seinem Namen als Versionsnummer haben (z.B. 1.2.3.3.sql, wenn die
Version 1.2.3 ist). Die Datei kann leer sein oder einfach nur eine
SQL-Kommentarzeile haben. Dies sollte in der alten Version erfolgen –
derjenigen vor dem Update. Alternativ können Sie diesen Wert auch über
eine SQL-Abfrage zum `#_schemas` hinzufügen.

## Support-Tools

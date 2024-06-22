<!-- Filename: J4.x:Improved_Override_Management / Display title: Verbesserte Overrideverwaltung -->

<span id="main-portal-heading">GSoC 2018  
Anleitung  
für die verbesserte Overrideverwaltung</span> [<img
src="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/75px-Gsoc2016.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/113px-Gsoc2016.png 1.5x, https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/150px-Gsoc2016.png 2x"
data-file-width="373" data-file-height="373" width="75" height="75"
alt="Gsoc2016.png" />](https://docs.joomla.org/GSOC_2018 "GSOC 2018")
Joomla!  4.x

## Einleitung

Dieses Projekt fügt zu Joomla! eine Einrichtung zur
Aktualisierungprüfung hinzu, dass heißt wenn eine Kerndatei eines
Template Overrides verändert oder aktualisiert wird, erhält der Benutzer
eine Nachricht, dass eine der Kerndateien seines Template Override sich
mit der Aktualisierung verändert hat, um Sicherheitsprobleme oder den
Verlust der Funktionsfähigkeit zu vermeiden, außerdem können sie ihren
Override anpassen bevor irgendjemand etwas bemerkt.

**Link für die Projektpaketquelle:**
<a href="https://github.com/joomla-projects/gsoc18_override_management"
class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla-projects/gsoc18_override_management</a>

## Erhalte einen Vorsprung mit der verbesserten Overrideverwaltung

### Hinweise

Bitte lies zuerst, falls du neu in der Joomla! Entwicklung bist und noch
nicht viel über die Templateverwaltung und Overrides weißt:

1.  [Verwendung des
    Template-Managers](https://docs.joomla.org/J3.x:How_to_use_the_Template_Manager "Special:MyLanguage/J3.x:How to use the Template Manager")
2.  [Layout Overrides in
    Joomla!](https://docs.joomla.org/Layout_Overrides_in_Joomla "Special:MyLanguage/Layout Overrides in Joomla")

Jetzt, wo du mit der Verwendung des Template Managers und den Typen von
Overrides in Joomla! vertraut bist, können wir uns die Merkmale des
Projekts näher anschauen.

- Unterstützte Overrides
- Diff View
- Override - Quick Icon Notification Plugin
- Installer - Override Plugin
- Aktualisiert - Overrideverlauf

## Von dieser Funktion unterstützte Typen von Overrides

Es unterstützt keine js und css overrides und keine alternativen
Layouts, in denen Dateinamen zu etwas anderem verändert wurden.

<img
src="https://docs.joomla.org/images/thumb/a/aa/Selection_035-en.png/800px-Selection_035-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/a/aa/Selection_035-en.png/1200px-Selection_035-en.png 1.5x, https://docs.joomla.org/images/thumb/a/aa/Selection_035-en.png/1600px-Selection_035-en.png 2x"
data-file-width="1907" data-file-height="956" width="800" height="401"
alt="Selection 035-en.png" />

## Diff view zwischen Kern- und Overridedateien

Diese Funktion zeigt den Unterschied zwischen den Kern- und den
Overridedateien an. Wenn irgendeine Overridedatei zur Bearbeitung
geöffnet wird, sind zwei Schaltflächen oder Schalter in der rechten
oberen Ecke der Seite zu sehen, ob die Kerndatei angezeigt werden soll.

Diese Schalter:

<img src="https://docs.joomla.org/images/6/66/Selection_027-en.png"
decoding="async" data-file-width="421" data-file-height="197"
width="421" height="197" alt="Buttons" />

Hier können die Ansichten diff view und Anzeige der Kerndateien
umgeschaltet werden. Im nächsten Bild sieht man den Standort der
Kerndatei und den diff view im Templatemanager.

<img
src="https://docs.joomla.org/images/thumb/f/f2/Selection_028-en.png/800px-Selection_028-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/f/f2/Selection_028-en.png/1200px-Selection_028-en.png 1.5x, https://docs.joomla.org/images/thumb/f/f2/Selection_028-en.png/1600px-Selection_028-en.png 2x"
data-file-width="1904" data-file-height="952" width="800" height="400"
alt="Selection 028-en.png" />

Die Kerndatei kann nicht verändert werden.

### Wie diff view funktioniert

Wenn man zur Bearbeitung auf eine beliebige Overridedatei klickt, wird
die Funktion `getCoreFile` aufgerufen, welche den Parameter `path` der
Overridedatei in dem Template erhält. Beispiel:
`/html/layouts/joomla/form/field/user.php` zeigt die Datei und den
Clientpfad der Datei und ob diese zu `Site` oder `Administrator` gehört.
Diese Informationen zu Grunde gelegt gibt es den Pfad der Kerndatei aus,
falls sie existiert. Um die Unterschiede zwischen den Kern- und
Overridedateien zu zeigen nutzten wir die Bibliothek unter
<a href="https://github.com/kpdecker/jsdiff" class="external text"
target="_blank" rel="nofollow noreferrer noopener">jsdiff</a>

## Override - Quick Icon Notification Plugin

Ein quick icon notification Plugin, welches die Benachrichtigungen im
cpanel und die Summe der aktualisierten Overrides von allen Templates
zeigt. Werden Overrides aktualisiert zeigt das quick icon so was
Ähnliches wie im Beispiel unten:

<img
src="https://docs.joomla.org/images/thumb/5/51/Selection_020-en.png/800px-Selection_020-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/5/51/Selection_020-en.png 1.5x"
data-file-width="1080" data-file-height="309" width="800" height="229"
alt="Quick Icon" />

Mit einem Klick darauf wird man auf die Templates umgeleitet, dort ist
eine Liste deiner Templates mit Beschreibung enthalten. Es erscheint
eine neue Spaltenüberschrift **Overrides**, welche die Anzahl der zum
Template gehörenden geänderten Overrides zeigt. Wurde nichts im Template
Override verändert zeigt es ein Up to date Emblem.

<img
src="https://docs.joomla.org/images/thumb/d/d6/Selection_022-en.png/800px-Selection_022-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/d/d6/Selection_022-en.png/1200px-Selection_022-en.png 1.5x, https://docs.joomla.org/images/thumb/d/d6/Selection_022-en.png/1600px-Selection_022-en.png 2x"
data-file-width="1897" data-file-height="662" width="800" height="279"
alt="Templates" />

### Wie das quick icon funktioniert

Ein AJAX Aufruf von `TemplateController.php` gibt die Informationen
zurück und zeigt eine Benachrichtigung wenn solche Overrides
aktualisiert werden.

**Warnung**

Das Plugin quick icon notification funktioniert nur wenn das
`installer/override` Plugin aktiv ist. Wenn `installer/override`
deaktiviert ist, wird eine Fehlermeldung in quick icon erscheinen.

<img
src="https://docs.joomla.org/images/thumb/9/9d/Selection_024-en.png/800px-Selection_024-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/9/9d/Selection_024-en.png/1200px-Selection_024-en.png 1.5x, https://docs.joomla.org/images/9/9d/Selection_024-en.png 2x"
data-file-width="1300" data-file-height="333" width="800" height="205"
alt="Enable" />

Mit einem Klick auf quick icon wird man auf das `installer/override`
Plugin umgeleitet wo man die Einstellungen für das Plugin ändern kann.

## Installer - Override Plugin

Dieses Plugin ist das Kernelement dieser Funktion. Es ermöglicht es, die
richtigen aktualisierten Overrides während der Erweiterungsinstallation
und Aktualisierung von Joomla! zu finden.

<img
src="https://docs.joomla.org/images/thumb/f/fe/Selection_025-en.png/800px-Selection_025-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/f/fe/Selection_025-en.png/1200px-Selection_025-en.png 1.5x, https://docs.joomla.org/images/thumb/f/fe/Selection_025-en.png/1600px-Selection_025-en.png 2x"
data-file-width="1887" data-file-height="719" width="800" height="305"
alt="Installer Override" />

### Wie das Plugin installer override funktioniert

Dieses Plugin arbeitet bei sechs Ereignissen:

- onExtensionBeforeUpdate
- onExtensionAfterUpdate
- onInstallerBeforeInstaller
- onInstallerAfterInstaller
- onJoomlaBeforeUpdate
- onJoomlaAfterUpdate

Es sammelt `md5_file()` die Hashwerte aller Overrides Kerndateien vor
und nach einem Update und vergleicht dann beide Werte miteinander. Dann
findet es die richtig veränderter oder aktualisierte Datei. Schließlich
wird die Information in der Tabelle `#__templates_overrides` abgelegt.

## Aktualisierter Overrideverlauf

Die Funktion erreicht man über den Reiter aktualisierte Dateien in einem
Template. In einer Listenansicht werden alle aktualisierten Overrides,
die zu diesem Template gehöreen, gezeigt.

<img
src="https://docs.joomla.org/images/thumb/b/b9/Selection_029-en.png/800px-Selection_029-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/b/b9/Selection_029-en.png/1200px-Selection_029-en.png 1.5x, https://docs.joomla.org/images/thumb/b/b9/Selection_029-en.png/1600px-Selection_029-en.png 2x"
data-file-width="1901" data-file-height="737" width="800" height="310"
alt="Selection 029-en.png" />

Es gibt viele vorhandene Möglichkeiten diese Liste zu verwalten. Hier
kann ein Klick gesetzt werden, ob der Status des Datenverlaufs ungeprüft
oder geprüft ist. Außerdem gezeigt werden das Erstelldatum, das Datum
der letzten Veränderung und einige Aktualisierungsaktionen, etwa Joomla!
Update, Erweiterungsupdate oder Erweiterungsinstallation.

**Anmerkung**

Diese Informationen beziehen sich nur auf die Vergangenheit, wenn also
die aktualisierten Overrideveränderungen geprüft wurden, kann der nun
unnötige Verlauf gelöscht werden.

**Schau Dir das Video-Tutorial an, um mehr über das Feature zu lernen**

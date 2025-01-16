<!-- Filename: https://docs.joomla.org/Package / Display title: Pakete -->

## Einführung in Pakete

Manchmal verwendet ein Modul (oder Plugin oder eine andere Erweiterung) die Modelle oder Helfer einer Komponente. In diesen Fällen ist es am besten, ein abhängiges Modul installieren oder deinstallieren zu können, wenn die Komponente selbst installiert oder deinstalliert wird. In Joomla wird diese Funktionalität mit einem Paket erreicht, obwohl die Verwendung von Paketen nicht narrensicher ist.

Ein Paket ist eine Erweiterung, die verwendet wird, um mehrere Erweiterungen gleichzeitig zu installieren. Sie in einem Paket zu kombinieren, ermöglicht es dem Benutzer, zwei oder mehr Erweiterungen in einem einzigen Vorgang zu installieren und zu deinstallieren.

## Paket-Erstellung

Ein Paket wird erstellt, indem alle einzelnen Erweiterungs-`.zip`-Dateien zusammen mit einer `.xml`-Manifestdatei gezippt werden. Zum Beispiel, für ein Paket bestehend aus:

* **Komponente** helloworld
* **Modul** helloworld
* **Bibliothek** helloworld
* **System-Plugin** helloworld
* **Vorlage** helloworld

Das Paket hätte im Zip-Archiv die folgende Baumstruktur:

```sh
  -- pkg_helloworld.xml
  -- packages <dir>
      |-- com_helloworld.zip
      |-- mod_helloworld.zip
      |-- lib_helloworld.zip
      |-- plg_sys_helloworld.zip
      |-- tpl_helloworld.zip
  -- pkg_script.php
```

Die Datei `pkg_helloworld.xml` könnte folgenden Inhalt haben:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<extension type="package" method="upgrade">
<name>Hello World Package</name>
<author>Hello World Package Team</author>
<creationDate>May 2022</creationDate>
<packagename>helloworld</packagename>
<version>1.0.0</version>
<url>http://www.yoururl.com/</url>
<packager>Hello World Package Team</packager>
<packagerurl>http://www.yoururl.com/</packagerurl>
<description>Example package to combine multiple extensions</description>
<update>http://www.updateurl.com/update</update>
<scriptfile>pkg_script.php</scriptfile>
<blockChildUninstall>true</blockChildUninstall>
<files folder="packages">
    <file type="component" id="com_helloworld">com_helloworld.zip</file>
    <file type="module" id="helloworld" client="site">mod_helloworld.zip</file>
    <file type="library" id="helloworld">lib_helloworld.zip</file>
    <file type="plugin" id="helloworld" group="system">plg_sys_helloworld.zip</file>
    <file type="template" id="helloworld" client="site">tpl_helloworld.zip</file>
</files>
</extension>
```

Wenn das Paket gezippt und installiert ist, wird es in der Erweiterungsliste sichtbar sein, sodass ein Benutzer alle im Paket enthaltenen Erweiterungen deinstallieren kann.

Denken Sie daran, den Paket-Deinstallationsprogramm anstelle einzelner Unterpaket-Deinstallationsprogramme zu verwenden, um verwaiste Erweiterungseinträge im Erweiterungsmanager zu vermeiden. Das ist der Teil, der nicht narrensicher ist!

### Datei-ID

Das id-Element im `<file ..>`-Tag ist **nicht** willkürlich! Das `id=` sollte auf den Wert der `element`-Spalte in der `#__extensions`-Tabelle gesetzt werden. Wenn sie nicht korrekt gesetzt sind, wird das untergeordnete `file` beim Deinstallieren des Pakets **nicht** gefunden und deinstalliert.

### Manifestdateiname und Paketname

Die Benennung der Manifestdatei und die Fähigkeit, die Paketdatei selbst zu deinstallieren, sind eng miteinander verknüpft. Die Manifestdatei muss das Präfix **pkg_** haben. Der Rest des Manifestnamens (ohne die **.xml**-Erweiterung) wird als `<packagename>` verwendet. Oder andersherum: Ein Paket, das Sie als **blurpblurp_J3** identifizieren möchten, erhält dies als `<packagename>` und sollte in einer Manifestdatei namens **pkg_blurpblurp_J3.xml** sein. Andernfalls wird es unmöglich sein, das Paket selbst zu deinstallieren.

Ein optionales `<pkg_script.php>`, das eine Klasse `pkg_<packagename>InstallerScript` mit der öffentlichen Funktion **postflight** enthält, kann verwendet werden.

### Erweiterungs-Tag

Der `<extension>`-Tag sollte `method="upgrade"` enthalten, damit ein Paket-Update bei nachfolgenden Installationen erfolgreich ist.

### Erweiterung Deinstallationsabhängigkeit

Eine Paket-Erweiterung kann deklarieren, dass ihre untergeordneten Elemente nicht unabhängig deinstalliert werden können, indem sie ein `<blockChildUninstall>true</blockChildUninstall>`-Element im Paketmanifest verwendet. Wenn Ihr Paket alle seine Erweiterungen benötigt, um effektiv zu funktionieren, setzen Sie dies auf true oder 1.

*Übersetzt von openai.com*


<!-- Filename: J4.x:MVC_Anatomy:_File_Structure / Display title: MVC Anatomie: Dateistruktur -->

## Entwickler-Setup

Es gibt mehrere Möglichkeiten, Dateien für Entwicklungszwecke zu organisieren. Eine Methode besteht darin, ein Klon eines GitHub-Repositories zu verwenden, wie in der folgenden schematischen Struktur:

```
cefjdemos-com-countrybase
|-- .vscode (folder for build settings)
|-- com_countrybase (folder compressed to create an installable zip file)
    |-- admin (the administrator files)
        |-- forms
        |-- help
            |-- countrybase.html (this is a Help screen used in the backend module edit form)
        |-- language
            |-- en-GB (language folder, kept with the extension code)
                |-- mod_downmsg.ini
                |-- mod_downmsg.sys.ini
        |-- services (folder for dependency injection)
            |-- provider.php (the DI code)
        |-- sql
            |-- updates
                |-- mysql
                    |-- index.html (an empty file required to avoid an installation error if there are no sql updates)
        |-- src (folder for namespaced classes)
            |-- more folders: Controller, Extension, Helper, Model, Table and View
        |-- tmpl (folder for layouts)
            |-- more folders: countries, country, currencies and currency
        |-- access.xml (standard list of access permissions)
        |-- config.xml (options parameters)
    |-- media
        |-- css (placeholder for CSS files - contains an empty index.html file)
        |-- js (placeholder for JavaScript files - contains an empty index.html file)
    |-- site (the site files, abbreviated here)
        |-- forms
        |-- language
        |-- src
        |-- tmpl
    |-- countrybase.xml (the manifest file)
    |-- script.php (a script to run on install, update or uninstall)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- changelog.xml (record of changes)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
|-- updates.xml (update server specification)
```

Dies ist die Struktur in der VSCodium-IDE:

![Vscodium-Dateistrukturansicht](../../../en/images/mvc-anatomy/com-countrybase-vscodium.png)

Bei der Installation werden die Teile der `com_countrybase` Komponente an verschiedene Orte in der Joomla-Dateistruktur verteilt:
- Administrator-Dateien gehen in `root/administrator/components/com_countrybase`.
- Seiten-Dateien gehen in `root/components/com_countrybase`.
- Mediendateien, JavaScript- und CSS-Dateien (falls vorhanden), gehen in `root/media/com_countrybase`.
- Sprachdateien gehen in die Administrator- und Seitenkomponentenstrukturen. Eine frühere Konvention platzierte sie in den Kernsprachenordnern.

Der genaue Ort, an dem die Beiträge platziert werden, wird durch die Komponentendatei manifest gesteuert, in diesem Fall countrybase.xml.

Beachten Sie, dass die ZIP-Datei alles im Ordner com_countrybase enthält. Sie können eine ZIP-Datei erstellen, indem Sie einfach diesen Ordner komprimieren. Außerhalb dieses Ordners befinden sich build.xml, eine Datei, die zum Erstellen der Komponente verwendet wird, wenn eine Änderung vorgenommen wird, und README.md, eine Standard-Markdown-Datei im Github-Format, die die Komponente beschreibt.

## Notizen

- Es gibt mehr als 40 Dateien in dieser einfachen Komponente!
- Bei der Installation wird die Datei countrybase.xml nach administrator/components/com_countrybase kopiert, wo sie für Aktualisierungs- oder Deinstallationszwecke benötigt wird.

*Übersetzt von openai.com*


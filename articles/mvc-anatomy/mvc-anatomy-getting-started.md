<!-- Filename: J4.x:MVC_Anatomy:_Getting_Started / Display title: MVC Anatomie: Erste Schritte -->

## Einführung

Joomla-Komponenten folgen dem Model-View-Controller-Ansatz, oder kurz MVC. Das Modell soll das Laden und Speichern von Daten verwalten. Die Ansicht soll die Anzeige von Daten verwalten. Und die Steuerung soll den Programmfluss sowie die Interaktion zwischen dem Komponentenmodell und dem Ansichtscode verwalten.

Wenn Sie Ihre eigene Komponente erstellen möchten, gibt es zwei Ansätze zum Lernen, die Sie möglicherweise übernehmen:

- **Bauen Sie es Schritt für Schritt:** Indem Sie einem einfachen Tutorial folgen, das jede Phase beschreibt.
- **Zerlegen Sie es Datei für Datei:** Indem Sie eine funktionierende Komponente auseinandernehmen, um zu lernen, wie jeder Teil funktioniert.

Dieses Tutorial verwendet den letzteren Ansatz. Die für das Tutorial verwendete Komponente heißt com_countrybase. Sie wird verwendet, um einige grundlegende Daten über die Länder der Welt zu verwalten und anzuzeigen. Sie ist tatsächlich nützlich in ihrer eigenen Funktionalität und könnte als Grundlage für den Bau von etwas Komplexerem dienen.

## Übersicht der Ziele

Welches Projekt Sie auch immer haben, Sie sollten mit einer Gliederung Ihrer Ziele beginnen und sich vielleicht fragen, ob diese Ziele mit den Joomla-Kernkomponenten oder einer verfügbaren Erweiterung erreicht werden können. Für com_countrybase wird eine Tabelle mit Ländern-Daten benötigt, zusammen mit Eingabe- und Ausgabeseiten. Und vielleicht ein Modul, um einen täglichen Währungswechselkurs anzuzeigen. Und sogar ein Plugin, das durch einen CLI-Befehl aufgerufen wird, um Währungsdaten in regelmäßigen Abständen zu aktualisieren. Dieses Tutorial behandelt nur die Komponente.

Die Komponente benötigt eine Datenbanktabelle, um Länderdaten und eine Tabelle, um Währungsdaten zu speichern. Jede wird eine Listenansicht und eine Bearbeitungsansicht benötigen. Dies kann nicht mit den Joomla-Kernkomponenten erreicht werden, daher handelt es sich eindeutig um einen Fall für eine benutzerdefinierte Komponente.

## Starter-Tabellen

Es gibt viele Daten über Länder, die aus allen möglichen Quellen in allen möglichen Formaten verfügbar sind. Da es weltweit mindestens 250 Länder gibt, wäre das Eingeben von Daten für jedes Land einzeln über ein Dateneingabeformular ziemlich mühsam. Deshalb habe ich Startertabellen vorbereitet, indem ich Daten aus verschiedenen Quellen gesammelt, diese in einer Tabelle kombiniert und dann SQL-Anweisungen exportiert habe, um die Tabellen zu erstellen.

Die Daten basieren auf ISO 3166-Ländercodes, aber einige sehr bekannte Länder sind nicht enthalten, zum Beispiel England, Schottland und Wales. Sie brauchen sich darüber keine Sorgen zu machen. Die Tabellen werden mit der com_countrybase-Komponente bereitgestellt.

Die Dateien für die com_countrybase-Komponente sind auf GitHub verfügbar. Sie können eine [ZIP](https://github.com/ceford/j4xdemos-com-countrybase/archive/refs/heads/master.zip)-Datei herunterladen und installieren, um zu sehen, wie sie im Administrator-Menü funktioniert. Erstellen Sie einen Menüpunkt, wenn Sie sehen möchten, wie es in Ihrem Site-Template funktioniert. Entpacken Sie die ZIP-Datei in Ihrem Projektdateiverzeichnis, nicht in Ihrem Test-Webseitbaum, um die Komponenten-Dateistruktur und den Dateiinhalt mit Ihrem bevorzugten IDE- oder Textbearbeitungswerkzeug zu untersuchen.

## Screenshot

Diese Administratorliste der Länder enthält fünf Beiträge, um die Bildgröße zu minimieren. Joomla zeigt normalerweise 20 Beiträge an.

![Liste der Länder](../../../en/images/mvc-anatomy/com-countrybase-countries.png)

## Boilerplate-Komponente

Um Ihnen den Einstieg mit Ihrer eigenen Komponente zu erleichtern, steht eine [Beispielkomponente](https://github.com/ceford/j4xdemos-com-bpsrc/archive/refs/heads/master.zip) auf Github zur Verfügung. Laden Sie diese herunter und entpacken Sie sie in Ihrem Projektdateienbereich, nicht im Verzeichnisbaum Ihrer Test-Website. Nach dem Herunterladen nehmen Sie alle Änderungen vor, die in der README angegeben sind, und schon können Sie loslegen.

Es gibt auch eine Reihe von kostenlosen und kommerziellen Erweiterungsgeneratoren, die Sie ausprobieren können, um ein Skelettkomponente für Ihre eigenen Zwecke zu generieren. Der [Joomla! Component Builder](https://www.joomlacomponentbuilder.com/) ist kostenlos und scheint umfassend zu sein.

*Übersetzt von openai.com*


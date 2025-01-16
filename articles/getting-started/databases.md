<!-- Filename: J4.x:Developer:_Required_Software / Display title: Einrichtung der Datenbank -->

## Über MySQL und MariaDB

Neulinge könnten den Eindruck haben, dass MySQL und MariaDB Datenbanken sind und könnten verwirrt sein, wenn Anleitungen sagen *erstelle eine neue Datenbank*. In der Tat sind beide Relationale Datenbankverwaltungssysteme (RDBMS), die viele einzelne Datenbanken verwalten können. Ihre lokale Testseite kann viele einzelne Joomla-Installationen haben, jede mit ihrer eigenen Datenbank. Zum Beispiel möchten Sie vielleicht Ihre Erweiterung in der aktuellen Joomla-Version testen und separat in einem kommenden Release-Kandidaten.

Der folgende Screenshot zeigt einen Teil einer Liste von über 30 Datenbanken, die für das Testen verschiedener Joomla-Installationen und -Erweiterungsprojekte erstellt wurden.

![Phypadmin-Screenshot der Liste der Datenbanken](../../../en/images/getting-started/phpmyadmin-databases.png)

Ein Hinweis am Rande: die Kollationen sind meist utf8mb4_0900_ai_ci:

- utf8mb4 bedeutet, dass jedes Zeichen als maximal 4 Bytes im UTF-8-Codierungsschema gespeichert wird.
- 0900 bezieht sich auf die Version des Unicode Collation Algorithmus.
- ai steht für Akzentunempfindlichkeit, es gibt keinen Unterschied zwischen e, è, é, ê und ë beim Sortieren.
- ci steht für Groß-/Kleinschreibungsunempfindlichkeit, es gibt keinen Unterschied zwischen p und P beim Sortieren.

Einzelne Tabellen und Spalten können eine andere Sortierung haben. Joomla-Tabellen haben typischerweise die Sortierung utf8mb4_unicode_ci.

## Datenbankeinrichtung mit phpMyAdmin

Bevor Sie Joomla installieren, benötigen Sie eine leere Datenbank, die bereit ist, gefüllt zu werden. Sie benötigen auch einen Datenbankbenutzer.

### Eine Datenbank erstellen

- **Starte phpMyAdmin** Gib localhost/phpmyadmin in die URL-Leiste deines Browsers ein.
- **Anmeldung** Abhängig von der Installation wird der Benutzername root oder admin sein, und es kann sein, dass ein Passwort erforderlich ist oder nicht.
- Wähle **Datenbanken** aus dem oberen Menü der phpMyAdmin-Startseite aus.
- Gib unter **Datenbank erstellen** einen kurzen Namen als Ersatz für den Hinweis **Datenbankname** ein, zum Beispiel jtest.
- Wähle **utf8mb4_0900_ai_ci** aus der Drop-down-Liste Kollation aus.
- Wähle **Erstellen**, um die Datenbank zu erstellen.

Das wird Sie zu einer leeren Datenbank führen. Oben befindet sich eine Nachricht: *Keine Tabellen in der Datenbank gefunden.*

### Einen Benutzer erstellen

- Wählen Sie das **Home**-Symbol oben links in phpMyAdmin, um zur Startseite zu gelangen.
- Wählen Sie **Benutzerkonten** aus dem oberen Menü der Startseite.
- Wählen Sie **Benutzerkonto hinzufügen** aus dem neuen Bereich unterhalb der Liste der aktuellen Benutzer (falls vorhanden).
- Geben Sie einen **Benutzernamen** ein, der ein kurzer Name sein kann, den Sie später während der Joomla-Installation benötigen. Beispiel: jtest. Sie können diesen gleichen Benutzer für andere Datenbanken verwenden, die später erstellt werden!
- Wählen Sie **Lokal** aus der Liste des Hostnamens-Felds. Es wird localhost im angrenzenden Wertfeld festlegen.
- Verwenden Sie die **Generieren**-Schaltfläche, um ein Passwort zu generieren. Sie müssen den generierten Wert kopieren und in einen Texteditor einfügen, um ihn später zu verwenden. Sie müssen es sich nicht langfristig merken, da es im Joomla configuration.php-File als Klartext gespeichert wird. Beispiel: 8t8mGQq.gw\[\]8lp(
- **Speichern** Sie die Datenbank- und globalen Berechtigungen-Abschnitte des Formulars für den Benutzer. Der **Los** (Speichern)-Button befindet sich am unteren Ende.

### Datenbankberechtigungen dem Benutzer zuweisen

- Auf der Seite **Benutzerkonten** wählen Sie den Benutzer (jtest) aus, falls erforderlich über Startseite / Benutzerkonten / Benutzername.
- Wählen Sie oben auf der Seite **Datenbank**. Dies zeigt eine Liste von Datenbanken, für die dieser Benutzer Zugriffsberechtigungen erhalten hat, und unten eine Liste von Datenbanken, für die der Benutzer keinen Zugriff erhalten hat.
- **Wählen** Sie die Datenbank aus, für die Zugang gewährt werden soll, und wählen Sie die Schaltfläche **Go**.
- **Datenbankspezifische Berechtigungen** Wählen Sie **Alle auswählen** und dann **Go**, um zu speichern.

Das ist alles! Sie können sich nun von phpMyAdmin abmelden. Haben Sie vergessen, das Datenbank-Benutzerpasswort aufzuschreiben? Gehen Sie zurück und bearbeiten Sie das erstellte Benutzerkonto, um es zu ändern. Wenn Sie denselben Datenbankbenutzer für mehrere Joomla-Datenbanken verwenden, finden Sie das Passwort in einer Joomla configuration.php Datei.

*Übersetzt von openai.com*


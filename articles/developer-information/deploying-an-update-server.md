<!-- Filename: Deploying_an_Update_Server / Display title: Aktualisiere Server -->

## Hintergrund

Es gibt eine gute Beschreibung der Update-Server in der Joomla! Programmierer-Dokumentation, die auf [dieser Seite](jdocmanual?article=docus/install-update/update-server) kopiert wurde oder in der [Originalquelle](https://manual.joomla.org/docs/building-extensions/install-update/update-server/) verfügbar ist.

## Fehlerbehebung

- **SQL-Update-Skript wird während des Updates nicht ausgeführt.**

Wenn das SQL-Update-Skript (zum Beispiel im Ordner `sql/updates/mysql`) während des Update-Prozesses nicht ausgeführt wird, könnte es daran liegen, dass in der Tabelle `#_schemas` vor dem Update keine Versionsnummer für diese Erweiterung vorhanden ist. Dieser Wert wird durch den letzten Skriptnamen im SQL-Update-Ordner bestimmt. Wenn dieser Wert leer ist, werden während dieses Update-Zyklus keine SQL-Skripte ausgeführt. Um sicherzustellen, dass dieser Wert korrekt gesetzt ist, sollten Sie sicherstellen, dass Sie ein SQL-Skript in diesem Ordner haben, dessen Name der Versionsnummer entspricht (zum Beispiel 1.2.3.sql, wenn die Version 1.2.3 ist). Die Datei kann leer sein oder nur eine SQL-Kommentarszeile enthalten. Dies sollte in der alten Version — derjenigen vor dem Update — geschehen. Alternativ können Sie diesen Wert mit einer SQL-Abfrage zur `#_schemas` hinzufügen.

*Übersetzt von openai.com*

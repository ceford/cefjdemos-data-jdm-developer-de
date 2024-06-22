<!-- Filename: J4.x:Inserting_Updating_and_Removing_data_using_JDatabase / Display title:  Einfügen, aktualisieren und löschen von Daten unter Verwendung von JDatabase -->

Joomla!  4.x Joomla!  3.x Joomla!  2.5 Versionshinweis

Hinweis: Online finden sich viele Beispiele, welche *prepared
statements* (vorbereitete Anweisungen), die mit Joomla! 4.x eingeführt
wurden, nicht benutzen. Diese alten Anwendungsprogrammierschnittstellen
(APIs) sollten nicht für neue Projekte verwendet werden, weil sie zu
erheblichen Sicherheitsrisiken führen können, falls eine Benutzereingabe
nicht korrekt bereinigt oder *escaped* wird.

Dieses Tutorial wurde auf zwei unabhängige Teile aufgetrennt:

- Einfügen, aktualisieren und entfernen von Daten der Datenbank.
- Daten aus einer oder mehr Tabellen auswählen und diese in vielen
  verschiedenen Formen erhalten.

Dieser Abschnitt richtet das Augenmerk auf das Einfügen, Aktualisieren
und Entfernen von Daten aus einer Datenbanktabelle. Der andere Teil
befindet sich
[hier](https://docs.joomla.org/J4.x:Selecting_data_using_JDatabase "Special:MyLanguage/J4.x:Selecting data using JDatabase")

## Einführung

Joomla! bietet eine hochentwickelte Datenbank mit hohem
Abstraktionsgrad, um Drittentwicklern die Bedienung zu erleichtern. Neue
Versionen der Joomla! Plattform API liefern zusätzliche Funktionen,
welche die Datenbankebene weiter ergänzen. Außerdem sind
Anschlussstellen für eine größere Bandbreite von Datenbankservern und
die Verkettung von Abfragen enthalten, um die Lesbarkeit von
Verbindungsprogrammcode zu verbessern und die Kodierung von SQL zu
erleichtern.

Joomla! kann verschiedene Arten von SQL-Datenbanksystemen verwenden und
funktioniert in einer Vielzahl von Umgebungen mit verschiedenen
Tabellen-Präfixen. Zusätzlich zu diesen Funktionen erzeugt diese
Objektklasse automatisch eine Datenbankverbindung. Außerdem braucht man
neben der Instantiierung des Objekts nur zwei Zeilen Programmcode, um
ein Ergebnis aus der Datenbank in einer großen Bandbreite von Formaten
zu erhalten. Die Verwendung der Joomla! Datenbankschicht gewährleistet
ein Maximum an Kompatibilität und Flexibilität für Erweiterungen.

## Die Abfrage

Joomla!'s Datenbankabfrage hat sich mit der Einführung der neuen
Joomla!-Struktur verändert. Die empfohlene Vorgehensweise, um eine
Datenbankabfrage zu erstellen, ist die "Abfragenverkettung" (obwohl
Zeichenkettenabfragen immer noch unterstützt werden).

Die Abfragenverkettung bezieht sich auf die Verbindung mehrerer
Methoden, eine nach der Anderen, wobei jede Methode ein Objekt ausgibt,
das die nächste Methode unterstützt, was die Lesbarkeit erleichtert und
den Programmcode vereinfacht.

Um eine neue Instanz der Klasse JDatabaseQuery zu erhalten, benützen wir
die JDatebaseDriver getQuery-Methode:

    $db = JFactory::getDbo();

    $query = $db->getQuery(true);

`JDatabaseDriver::getQuery` erhält das optionales Argument `$new`,
welches die Werte wahr oder falsch annehmen kann (standardmäßig ist
falsch vorgegeben).

Um unsere Datenquelle abzufragen rufen wir eine Reihe von JDatabaseQuery
Methoden auf; diese Methoden verkapseln die Abfragesprache der
Datenquelle (in den meisten Fällen SQL), verbergen abfragespezifische
Syntax vor dem Entwickler und erhöhen die Übertragbarkeit seines
Quellkodes.

Einige der häufiger verwendeten Verfahren beinhalten: select, from,
join, where und order. Es gibt auch Methoden wie insert, update und
delete, um Einträge im Datenbestand zu verändern. Durch die Verkettung
dieser und anderer Methodenaufrufe kann man fast beliebig viele Abfragen
auf den Datenbestand erzeugen ohne die Übertragbarkeit des Kodes zu
gefährden.

## Einen Eintrag einfügen

### SQL verwenden

Die Objektklasse JDatabaseQuery bietet eine Vielzahl von Methoden, um
Einfügeabfragen zu erstellen, am verbreitesten sind <a
href="http://api.joomla.org/11.4/Joomla-Platform/Database/JDatabaseQuery.html#insert"
class="external text" target="_blank"
rel="noreferrer noopener">insert</a>, <a
href="http://api.joomla.org/11.4/Joomla-Platform/Database/JDatabaseQuery.html#columns"
class="external text" target="_blank"
rel="noreferrer noopener">columns</a> und <a
href="http://api.joomla.org/11.4/Joomla-Platform/Database/JDatabaseQuery.html#values"
class="external text" target="_blank"
rel="noreferrer noopener">values</a>.

    // Get a db connection.
    $db = JFactory::getDbo();

    // Create a new query object.
    $query = $db->getQuery(true);

    // Insert columns.
    $columns = array('user_id', 'profile_key', 'profile_value', 'ordering');

    // Prepare the insert query.
    $query
        ->insert($db->quoteName('#__user_profiles'))
        ->columns($db->quoteName($columns))
        ->values(':user_id, :profile_key, :profile_value, :ordering');

    // Bind values
    $query
        ->bind(':user_id', 1001, Joomla\Database\ParameterType::INTEGER)
        ->bind(':profile_key', 'custom.message')
        ->bind(':profile_value', 'Inserting a record using insert()')
        ->bind(':ordering', 1, Joomla\Database\ParameterType::INTEGER);

    // Set the query using our newly populated query object and execute it.
    $db->setQuery($query);

    $db->execute();

### Verwendung eines Objekts

Die `JDatabaseDriver` Objektklasse bietet auch einen bequemen Weg, ein
Objekt direkt in der Datenbank zu speichern, was uns erlaubt, einen
Eintrag in einer Tabelle hinzuzufügen, ohne auch nur eine einzige Zeile
SQL zu schreiben.

    // Create and populate an object.
    $profile = new stdClass();
    $profile->user_id = 1001;
    $profile->profile_key='custom.message';
    $profile->profile_value='Inserting a record using insertObject()';
    $profile->ordering=1;

    // Insert the object into the user profile table.
    $result = JFactory::getDbo()->insertObject('#__user_profiles', $profile);

Berücksichtige hier, das wir den Tabellennamen nicht auskommentieren
müssen; die insertObject Methode erledigt das für uns.

Die insertObject Anweisung wird einen Fehler ausgeben, falls es ein
Problem beim Einfügen eines Eintrags in die Datenbanktabelle gibt.

Legt man einen eindeutigen Wert für den Primärschlüssel fest (wie im
obigen Beispiel), wird stark empfohlen, dass man diesen Spaltenwert von
der Tabelle auswählt, bevor man versucht etwas einzufügen.

Wird einfach nur die nächste Zeile in die Tabelle eingefügt (das heißt
die Datenbank erstellt einen Wert für den Primärschlüssel) kann man den
Spaltennamen des Primärschlüssels als dritten Parameter der
insertObject() Anweisung festlegen. Diese Methode wird das Objekt auf
den neu erstellten Primärschlüsselwert aktualisieren.

Zum Beispiel nachstehende aufgeführte Anweisung:

    $result = $dbconnect->insertObject('#__my_table', $object, 'primary_key');

nach der Ausführung wird \$object-\>primary_key mit dem
Primärschlüsselwert der neu eingegebenen Zeile aktualisiert.

**Tipp:** Setze den \$object-\>primary_key vor der Eingabe auf null oder
0.

## Einen Eintrag aktualisieren

### Verwendung von SQL

Die Objektklasse `JDatabaseQuery` stellt auch Verfahren zum Erstellen
von Aktualisierungabfragen zur Verfügung, insbesondere <a
href="http://api.joomla.org/11.4/Joomla-Platform/Database/JDatabaseQuery.html#update"
class="external text" target="_blank"
rel="noreferrer noopener">update</a> und <a
href="http://api.joomla.org/11.4/Joomla-Platform/Database/JDatabaseQuery.html#set"
class="external text" target="_blank" rel="noreferrer noopener">set</a>.
Wir verwenden auch eine andere Anweisung wieder, die wir bereits beim
Erstellen von select-Anweisungen benutzt haben, die where-Anweisung.

    $db = JFactory::getDbo();

    $query = $db->getQuery(true);

    // Fields to update.
    $fields = array(
        $db->quoteName('profile_value') . ' = :profile_value',
        $db->quoteName('ordering') . ' = :ordering'
    );

    // Conditions for which records should be updated.
    $conditions = array(
        $db->quoteName('user_id') . ' = :user_id', 
        $db->quoteName('profile_key') . ' = :profile_key'
    );

    $query->update($db->quoteName('#__user_profiles'))->set($fields)->where($conditions);

    $query
        ->bind(':profile_value', 'Updating custom message for user 1001.')
        ->bind(':ordering', 2, Joomla\Database\ParameterType::INTEGER)
        ->bind(':user_id', 42, Joomla\Database\ParameterType::INTEGER)   
        ->bind(':profile_key', 'custom.message');

    $db->setQuery($query);

    $result = $db->execute();

### Ein Objekt verwenden

Wie das `insertObject`, bietet die JDatabaseDriver Klasse eine bequeme
Methode für das Aktualisieren eines Objekts.

Nachfolgend werden wir unsere angepasste Tabelle mit neuen Werten unter
Verwendung einer bestehenden ID des Primärschlüssels aktualisieren.

    // Create an object for the record we are going to update.
    $object = new stdClass();

    // Must be a valid primary key value.
    $object->id = 1;
    $object->title = 'My Custom Record';
    $object->description = 'A custom record being updated in the database.';

    // Update their details in the users table using id as the primary key.
    $result = JFactory::getDbo()->updateObject('#__custom_table', $object, 'id');

Genau wie insertObject, kümmert sich updateObject um das Auskommentieren
der Tabellennamen.

Die updateObject Anweisung gibt einen Fehler zurück, falls es ein
Problem beim Aktualisieren eines Eintrags in der Datenbanktabelle gibt.

Vor der Aktualisierung muss sichergestellt werden, dass der Eintrag
bereits besteht. Daher sollte eine Eintragsprüfung stattfinden, bevor
die updateObject-Anweisung ausgeführt wird.

## Einen Eintrag löschen

Schließlich gibt es noch eine delete-Anweisung, um Einträge aus der
Datenbank zu entfernen.

    $db = JFactory::getDbo();

    $query = $db->getQuery(true);

    // delete all custom keys for user 1001.
    $conditions = array(
        $db->quoteName('user_id') . ' = :user_id', 
        $db->quoteName('profile_key') . ' = :profile_key'
    );

    $query->delete($db->quoteName('#__user_profiles'));
    $query->where($conditions);

    $query
        ->bind(':user_id', 1001, Joomla\Database\ParameterType::INTEGER)
        ->bind(':profile_key', 'custom.%');

    $db->setQuery($query);

    $result = $db->execute();

### See also

- [Selecting data using
  JDatabase](https://docs.joomla.org/J4.x:Selecting_data_using_JDatabase "Special:MyLanguage/J4.x:Selecting data using JDatabase")
- [Moving Joomla To Prepared
  Statements](https://docs.joomla.org/J4.x:Moving_Joomla_To_Prepared_Statements "Special:MyLanguage/J4.x:Moving Joomla To Prepared Statements")
- [Using the union methods in database
  queries](https://docs.joomla.org/Using_the_union_methods_in_database_queries "Special:MyLanguage/Using the union methods in database queries")
  (Joomla! 3.3+)
- <a
  href="https://api.joomla.org/framework-1/classes/Joomla.Database.DatabaseQuery.html"
  class="external text" target="_blank" rel="noreferrer noopener">Joomla
  Framework API</a>

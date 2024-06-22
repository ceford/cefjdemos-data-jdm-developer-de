<!-- Filename: J4.x:Selecting_data_using_JDatabase / Display title:  Nutzung von JDatabase für Datenabfragen -->

Joomla!  4.x Versionshinweis

Bitte beachten: Online finden sich viele Beispiele, welche die Methoden
für vorbereitete Anweisungen (*prepared statements*), die mit Joomla!
4.x eingeführt wurden, nicht benutzen. Bitte verwende diese alten
Anwendungsprogrammierschnittstellen (APIs) nicht für neue Projekte. Sie
können zu erheblichen Sicherheitsrisiken führen, falls eine
Benutzereingabe nicht genauestens bereinigt (*escaped*) wird.

Dieses Tutorial wurde in zwei unabhängige Teile aufgeteilt:

- Einfügen, aktualisieren und entfernen von Daten der Datenbank.
- Daten aus einer oder mehr Tabellen auswählen und diese in vielfältigen
  verschiedenen Formaten abrufen

Dieser Bereich der Dokumentation richtet das Augenmerk auf das Auswählen
von Daten in einer Datenbanktabelle und deren Abruf in verschiedenen
Formaten. Um den anderen Teil durchzuarbeiten [hier
klicken](https://docs.joomla.org/J4.x:Inserting_Updating_and_Removing_data_using_JDatabase "Special:MyLanguage/J4.x:Inserting Updating and Removing data using JDatabase")

## Einleitung

Joomla! bietet eine hochentwickelte Datenbank mit hohem
Abstraktionsgrad, um Drittentwicklern die Bedienung zu erleichtern. Neue
Versionen der Joomla! Plattform API liefern zusätzliche Funktionen,
welche die Datenbankebene weiter ergänzenSie enthält Funktionen wie
Anschlussstellen für eine größere Bandbreite von Datenbankservern und
die Abfragenverkettung, um die Lesbarkeit von Verbindungsprogrammcode zu
verbessern und die Kodierung von SQL zu erleichtern.

Joomla! kann verschiedene Arten von SQL-Datenbanksystemen verwenden und
funktioniert in einer Vielzahl von Umgebungen mit verschiedenen
Tabellenbezeichnungen. Zusätzlich zu diesen Funktionen erzeugt diese
Objektklasse automatisch eine Datenbankverbindung. Außerdem braucht man
neben der Instantiierung des Objekts nur zwei Zeilen Programmcode, um
ein Ergebnis aus der Datenbank in einer großen Bandbreite von Formaten
zu erhalten. Die Verwendung der Joomla! Datenbankschicht gewährleistet
ein Maximum an Kompatibilität und Flexibilität für deine Erweiterung.

## Die Abfrage

Da Joomla in Joomla 1.6 die Unterstützung für eine Vielzahl von
Datenbanktypen eingeführt hat, wird empfohlen, Datenbankabfragen durch
"Abfragenverkettung" zu erstellen (obwohl Zeichenkettenabfragen auch
weiterhin unterstützt werden).

Die Abfragenverkettung bezieht sich auf ein Verfahren eine Vielzahl von
Methoden zu verbinden, eine nach der Anderen, wobei jede Methode ein
Objekt ausgibt, das die nächste Methode unterstützt, was die Lesbarkeit
erleichtert und den Programmcode vereinfacht.

Um eine neue Instanz der Klasse \Joomla\Database\DatabaseQuery zu
erhalten, benützen wir die Methode *\Joomla\Database\DatabaseDriver
getQuery*:

    use Joomla\CMS\Factory;

    $db = Factory::getDbo();

    $query = $db->getQuery(true);

\Joomla\Database\DatabaseDriver::getQuery erhält ein optionales
Argument, \$new, welches die Werte wahr oder falsch annehmen kann
(standardmäßig ist falsch vorgegeben).

Um unsere Datenquelle abzufragen rufen wir eine Reihe der Methoden
\Joomla\Database\DatabaseQuery auf. Diese Methoden verkapseln die
Abfragesprache der Datenquelle (in den meisten Fällen SQL), verbergen
abfragespezifische Syntax vor dem Entwickler und erhöhen die
Übertragbarkeit des Quellkodes des Entwicklers.

Einige der häufiger verwendeten Verfahren beinhalten: select, from,
join, where und order. Es gibt auch Methoden wie insert, update und
delete um Einträge im Datenbestand zu verändern. Durch die Verkettung
dieser und anderer Methodenaufrufe kann man fast beliebig viele Abfragen
auf den Datenbestand erzeugen ohne die Übertragbarkeit des Kodes zu
gefährden..

## Einträge aus einer einzelnen Tabelle auswählen

Darunter befindet sich ein Beispiel, wie eine Datenbankabfrage erstellt
wird, welche die Klasse tt\>\Joomla\Database\DatabaseQuery verwendet.
Durch den Gebrauch der Methoden *select*, *from*, *where* und *order*
können wir Abfragen erstellen, die flexibel, einfach lesbar und
übertragbar sind:

    use Joomla\CMS\Factory;

    // Get a db connection.
    $db = Factory::getDbo();

    // Create a new query object.
    $query = $db->getQuery(true);

    // Select all records from the user profile table where key begins with "custom.".
    // Order it by the ordering field.
    $query->select($db->quoteName(['user_id', 'profile_key', 'profile_value', 'ordering']));
    $query->from($db->quoteName('#__user_profiles'));
    $query->where($db->quoteName('profile_key') . ' LIKE :profile_key');
    $query->order($db->quoteName('ordering') . ' ASC');

    // bind value for prepared statements
    $query->bind(':profile_key', 'custom.%');

    // Reset the query using our newly populated query object.
    $db->setQuery($query);

    // Load the results as a list of stdClass objects (see later for more options on retrieving data).
    $results = $db->loadObjectList();

Die Abfrage kann auch verkettet und damit weiter vereinfacht werden:

    $query
        ->select($db->quoteName(['user_id', 'profile_key', 'profile_value', 'ordering']))
        ->from($db->quoteName('#__user_profiles'))
        ->where($db->quoteName('profile_key') . ' LIKE :profile_key')
        ->order($db->quoteName('ordering') . ' ASC')
        ->bind(':profile_key', 'custom.%');

Das Verketten kann sich als praktischer erweisen, wenn die Abfragen
länger und komplexer werden.

Das Gruppieren kann auch einfach erreicht werden. Die folgende Abfrage
würde die Anzahl der Beiträge in jeder Kategorie zählen.

    $query
        ->select( ['catid', 'COUNT(*)'] )
        ->from($db->quoteName('#__content'))
        ->group($db->quoteName('catid'));

Mit "setLimit" wird die Abfrage begrenzt. In der folgenden Abfrage zum
Beispiel würden bis zu 10 Einträge ausgegeben.

    $query
        ->select($db->quoteName(['user_id', 'profile_key', 'profile_value', 'ordering']))
        ->from($db->quoteName('#__user_profiles'))
        ->setLimit('10');

## Einträge aus vielen Tabellen auswählen

Durch Nutzung der Methode \Joomla\Database\DatabaseQuery's <a
href="https://api.joomla.org/framework-1/classes/Joomla.Database.DatabaseQuery.html#join"
class="external text" target="_blank" rel="noreferrer noopener">join</a>
können wir Einträge in mehreren verknüpften Tabellen auswählen. Die
generische Methode "join" nimmt zwei Argumente an: den join-Typ (inner,
outer, left, right) und die join-Bedingung. Im folgenden Beispiel wird
erkennbar, dass wir all die Schlagwörter benutzen, die wir
gewöhnlicherweise auch für eine native SQL-Abfrage verwenden. Diese
beinhalten das Schlagwort AS, um Tabellen ein Alias zuzuordnen, und das
Schlüsselwort ON, um Beziehungen zwischen den Tabellen herzustellen. Es
sollte beachtet werden, das der Tabellenalias in allen Methoden
verwendet wird, die auf Tabellenspalten Bezug nehmen (zum Beispiel
select, where, order).

    use Joomla\CMS\Factory;

    // Get a db connection.
    $db = Factory::getDbo();

    // Create a new query object.
    $query = $db->getQuery(true);

    // Select all articles for users who have a username which starts with 'a'.
    // Order it by the created date.
    // Note by putting 'a' as a second parameter will generate `#__content` AS `a`
    $query
        ->select(['a.*', 'b.username', 'b.name'])
        ->from($db->quoteName('#__content', 'a'))
        ->join('INNER', $db->quoteName('#__users', 'b') . ' ON (' . $db->quoteName('a.created_by') . ' = ' . $db->quoteName('b.id') . ')')
        ->where($db->quoteName('b.username') . ' LIKE :username')
        ->order($db->quoteName('a.created') . ' DESC')
        ->bind(':username', 'a%');

    // Reset the query using our newly populated query object.
    $db->setQuery($query);

    // Load the results as a list of stdClass objects (see later for more options on retrieving data).
    $results = $db->loadObjectList();

Die oben beschriebene Methode *join* versetzt uns in die Lage, sowohl
die Inhalts- als auch die Benutzertabelle abzufragen, was Artikel mit
deren Verfasserdetails auflistet.

- <a
  href="https://api.joomla.org/framework-1/classes/Joomla.Database.DatabaseQuery.html#method_innerJoin"
  class="external text" target="_blank"
  rel="noreferrer noopener">innerJoin()</a>
- <a
  href="https://api.joomla.org/framework-1/classes/Joomla.Database.DatabaseQuery.html#method_leftJoin"
  class="external text" target="_blank"
  rel="noreferrer noopener">leftJoin()</a>
- <a
  href="https://api.joomla.org/framework-1/classes/Joomla.Database.DatabaseQuery.html#method_rightJoin"
  class="external text" target="_blank"
  rel="noreferrer noopener">rightJoin()</a>
- <a
  href="https://api.joomla.org/framework-1/classes/Joomla.Database.DatabaseQuery.html#method_outerJoin"
  class="external text" target="_blank"
  rel="noreferrer noopener">outerJoin()</a>

Wir können die Methode *join* mehrfach verwenden, um Abfragen über mehr
als zwei Tabellen zu erstellen:

    $query
        ->select(array('a.*', 'b.username', 'b.name', 'c.*', 'd.*'))
        ->from($db->quoteName('#__content', 'a'))
        ->join('INNER', $db->quoteName('#__users', 'b') . ' ON (' . $db->quoteName('a.created_by') . ' = ' . $db->quoteName('b.id') . ')')
        ->join('LEFT', $db->quoteName('#__user_profiles', 'c') . ' ON (' . $db->quoteName('b.id') . ' = ' . $db->quoteName('c.user_id') . ')')
        ->join('RIGHT', $db->quoteName('#__categories', 'd') . ' ON (' . $db->quoteName('a.catid') . ' = ' . $db->quoteName('d.id') . ')')
        ->where($db->quoteName('b.username') . ' LIKE :username')
        ->order($db->quoteName('a.created') . ' DESC')
        ->bind(':username', 'a%');

Die Verkettung macht den Quellkode für diese längeren Abfragen viel
besser lesbar.

In manchen Fällen muss auch die AS-Klausel benutzt werden, wenn Elemente
ausgewählt werden, um Konflikte mit Spaltenüberschriften zu vermeiden.
In diesem Fall können mehrere Auswahlanweisungen in Verbindung mit dem
Gebrauch des zweiten Parameters in Form von \$db-\>quoteName verkettet
werden.

    $query
        ->select('a.*')
        ->select($db->quoteName('b.username', 'username'))
        ->select($db->quoteName('b.name', 'name'))
        ->from($db->quoteName('#__content', 'a'))
        ->join('INNER', $db->quoteName('#__users', 'b'), $db->quoteName('a.created_by') . ' = ' . $db->quoteName('b.id'))
        ->where($db->quoteName('b.username') . ' LIKE :username')
        ->order($db->quoteName('a.created') . ' DESC')
        ->bind(':username', 'a%');

Es ist auch möglich, ein zweites Datenfeld als weiteren Parameter für
die gewählte Anweisung zu benutzen um die Werte des AS Abschnitts zu
bestücken. Bitte daran denken, Nullen in das zweite Datenfeld
einzubeziehen um mit den Spalten im ersten Datenfeld übereinzustimmen,
für das man den AS Abschnitt nicht benutzen will.

    $query
        ->select(['a.*'])
        ->select($db->quoteName(array('b.username', 'b.name'), ['username', 'name']))
        ->from($db->quoteName('#__content', 'a'))
        ->join('INNER', $db->quoteName('#__users', 'b') . ' ON (' . $db->quoteName('a.created_by') . ' = ' . $db->quoteName('b.id') . ')')
        ->where($db->quoteName('b.username') . ' LIKE :username')
        ->order($db->quoteName('a.created') . ' DESC')
        ->bind(':username', 'a%');

## Verwendung von vorbereiteten Anweisungen

Mit Joomla! 4.0 haben wir [alle Anfragen auf vorbereitete Aussagen
umgestellt](https://docs.joomla.org/J4.x:Moving_Joomla_To_Prepared_Statements "Special:MyLanguage/J4.x:Moving Joomla To Prepared Statements").
Zur einfacheren Verwendung von Prepared Statements haben wir einige
Hilfsfunktionen eingeführt und erlauben die Verwendung von Arrays in
mehreren Funktionsaufrufen. Einfache Abfrage mit vorbereiteten
Anweisungen.

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName(array('id', 'password')))
        ->from($this->db->quoteName('#__users'))
        ->where($this->db->quoteName('username') . ' = :username')
        ->bind(':username', $credentials['username']);

Wir fügen die **\$credentials\['username'\]** nicht direkt der Abfrage
hinzu, sondern fügen stattdessen den Platzhalter **:username** in die
Abfrage ein und **binden** die Variable an die Abfrage. Wenn wir eine
Variable an eine Abfrage binden, brauchen wir sie weder zu "escapen"
noch in Anführungszeichen zu setzen. Es sollte beachtet werden, dass das
Binden einer Variablen immer eine Referenz darstellt. Ein netter
Nebeneffekt davon ist, dass die Abfrage in einer Schleife manipuliert
werden kann.

    $listOfUsernames = [ 'admin', 'user1' ];

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName(array('id', 'password')))
        ->from($this->db->quoteName('#__users'))
        ->where($this->db->quoteName('username') . ' = :username')
        ->bind(':username', $username);

    foreach($listOfUsernames as $name)
    {
      $username = $name;
      $this->db->setQuery($query);
      $user = $this->db->loadObject();
      print_r($user);
    }

In der Schleife setzen wir die zuvor gebundene Variable **\$username**
mit der Variablen **\$name** aus der Schleife. Dann müssen wir die
Abfrage erneut setzen (weil Joomla den Datenbanktreiber nach der
Abfrageausführung zurückgesetzt hat, was nur für load\*-Funktionen
gilt). Das Ergebnis davon werden mehrere Abfragen mit unterschiedlichen
Benutzernamenswerten sein. Wir können Arrays verwenden, um mehrere
Variablen auf einmal hinzuzufügen.

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName(array('id', 'password')))
        ->from($this->db->quoteName('#__users'))
        ->where($this->db->quoteName('username') . ' = :username')
        ->where($this->db->quoteName('id') . ' = :id')
        ->bind([':username', ':id'], [$credentials['username'], 42], [Joomla\Database\ParameterType::STRING, Joomla\Database\ParameterType::INTEGER]);

Wir fügen **username** und **id'** als Bindungsparameter hinzu und
stellen den korrekten ParameterType für jede Variable ein. Es ist auch
möglich, eine Variable für alle Bindungswerte und Parameter-Typen zu
verwenden.

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName(array('id', 'password')))
        ->from($this->db->quoteName('#__users'))
        ->where($this->db->quoteName('username') . ' = :username')
        ->where($this->db->quoteName('password') . ' = :password')
        ->bind([':username', ':password], $credentials['username']);

Die Parameter :username und :password werden auf den gleichen Wert und
den Standard-ParameterType gesetzt. Die Funktionen **whereIn()** und
**whereNotIn()** verwenden immer vorbereitete Anweisungen. Intern
verwenden diese Funktionen die Funktion **bindArray**. Sie kann
verwendet werden, um ein Array von Variablen zu binden, ohne den
Platzhalter anzugeben.

    $userids = [1,2,3,4];

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName(array('id', 'password')))
        ->from($this->db->quoteName('#__users'));

    $parameterNames = $query->bindArray($userids);

    $query->where($this->db->quoteName('id') . ' IN (' . implode(',', $parameterNames) . ')');

The **bindArray** function returns an array of placeholders. The index
is unique for the whole query.

    $placeholders = [
      ':preparedArray1',
      ':preparedArray2',
      ':preparedArray3',
      ':preparedArray4'
    ];

## Abfrageergebnisse

Das Datenbankmodell enthält viele Verfahrensweisen, um die Ergebnisse
einer Abfrage darzustellen.

### Einzelwertergebnis

#### loadResult()

Verwende **loadResult()** wenn du einen Einzelwert aus deiner
Datenbankabfrage erwartest.

|     |                  |                          |              |
|-----|------------------|--------------------------|--------------|
| id  | Name             | E-Mail                   | Benutzername |
| 1   | John Smith       | johnsmith@domain.example | johnsmith    |
| 2   | Magda Hellman    | magda_h@domain.example   | magdah       |
| 3   | Yvonne de Gaulle | ydg@domain.example       | ydegaulle    |

Dies ist oft das Ergebnis einer 'count' Abfrage, um eine Vielzahl von
Datensätzen zu erhalten.

    use Joomla\CMS\Factory;
    $db = Factory::getDbo();
    $query = $db->getQuery(true);
    $query->select('COUNT(*)');
    $query->from($db->quoteName('#__my_table'));
    $query->where($db->quoteName('name')." = :value");
    $query->bind('value', $value)

    // Reset the query using our newly populated query object.
    $db->setQuery($query);
    $count = $db->loadResult();

oder wo du nur einmal nach einem einzelnen Datenfeld aus einer einzigen
Spalte einer Tabelle suchst (oder möglicherweise wird ein einzelnes
Datenfeld aus der ersten Zeile ausgegeben).

    use Joomla\CMS\Factory;
    $db = Factory::getDbo();
    $query = $db->getQuery(true);
    $query->select('field_name');
    $query->from($db->quoteName('#__my_table'));
    $query->where($db->quoteName('some_name')." = :value");
    $query->bind(':value', $some_value);

    $db->setQuery($query);
    $result = $db->loadResult();

### Einzelne Zeilenergebnisse

Jede dieser Funktionen gibt einen einzelnen Datensatz aus der Datenbank
zurück, selbst wenn es möglicherweise mehrere Datensätze gibt, welche
die gesetzten Kriterien erfüllen. Um mehr Datensätze zu erhalten, muss
die Funktion erneut aufgerufen werden.

|     |                  |                          |              |
|-----|------------------|--------------------------|--------------|
| id  | Name             | E-Mail                   | Benutzername |
| 1   | John Smith       | johnsmith@domain.example | johnsmith    |
| 2   | Magda Hellman    | magda_h@domain.example   | magdah       |
| 3   | Yvonne de Gaulle | ydg@domain.example       | ydegaulle    |

#### loadRow()

`loadRow()` liefert einen indizierten Array eines einzelnen Datensatzes
in der Tabelle:

    . . .
    $db->setQuery($query);
    $row = $db->loadRow();
    print_r($row);

führt zur Ausgabe von:

    Array ( [0] => 1, [1] => John Smith, [2] => johnsmith@domain.example, [3] => johnsmith ) 

Man kann auf einzelne Werte zugreifen durch den Gebrauch von:

    $row['index'] // e.g. $row['2']

Anmerkungen:

1.  Die Array-Indizes sind numerisch und fangen bei Null an.
2.  Während man die Prozedur erneut aufrufen kann, um weitere Zeilen zu
    erhalten, wäre eine der Funktionen, die mehrere Zeilen ausgeben,
    eventuell praktischer.

#### loadAssoc()

`loadAssoc()` liefert einen assoziierten Array für einem einzelnen
Eintrag der Tabelle:

    . . .
    $db->setQuery($query);
    $row = $db->loadAssoc();
    print_r($row);

wird ergeben:

    Array ( [id] => 1, [Name] => John Smith, [E-mail] => johnsmith@domain.example, [Benutzername] => johnsmith )

Man kann auf individuelle Werte zugreifen durch den Gebrauch von:

    $row['name'] // e.g. $row['email']

Anmerkung:

1.  Während man den Aufruf wiederholen kann um weitere Zeilen zu
    erhalten, wäre eine derjenigen Funktionen, die mehrere Zeilen
    ausgeben, eventuell praktischer.

#### loadObject()

loadObject gibt ein PHP-Objekt von einem einzelnen Eintrag in der
Tabelle aus:

    . . .
    $db->setQuery($query);
    $result = $db->loadObject();
    print_r($result);

wird ausgeben:

    stdClass Object ( [id] => 1, [Name] => John Smith, [E-mail] => johnsmith@domain.example, [Benutzername] => johnsmith )

Man kann auf einzelne Werte zugreifen durch den Gebrauch von:

    $result->index // e.g. $result->email

Anmerkung:

1.  Während man den Aufruf wiederholen kann um weitere Zeilen zu
    erhalten, wäre eine derjenigen Funktionen, die mehrere Zeilen
    ausgeben, eventuell praktischer.

### Einzelne Zeilenergebnisse

Jede dieser Resulatsfunktionen wird eine einzelne Spalte aus der
Datenbank ausgeben.

|     |                  |                          |              |
|-----|------------------|--------------------------|--------------|
| id  | Name             | E-Mail                   | Benutzername |
| 1   | John Smith       | johnsmith@domain.example | johnsmith    |
| 2   | Magda Hellman    | magda_h@domain.example   | magdah       |
| 3   | Yvonne de Gaulle | ydg@domain.example       | ydegaulle    |

#### loadColumn()

`loadColumn()` liefert eine indizierte Datenreihe einer einzelnen Spalte
in der Tabelle:

    $query->select('name'));
          ->from . . .";
    . . .
    $db->setQuery($query);
    $column= $db->loadColumn();
    print_r($column);

führt zur Ausgabe von:

    Array ( [0] => John Smith, [1] => Magda Hellman, [2] => Yvonne de Gaulle )

Man kann auf einzelne Werte zugreifen durch den Gebrauch von:

    $column['index'] // e.g. $column['2']

Anmerkungen:

1.  Die Array-Indizes sind numerisch und fangen bei Null an.
2.  `loadColumn()` ist ein Equivalent von loadColumn(0).

#### loadColumn(\$index)

loadColumn(\$index) liefert eine indizierte Datenreihe einer einzelnen
Spalte in der Tabelle:

    $query->select(array('name', 'email', 'username'));
          ->from . . .";
    . . .
    $db->setQuery($query);
    $column= $db->loadColumn(1);
    print_r($column);

führt zu:

    Array ( [0] => johnsmith@domain.example, [1] => magda_h@domain.example, [2] => ydg@domain.example )

Man kann auf einzelne Werte zugreifen durch den Gebrauch von:

    $column['index'] // e.g. $column['2']

loadColumn(\$index) erlaubt es dir, schrittweisen Zugriff auf eine Folge
von Spalten in den Ergebnissen zu erhalten.

    . . .
    $db->setQuery($query);
    for ( $i = 0; $i <= 2; $i++ ) {
      $column = $db->loadColumn($i);
      print_r($column);
    }

führt zur Ausgabe von:

    Array ( [0] => John Smith, [1] => Magda Hellman, [2] => Yvonne de Gaulle ),
    Array ( [0] => johnsmith@domain.example, [1] => magda_h@domain.example, [2] => ydg@domain.example ),
    Array ( [0] => johnsmith, [1] => magdah, [2] => ydegaulle )

Bemerkung:

1.  Die Datenfeldindizes sind numerisch und fangen bei Null an.

### Ergebnisse aus mehreren Zeilen

Jede dieser Funktionen gibt mehrere Datensätze aus der Datenbank aus.

|     |                  |                          |              |
|-----|------------------|--------------------------|--------------|
| id  | Name             | E-Mail                   | Benutzername |
| 1   | John Smith       | johnsmith@domain.example | johnsmith    |
| 2   | Magda Hellman    | magda_h@domain.example   | magdah       |
| 3   | Yvonne de Gaulle | ydg@domain.example       | ydegaulle    |

#### loadRowList()

`loadRowList()` gibt einen indizierten Array von bereits indizierten
Arrays der Tabelleneinträge aus, die durch die Abfrage zurückgeliefert
wurden:

    . . .
    $db->setQuery($query);
    $row = $db->loadRowList();
    print_r($row);

wird ergeben (der Übersichtlichkeit wegen mit zusätzlichen
Zeilenumbrüchen):

    Array ( 
    [0] => Array ( [0] => 1, [1] => John Smith, [2] => johnsmith@domain.example, [3] => johnsmith ), 
    [1] => Array ( [0] => 2, [1] => Magda Hellman, [2] => magda_h@domain.example, [3] => magdah ), 
    [2] => Array ( [0] => 3, [1] => Yvonne de Gaulle, [2] => ydg@domain.example, [3] => ydegaulle ) 
    )

Man kann auf einzelne Zeilen zugreifen durch den Gebrauch von:

    $row['index'] // e.g. $row['2']

und man kann auf einzelne Werte zugreifen durch die Nutzung von:

    $row['index']['index'] // e.g. $row['2']['3']

Bemerkung:

1.  Die Array-Indizes sind numerisch und fangen bei Null an.

#### loadAssocList()

`loadAssocList()` gibt einen indizierten Array von assoziierten Arrays
der Tabelleneinträge aus, die sich aus der Abfrage ergaben:

    . . .
    $db->setQuery($query);
    $row = $db->loadAssocList();
    print_r($row);

wird ergeben (der Übersichtlichkeit wegen mit zusätzlichen
Zeilenumbrüchen):

    Array ( 
    [0] => Array ( [id] => 1, [name] => John Smith, [email] => johnsmith@domain.example, [username] => johnsmith ), 
    [1] => Array ( [id] => 2, [name] => Magda Hellman, [email] => magda_h@domain.example, [username] => magdah ), 
    [2] => Array ( [id] => 3, [name] => Yvonne de Gaulle, [email] => ydg@domain.example, [username] => ydegaulle ) 
    ) 

Man kann auf einzelne Zeilen zugreifen indem man Folgendes macht:

    $row['index'] // e.g. $row['2']

und man kann auf einzelne Werte zugreifen durch die Nutzung von:

    $row['index']['column_name'] // e.g. $row['2']['email']

#### loadAssocList(\$Datenbankschlüssel)

`loadAssocList('Datenbankschlüssel')` gibt einen assoziierten Array -
indiziert durch den Datenbankschlüssel - zurück, der aus assoziierten
Arrays der Tabelleneinträge besteht, die sich aus der Abfrage ergaben:

    . . .
    $db->setQuery($query);
    $row = $db->loadAssocList('username');
    print_r($row);

wird ergeben (der Übersichtlichkeit wegen mit zusätzlichen
Zeilenumbrüchen):

    Array ( 
    [johnsmith] => Array ( [id] => 1, [name] => John Smith, [email] => johnsmith@domain.example, [username] => johnsmith ), 
    [magdah] => Array ( [id] => 2, [name] => Magda Hellman, [email] => magda_h@domain.example, [username] => magdah ), 
    [ydegaulle] => Array ( [id] => 3, [name] => Yvonne de Gaulle, [email] => ydg@domain.example, [username] => ydegaulle ) 
    )

Man kann auf einzelne Zeilen zugreifen durch den Gebrauch von:

    $row['key_value'] // e.g. $row['johnsmith']

und man kann auf einzelne Werte zugreifen durch die Nutzung von:

    $row['key_value']['column_name'] // e.g. $row['johnsmith']['email']

Beachte: Der Datenbankschlüssel muss ein gültiger Spaltenname aus der
Tabelle sein; es braucht kein Index oder Primärschlüssel zu sein. Aber
falls er keinen eindeutigen Wert hat, kann man eventuell keine
Ergebnisse zuverlässig abrufen.

#### loadAssocList(\$Datenbankschlüssel, \$Spalte)

Durch `loadAssocList('Datenbankschlüssel', 'Spalte')` erhält man einen
assoziierten Array, indiziert durch den Datenbankschlüssel, und die
Werte aus der Spalte mit den jeweiligen Namen, die sich aus der Abfrage
ergaben:

    . . .
    $db->setQuery($query);
    $row = $db->loadAssocList('id', 'username');
    print_r($row);

wird ergeben (der Übersichtlichkeit wegen mit zusätzlichen
Zeilenumbrüchen):

    Array ( 
    [1] => John Smith, 
    [2] => Magda Hellman, 
    [3] => Yvonne de Gaulle,
    )

Beachte: Der Datenbankschlüssel muss ein gültiger Spaltenname aus der
Tabelle sein; es braucht kein Index oder Primärschlüssel zu sein. Aber
falls er keinen eindeutigen Wert hat, kann man eventuell keine
Ergebnisse zuverlässig abrufen.

#### loadObjectList()

`loadObjectList()` gibt einen indizierten Array von PHP-Objekten aus,
die von den Tabelleneinträgen stammen, die durch die Abfrage ausgegeben
wurden:

    . . .
    $db->setQuery($query);
    $row = $db->loadObjectList();
    print_r($row);

wird ergeben (der Übersichtlichkeit wegen mit zusätzlichen
Zeilenumbrüchen):

    Array ( 
    [0] => stdClass Object ( [id] => 1, [name] => John Smith, 
        [email] => johnsmith@domain.example, [username] => johnsmith ), 
    [1] => stdClass Object ( [id] => 2, [name] => Magda Hellman, 
        [email] => magda_h@domain.example, [username] => magdah ), 
    [2] => stdClass Object ( [id] => 3, [name] => Yvonne de Gaulle, 
        [email] => ydg@domain.example, [username] => ydegaulle ) 
    )

Man kann auf einzelne Zeilen zugreifen durch den Gebrauch von:

    $row['index'] // e.g. $row['2']

und man kann auf einzelne Werte zugreifen durch die Nutzung von:

    $row['index']->name // e.g. $row['2']->email

#### loadObjectList(\$Datenbankschlüssel)

`loadObjectList('Datenbankschlüssel')` gibt einen assoziierten Array -
indiziert durch den Datenbankschlüssel - von Objekten der
Tabelleneinträge zurück, die sich aus der Abfrage ergaben:

    . . .
    $db->setQuery($query);
    $row = $db->loadObjectList('username');
    print_r($row);

wird ergeben (der Übersichtlichkeit wegen mit zusätzlichen
Zeilenumbrüchen):

    Array ( 
    [johnsmith] => stdClass Object ( [id] => 1, [name] => John Smith, 
        [email] => johnsmith@domain.example, [username] => johnsmith ), 
    [magdah] => stdClass Object ( [id] => 2, [name] => Magda Hellman, 
        [email] => magda_h@domain.example, [username] => magdah ), 
    [ydegaulle] => stdClass Object ( [id] => 3, [name] => Yvonne de Gaulle, 
        [email] => ydg@domain.example, [username] => ydegaulle ) 
    )

Man kann auf einzelne Zeilen zugreifen durch den Gebrauch von:

    $row['key_value'] // e.g. $row['johnsmith']

und man kann auf einzelne Werte zugreifen durch die Nutzung von:

    $row['key_value']->column_name // e.g. $row['johnsmith']->email

Beachte: Der Datenbankschlüssel muss ein gültiger Spaltenname aus der
Tabelle sein; es braucht kein Index oder Primärschlüssel zu sein. Aber
falls er keinen eindeutigen Wert hat, kann man eventuell keine
Ergebnisse zuverlässig abrufen.

### Verschiedene ergebnisorientierte Methoden

#### getNumRows()

`getNumRows()` gibt die Anzahl der Ergebniszeilen aus, die durch die
letzte SELECT- oder SHOW-Abfrage gefunden wurden und darauf warten
gelesen zu werden. Um ein Ergebnis durch getNumRows() zu erhalten, muss
man es **nach** der Abfrage und **bevor** irgendein Ergebnis erhalten
wurde, ausführen. Um die Anzahl der Zeilen zu erhalten, die von einer
INSERT-, UPDATE-, REPLACE- oder DELETE-Abfrage betroffen sind, benutze
getAffectedRows().

    . . .
    $db->setQuery($query);
    $db->execute();
    $num_rows = $db->getNumRows();
    print_r($num_rows);
    $result = $db->loadRowList();

wird ergeben

    3

Anmerkung: getNumRows() ist nur zulässig für Anweisungen wie SELECT oder
SHOW, die eine tatsächliche Ergebnisgruppe zurückgeben.

    Warning: mysql_num_rows(): 80 is not a valid MySQL result resource 
    in libraries\joomla\database\database\mysql.php on line 344

### Siehe auch

- [Einfügen, aktualisieren und löschen von Daten unter Verwendung von
  JDatabase](https://docs.joomla.org/J4.x:Inserting,_Updating_and_Removing_data_using_JDatabase "Special:MyLanguage/J4.x:Inserting, Updating and Removing data using JDatabase")
- [Umstellung in Joomla auf Prepared
  Statements](https://docs.joomla.org/J4.x:Moving_Joomla_To_Prepared_Statements "Special:MyLanguage/J4.x:Moving Joomla To Prepared Statements")
- [Die Verwendung von *union*-Methoden in
  Datenbankabfragen](https://docs.joomla.org/Using_the_union_methods_in_database_queries "Special:MyLanguage/Using the union methods in database queries")
  (Joomla! 3.3+)
- <a href="https://php.net/manual/en/pdo.prepared-statements.php"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">The PHP Manual on Prepared
  statements</a>
- <a
  href="https://api.joomla.org/framework-1/classes/Joomla.Database.DatabaseQuery.html"
  class="external text" target="_blank" rel="noreferrer noopener">Joomla
  Framework API</a>

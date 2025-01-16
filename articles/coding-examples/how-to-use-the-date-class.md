<!-- Filename: How_to_use_JDate / Display title: So verwenden Sie die Date-Klasse -->

## Einführung
Die Joomla-Date-Klasse ist eine Hilfsklasse, die von der PHP-DateTime-Klasse erweitert wurde und es Entwicklern ermöglicht, die Datumformatierung effizienter zu handhaben. Die Klasse erlaubt es Entwicklern, Daten für leserliche Zeichenfolgen, MySQL-Interaktion, UNIX-Zeitstempelberechnung zu formatieren und bietet auch Hilfsmethoden für die Arbeit in verschiedenen Zeitzonen.

## Erstellen einer Datumsinstanz

Alle Datumsunterstützungs-Methoden erfordern eine Instanz der Date-Klasse. Zunächst müssen Sie eine erstellen. Ein Date-Objekt kann auf zwei Arten erstellt werden. Eine Möglichkeit ist die typische native Methode, einfach eine neue Instanz zu erstellen:

```php
use Joomla\CMS\Date\Date;

$date = new Date(); // Creates a new Date object equal to the current time.
```

Sie können auch eine Instanz mit der statischen Methode erstellen, die in Date definiert ist:

```php
use Joomla\CMS\Date\Date;

$date = Date::getInstance(); // Alias of 'new Date();'
```

Es gibt keinen Unterschied zwischen diesen Methoden, da Date::getInstance einfach eine neue Instanz von Date genau wie die erste gezeigte Methode erstellt.

Alternativ können Sie auch das aktuelle Datum (als Date-Objekt) vom Application-Objekt abrufen, indem Sie verwenden:```php
use Joomla\CMS\Factory;

$date = Factory::getDate();
```

## Argumente

Der Date-Konstruktor (und die statische Methode getInstance) akzeptiert zwei optionale Parameter: Einen Datumsstring zum Formatieren und eine Zeitzone. Wenn kein Datumsstring übergeben wird, erstellt das ein Date-Objekt mit dem aktuellen Datum und der aktuellen Uhrzeit. Wenn keine Zeitzone übergeben wird, verwendet das Date-Objekt die standardmäßig eingestellte Zeitzone.

Das erste Argument, falls verwendet, sollte ein String sein, der mit PHPs nativen DateTime-Konstruktor geparst werden kann. Zum Beispiel:```php
use Joomla\CMS\Date\Date;

$currentTime = new Date('now'); // Current date and time
$tomorrowTime = new Date('now +1 day'); // Current date and time, + 1 day.
$plus1MonthTime = new Date('now +1 month'); // Current date and time, + 1 month.
$plus1YearTime = new Date('now +1 year'); // Current date and time, + 1 year.
$plus1YearAnd1MonthTime = new Date('now +1 year +1 month'); // Current date and time, + 1 year and 1 month.
$plusTimeToTime = new Date('now +1 hour +30 minutes +3 seconds'); // Current date and time, + 1 hour, 30 minutes and 3 seconds
$plusTimeToTime = new Date('now -1 hour +30 minutes +3 seconds'); // Current date and time, + 1 hour, 30 minutes and 3 seconds
$combinedTimeToTime = new Date('now -1 hour -30 minutes 23 seconds'); // Current date and time, - 1 hour, +30 minutes and +23 seconds

$date = new Date('2012-12-1 15:20:00'); // 3:20 PM, December 1st, 2012
```

Ein Unix-Zeitstempel (in Sekunden) kann auch als erstes Argument übergeben werden, in welchem Fall er intern in ein Datum umgewandelt wird. Wenn eine Zeitzone als zweites Argument an den Konstruktor übergeben wurde, wird er in diese Zeitzone umgewandelt.

## Ausgeben von Daten

Ein Hinweis zur Vorsicht beim Ausgeben von Date-Objekten in einem Benutzerkontext: Geben Sie diese nicht einfach auf dem Bildschirm aus. Die toString()-Methode des Date-Objekts ruft einfach die format()-Methode des Elternteils auf, ohne Berücksichtigung von Zeitzonen oder lokalisierten Datumsformaten. Dies wird nicht zu einer guten Benutzererfahrung führen und zu Inkonsistenzen zwischen der Formatierung in Ihrer Erweiterung und anderswo in Joomla führen. Stattdessen sollten Sie immer Daten mit den unten gezeigten Methoden ausgeben.

### Gängige Datumsformate

Eine Reihe von Datumsformaten ist in Joomla als Teil der grundlegenden Sprachpakete vordefiniert. Dies ist vorteilhaft, da es bedeutet, dass Datumsformate leicht internationalisiert werden können. Ein Beispiel der verfügbaren Formatzeichenfolgen ist unten aus dem en-GB-Sprachpaket aufgeführt. Es wird dringend empfohlen, diese Formatzeichenfolgen zu verwenden, wenn Sie Daten ausgeben, damit Ihre Daten automatisch entsprechend der Regionseinstellung eines Benutzers neu formatiert werden. Sie können auf die gleiche Weise wie jede andere Sprachzeichenfolge abgerufen werden (siehe unten für Beispiele).

```ini
DATE_FORMAT_LC="l, d F Y"
DATE_FORMAT_LC1="l, d F Y"
DATE_FORMAT_LC2="l, d F Y H:i"
DATE_FORMAT_LC3="d F Y"
DATE_FORMAT_LC4="Y-m-d"
DATE_FORMAT_LC5="Y-m-d H:i"
DATE_FORMAT_LC6="Y-m-d H:i:s"
DATE_FORMAT_JS1="y-m-d"
DATE_FORMAT_CALENDAR_DATE="%Y-%m-%d"
DATE_FORMAT_CALENDAR_DATETIME="%Y-%m-%d %H:%M:%S"
DATE_FORMAT_FILTER_DATE="Y-m-d"
DATE_FORMAT_FILTER_DATETIME="Y-m-d H:i:s"
```

### Die HtmlHelper-Methode (Empfohlen)

Wie bei vielen gängigen Ausgabeelementen ist die HtmlHelper-Klasse hier zur...Hilfe! Die date()-Methode von HtmlHelper nimmt jeden Datums-Zeit-String an, den der Date-Konstruktor akzeptieren würde, zusammen mit einem Formatierungsstring, und gibt das Datum entsprechend den Zeitzoneneinstellungen des aktuellen Benutzers aus. Daher ist dies die empfohlene Methode, um Daten für den Benutzer auszugeben.

```php
use Joomla\CMS\HTML\HTMLHelper;
use Joomla\CMS\Language\Text;

$myDateString = '2012-12-1 15:20:00';
echo HtmlHelper::date($myDateString, Text::_('DATE_FORMAT_FILTER_DATETIME'));
```

### Die format()-Methode des Date-Objekts

Eine weitere Option ist, das Datum manuell zu formatieren. Wenn diese Methode verwendet wird, müssen Sie auch manuell die Zeitzone des Benutzers abrufen und einstellen. Diese Methode ist nützlicher für die Formatierung von Daten außerhalb der Benutzeroberfläche, beispielsweise in Systemprotokollen oder API-Aufrufen.

```php
use Joomla\CMS\Language\Text;
use Joomla\CMS\Date\Date;
use Joomla\CMS\Factory;

$myDateString = '2012-12-1 15:20:00';
$timezone = Factory::getUser()->getTimezone();

$date = new Date($myDateString);
$date->setTimezone($timezone);
echo $date->format(Text::_('DATE_FORMAT_FILTER_DATETIME'));
```

## Weitere nützliche Code-Beispiele

### Schnelles Ausgeben der aktuellen Uhrzeit

Es gibt zwei einfache Möglichkeiten, dies zu tun.
- Die date()-Methode von HtmlHelper, wenn kein Datumswert angegeben wird, wird standardmäßig die aktuelle Uhrzeit verwenden.
- Factory::getDate() erhält das aktuelle Datum als Date-Objekt, das wir dann formatieren können.

```php
use Joomla\CMS\Factory;
use Joomla\CMS\HTML\HTMLHelper;
use Joomla\CMS\Language\Text;

// These two are functionally equivalent
echo HtmlHelper::date('now', Text::_('DATE_FORMAT_FILTER_DATETIME'));

$timezone = Factory::getUser()->getTimezone();
echo Factory::getDate()->setTimezone($timezone)->format(Text::_('DATE_FORMAT_FILTER_DATETIME'));
```

### Hinzufügen und Subtrahieren von Daten

Da das Joomla Date-Objekt das PHP DateTime-Objekt erweitert, bietet es Methoden zum Addieren und Subtrahieren von Daten. Die einfachste dieser Methoden ist die modify()-Methode, die jeden relativen Änderungsstring akzeptiert, den auch die PHP-Methode [strtotime](https://www.php.net/manual/en/function.strtotime.php) akzeptieren würde. Zum Beispiel:

```php
use Joomla\CMS\Date\Date;

$date = new Date('2012-12-1 15:20:00');
$date->modify('+1 year');
echo $date->toSQL(); // 2013-12-01 15:20:00
```

Es gibt auch separate Methoden add() und sub(), um Zeit zu einem Datumsobjekt hinzuzufügen oder davon abzuziehen. Diese akzeptieren PHP-Standard-[DateInterval](https://www.php.net/manual/en/class.dateinterval.php)-Objekte:

```php
use Joomla\CMS\Date\Date;

$interval = new \DateInterval('P1Y1D'); // Interval represents 1 year and 1 day

$date1 = new Date('2012-12-1 15:20:00');
$date1->add($interval);
echo $date1->toSQL(); // 2013-12-02 15:20:00

$date2 = new Date('2012-12-1 15:20:00');
$date2->sub($interval);
echo $date2->toSQL(); // 2011-11-30 15:20:00
```

### Datumsangabe im ISO 8601-Format

```php
$date = new Date('2012-12-1 15:20:00');
$date->toISO8601(); // 20121201T152000Z
```

### Ausgaben von Daten im RFC 822-Format

```php
$date = new Date('2012-12-1 15:20:00');
$date->toRFC822(); // Sat, 01 Dec 2012 15:20:00 +0000
```

### Ausgabe von Daten im SQL-Datumszeitformat

```php
$date = new Date('20121201T152000Z');
$date->toSQL(); // 2012-12-01 15:20:00
```

### Ausgabe von Daten als Unix-Zeitstempel

Ein Unix-Zeitstempel wird als die Anzahl der Sekunden ausgedrückt, die seit dem Unix-Epoch (der ersten Sekunde des 1. Januar 1970) vergangen sind.

*Übersetzt von openai.com*

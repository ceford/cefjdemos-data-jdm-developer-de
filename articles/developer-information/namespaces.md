<!-- Filename: J4.x:Namespace_Conventions_In_Joomla / Display title: Namensräume -->

## Einführung

Die Verwendung von **PHP-Namespaces** ist eine Möglichkeit, verwandte Klassen, Schnittstellen, Funktionen und Konstanten zu gruppieren, um Namenskonflikte in größeren Projekten zu vermeiden. Sie ermöglichen es Entwicklern, Code effektiver zu organisieren, insbesondere beim Arbeiten mit Drittanbieter-Bibliotheken oder großen Codebasen, die möglicherweise doppelte Klassennamen enthalten.

Es gibt weitere Informationen im Abschnitt [Namespace](jdocmanual?article=docus/namespaces/index) der Joomla! Programmierer-Dokumentation.

### Hauptmerkmale von PHP-Namensräumen

1. **Vermeidung von Namenskonflikten**:
   - Ohne Namespaces führt PHP einen Fehler aus, wenn zwei Klassen denselben Namen haben. Namespaces bieten einen Kontext, um Klassen oder andere Elemente eindeutig zu identifizieren.
2. **Code organisieren**:
   - Namespaces ermöglichen die logische Gruppierung verwandter Klassen oder Funktionen, was den Code leichter lesbar und wartbar macht.
3. **Zugriff über vollqualifizierte Namen**:
   - Klassen und Funktionen innerhalb eines Namespaces können über ihren vollqualifizierten Namen aufgerufen werden, der den Namespace-Pfad einschließt.

### Gemeinsame Praktiken:
- Verwenden Sie Namensräume, um die Struktur Ihres Projektordners zur Konsistenz widerzuspiegeln.
- Importieren Sie Namensräume mit dem Schlüsselwort `use` für saubereren Code.

## Die Manifestdatei

Die initiale Deklaration eines Erweiterungs-Namensraums befindet sich in einer Erweiterungs-Manifeste-Datei, wie in diesem Beispiel:

```xml
    <namespace path="src">Acme\Component\Wonderful</namespace>
```

Die Beiträge in dieser Erklärung sind wie folgt:

- **path="src"** teilt dem Klassenlader mit, dass die namensräumlich organisierten Klassen im **src**-Ordner der Erweiterung enthalten sind, zum Beispiel com_wonderful/src.
- **Acme** ist das Lieferantenpräfix. Für Joomla-Kernerweiterungen wäre das *Joomla*. Sie müssen Ihr eigenes einzigartiges Lieferantenpräfix erfinden.
- **Komponente** gibt den Erweiterungstyp an (Komponente, Modul, Plugin, Vorlage, Bibliothek, ...).
- **Wunderbar** ist der Erweiterungsname. Wählen Sie einen Erweiterungsnamen, der kurz, aussagekräftig und voraussichtlich einzigartig ist.

## Die Beitragsdatei

Die Namespace-Deklaration muss die erste Anweisung in der Datei sein, in der sie verwendet wird. Zum Beispiel:

```php
<?php

/**
 * @package     Wonderful
 * @subpackage  Site
 *
 * @copyright   (C) 2024 Merlin. All rights reserved.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace Acme\Component\Wonderful\Controller;

use Joomla\CMS\MVC\Controller\BaseController;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

/**
 * Controller to display the default page - display of wonderful items
 *
 * @since  1.0.0
 */
class MagicController extends BaseController
{
    public function dosomething($wonderful)
    {
        // ... output something wonderful!
    }
}
```

## Verwenden der Klassendatei

Wenn Sie die Klasse verwenden müssen, zum Beispiel in einer Vorlage, würden Sie eine **use**-Anweisung am Anfang der Vorlage einfügen:

```
use Acme\Component\Wonderful\Controller\MagicController;
```

Und dann instanziieren Sie die Klasse und rufen die Funktion auf:

```
$magic = new MagicController;
$result = $magic->dosomething('wonderful');
```

## Die autoload_psr4.php-Datei

Diese Datei befindet sich im Ordner administrator/cache der Joomla-Website. Sie enthält eine vollständige Karte der Namensräume, die aus Manifestdateien extrahiert wurden. Sie wird bei der Installation generiert und jedes Mal neu generiert, wenn eine Erweiterung installiert oder neu installiert wird. Sie können diese Datei löschen, und sie wird bei der nächsten Seitenladung neu generiert. Es ist manchmal notwendig, dies zu tun, wenn die Installation einer Erweiterung mit einer ungewöhnlichen Methode versucht wurde. Die Datei enthält 275 oder mehr Pfade zu klassifizierten Standorten innerhalb der Namensräume.

**PSR** ist ein Akronym für [**PHP Standards Recommendation**](https://www.php-fig.org/psr/) und [**PSR-4: Autoloader**](https://www.php-fig.org/psr/psr-4/) ist ein akzeptierter Standard für das Laden von PHP-Klassen.

*Übersetzt von openai.com*


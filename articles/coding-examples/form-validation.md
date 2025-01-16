<!-- Filename: J4.x:Joomla_4_Tips_and_Tricks:_Form_Validation_Basics / Display title: Formularvalidierung -->

## Einführung

Joomla verfügt über ein Client-seitiges Validierungsskript, das von jeder Komponente verwendet und erweitert werden kann. Dieser Beitrag erklärt, wie es funktioniert und wie man es benutzt. Es gibt vier Standard-Validatoren für die Feldtypen Benutzername, Passwort, numerisch und E-Mail. Es gibt auch einen allgemeineren Muster-Validator und einen Pflichtfeld-Validator.

Beachten Sie, dass moderne Browser standardmäßig auch eine Formularvalidierung bieten. Die Browser-Validierung kann ausgeschaltet werden, indem novalidate im Form-Tag aufgenommen wird.

## So rufen Sie die Validierung auf

Am Anfang der edit.php-Datei, die den Code zur Formularerstellung enthält, fügen Sie den Aufruf zum Laden der Validator-JavaScript-Datei ein:

```php
/** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
$wa = $this->document->getWebAssetManager();
$wa->useScript('keepalive')
    ->useScript('form.validate');
```

im Form-Tag stellen Sie sicher, dass Sie class="form-validate" einfügen. Dieses Beispiel stammt aus dem Benutzerbearbeitungsformular:

```html
<form action="<?php echo Route::_('index.php?option=com_users&layout=edit&id=' . (int) $this->item->id); ?>"
    method="post" name="adminForm" id="user-form"
    enctype="multipart/form-data"
    aria-label="<?php echo Text::_('COM_USERS_USER_FORM_' . ( (int) $this->item->id === 0 ? 'NEW' : 'EDIT'), true); ?>"
    class="form-validate">
```

Im XML-Formulardatei die Anweisungen einschließen, die die individuelle Feldvalidierung aufrufen. Dieses Beispiel ist das Benutzer-E-Mail-Feld:

```xml
        <field
            name="email"
            type="email"
            label="JGLOBAL_EMAIL"
            required="true"
            size="30"
            validate="email"
            validDomains="com_users.domains"
        />
```

## Die Validierungsausdrücke

Dieser Abschnitt soll ein wenig Verständnis für die Validierungsausdrücke vermitteln, um die Verwendung zu erklären. Alle sind im validator.js-Code enthalten.

### Benutzername

```php
this.setHandler('username', value => {
      const regex = new RegExp('[<|>|"|\'|%|;|(|)|&]', 'i');
      return !regex.test(value);
    });
```

Hier sucht der Ausdruck nach dem Vorkommen eines der alternativen Zeichen innerhalb der Zeichenklasse [] und gibt false (ungültig) zurück, wenn eines gefunden wird.

### Passwort

```pnp
this.setHandler('password', value => {
      const regex = /^\S[\S ]{2,98}\S$/;
      return regex.test(value);
    });
```

Hier sucht der Ausdruck nach einem führenden Zeichen, das kein Leerzeichen ist, gefolgt von 2 bis 98 Zeichen, die entweder kein Leerzeichen oder ein Leerzeichen sind und mit einem Zeichen ohne Leerzeichen enden. Joomla verfügt über einen ausgefeilteren Passwortprüfer, der eine Mischung aus Zeichentypen und eine Mindestlänge gewährleistet.

### E-Mail

```php
this.setHandler('email', value => {
      const newValue = punycode.toASCII(value);
      const regex = /^[a-zA-Z0-9.!#$%&’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/;
      return regex.test(newValue);
    }); // Attach all forms with a class 'form-validate'
```

Hier akzeptiert der reguläre Ausdruck jede Zeichenkette, die mit einem oder mehreren der Zeichen aus der [] Zeichenklasse beginnt, gefolgt von @, gefolgt von einem oder mehreren Zeichen aus der zweiten [] Zeichenklasse, gefolgt von null oder mehr Punkten, gefolgt von einem oder mehreren Zeichen der dritten Zeichenklassengruppe und endet mit einer der Gruppen nach dem @.

Obwohl technisch korrekt, erlaubt dieser Validator einige ziemlich unsinnige E-Mail-Adressen wie !@me - Sie könnten ein restriktiveres Muster erstellen wollen.

### Numerisch

```php
this.setHandler('numeric', value => {
      const regex = /^(\d|-)?(\d|,)*\.?\d*$/;
      return regex.test(value);
    });
```

Hier muss der Wert mit einer Ziffer oder einem Minuszeichen null- oder einmal beginnen, gefolgt von einer Ziffer oder einem Komma null- oder mehrmals, gefolgt von einem Punkt null- oder einmal, endend mit einer Ziffer null- oder mehrmals. Dies passt also zu 3.142 oder -10 oder 100.000,0 und so weiter. Die tatsächliche Zahl muss dem Feldtyp in der Datenbank entsprechen. Der Ausdruck wird keine Zahlen mit Exponenten akzeptieren, daher wird 10e2 abgelehnt.

### Muster

Angenommen, Sie möchten ein Eingabefeld auf eine positive ganze Zahl beschränken. Sie könnten ein eigenes Muster verwenden, das in der Form-XML-Datei platziert wird:

```xml
        <field
            name="number"
            type="text"
            label="COM_MYCOMPONENT_CAMP_NUMBER_LABEL"
            description="COM_MYCOMPONENT_CAMP_NUMBER_DESC"
            class="w-auto"
            required="true"
            pattern="\d+"
        />
```

Hier erlaubt das Muster eine oder mehrere Ziffern. Daher wäre -1 ungültig, ebenso wie 3.142.

### Erforderlich

Stellen Sie sicher, dass required="true" in der Feldspezifikation enthalten ist, andernfalls wird das Formularlabel den häufig verwendeten * Marker für Pflichtfelder weglassen. Die Aufnahme von required in die Klasse reicht nicht für die Validierung aus.

*Übersetzt von openai.com*

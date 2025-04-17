<!-- Filename: API_Guides / Display title: API-Anleitungen -->

Diese Seite enthält ein Verzeichnis der Joomla API-Anleitungen. Diese Leitfäden sollen Ihnen helfen, zu verstehen, wie Sie diese Joomla-Funktionen in Ihren eigenen Joomla-Erweiterungen nutzen können.

Jedes API-Handbuch enthält Beispielcode, den Sie kopieren und in Ihrer eigenen Entwicklungsumgebung installieren können. In der Regel sind diese Codebeispiele so geschrieben, dass sie als Joomla-Modul eingebunden und installiert werden, daher könnte es hilfreich sein, die kurze Serie [Ein einfaches Modul erstellen](https://docs.joomla.org/Creating_a_simple_module) durchzugehen, wenn Sie mit der Entwicklung von Joomla-Modulen nicht vertraut sind.

Um Joomla 3.8 begann das Joomla-Entwicklungsteam damit, die Namenskonvention von Joomla-Klassen zu ändern und Namespaces zu verwenden. So wurde zum Beispiel aus JFactory die Klasse Factory im Namespace Joomla\CMS. Wenn Sie bestehenden Joomla-Code und Dokumentation lesen, können Sie feststellen, dass die Klassen entweder dem neuen oder dem alten Namensstandard folgen. Sie können die Zuordnung zwischen den beiden Namenskonventionen in der Datei `libraries/classmap.php` in Ihrer Joomla-Instanz finden.

- Die grundlegenden Anwendungsklassen und deren Hierarchie und Zwecke werden beschrieben in [Verständnis der Anwendungsklassen](https://docs.joomla.org/J3.x:Understanding_the_Application_classes)
- Ajax-Verarbeitung innerhalb von Joomla-Komponenten wird beschrieben in [JSON-Antworten mit JResponseJson](https://docs.joomla.org/JSON_Responses_with_JResponseJson "JSON Responses with JResponseJson"). Ajax kann auch innerhalb von Joomla-Modulen und Plugins verwendet werden, wie beschrieben in [Verwendung der Joomla Ajax-Schnittstelle](https://docs.joomla.org/Using_Joomla_Ajax_Interface).
- [Cache](https://docs.joomla.org/Cache_Basic_API_Guide) - wie der Rückruf-Cache in Ihrem Code verwendet werden kann.
- [Kategorien](https://docs.joomla.org/Categories_and_CategoryNodes_API_Guide) Verwendung der `Categories` und `CategoryNode` Klassen, um auf Daten zu Joomla-Kategorien zuzugreifen.
- CSS kann hinzugefügt werden, wie beschrieben in [Hinzufügen von JavaScript und CSS zur Seite](https://docs.joomla.org/Adding_JavaScript_and_CSS_to_the_page)
- Datenbank / JDatabase. Es gibt zwei API-Guides, die sich mit [Auswahl von Daten mit JDatabase](https://docs.joomla.org/Selecting_data_using_JDatabase "Selecting data using JDatabase") und [Einfügen, Aktualisieren und Entfernen von Daten mit JDatabase](https://docs.joomla.org/Inserting,_Updating_and_Removing_data_using_JDatabase) beschäftigen.
- [Datum/JDate](https://docs.joomla.org/How_to_use_JDate) ist die Joomla-Datumsklasse.
- Dateien und Ordner. Siehe [Verwendung des Dateisystempakets](https://docs.joomla.org/How_to_use_the_filesystem_package).
- Formular / JForm. Es gibt einen [Grundlagen-Leitfaden](https://docs.joomla.org/Basic_form_guide "Basic form guide") zur Verwendung der Joomla Form-API und zur Integration von Formularen in eine Joomla-Komponente sowie einen [Fortgeschrittenen Leitfaden](https://docs.joomla.org/Advanced_form_guide) für komplexere Aspekte der APIs.
- FormField / JFormField. Diese Klasse und verwandte Klassen wie JFormFieldList, die von ihr erben, sind hauptsächlich nützlich für die Definition benutzerdefinierter Formularfelder, wie beschrieben in [Erstellen eines benutzerdefinierten Formulartypfelds](https://docs.joomla.org/Creating_a_custom_form_field_type).
- [Eingabe / JInput](https://docs.joomla.org/Retrieving_request_data_using_JInput) Verwendung der `Input` Klasse, um die Werte von Parametern in HTTP-GET- und POST-Anfragen zu erhalten.
- JavaScript kann hinzugefügt werden, wie beschrieben in [Hinzufügen von JavaScript und CSS zur Seite](https://docs.joomla.org/Adding_JavaScript_and_CSS_to_the_page)
- Die Verwendung von Joomla-Layouts wird beschrieben in [Freigeben von Layouts über Ansichten oder Erweiterungen mit JLayout](https://docs.joomla.org/J3.x:Sharing_layouts_across_views_or_extensions_with_JLayout "J3.x:Sharing layouts across views or extensions with JLayout"). Die Flexibilität wurde in Joomla 3.2 erhöht, wie beschrieben in [JLayout-Verbesserungen für Joomla!](https://docs.joomla.org/J3.x:JLayout_Improvements_for_Joomla!).
- [Protokoll / JLog](https://docs.joomla.org/Using_JLog) Protokollierung von Nachrichten (z.B. Fehlermeldungen, Debug-Nachrichten) in eine Protokolldatei und optional in die Debug-Konsole.
- [Menü und Menüelemente](https://docs.joomla.org/Menu_and_Menuitems_API_Guide)
- [Verschachtelte Mengen](https://docs.joomla.org/Using_nested_sets), die ermöglichen, eine Baumhierarchie in der Datenbanktabelle zu implementieren, werden von Joomla-Menüs, Beiträgen, Kategorien etc. verwendet.
- [Registry/JRegistry](https://github.com/joomla-framework/registry) ist eine Hilfsklasse, die sehr nützlich für die Bearbeitung von PHP-Arrays, das Konvertieren in JSON etc. ist.
- [JResponseJson](https://docs.joomla.org/JSON_Responses_with_JResponseJson) unterstützt die Beantwortung von Ajax-Anfragen im JSON-Format.
- Route / JRoute siehe [URLs in Joomla](https://docs.joomla.org/URLs_in_Joomla)
- Tabelle / JTable bietet Funktionen zum Ausführen von CRUD-Operationen (und mehr) auf Datenbanktabellen. Der Leitfaden ist aufgeteilt in einen [Grundlegenden API-Leitfaden](https://docs.joomla.org/Table_Basic_API_Guide "Table Basic API Guide") und einen [Fortgeschrittenen API-Leitfaden](https://docs.joomla.org/Table_Advanced_API_Guide).
- Die [Controller](https://docs.joomla.org/Controllers) (BaseController, AdminController, FormController, ApiController) sind verantwortlich für die Analyse der Benutzeranfrage, die Überprüfung, dass der Benutzer die Aktion durchführen darf, und die Bestimmung, wie die Anfrage erfüllt wird.
- Die [Modelle](https://docs.joomla.org/Models) (BaseModel, BaseDatabaseModel, ItemModel, ListModel, FormModel, AdminModel) kapseln die Daten, die von der Komponente verwendet werden. Sie sind auch verantwortlich für die Aktualisierung der Datenbank, wo es erforderlich ist.
- Die [Ansichten](https://docs.joomla.org/Views) (AbstractView, CategoriesView, CategoryFeedView, CategoryView, FormView, HtmlView, JsonApiView, JsonView, ListView) spezifizieren, was auf der Webseite erscheinen soll, und sammeln alle notwendigen Daten, um die HTTP-Antwort auszugeben.
- [Tags](https://docs.joomla.org/Tags_API_Guide).
- Uri / JUri siehe [URLs in Joomla](https://docs.joomla.org/URLs_in_Joomla)
- [Benutzer / JUser](https://docs.joomla.org/Accessing_the_current_user_object) API in Bezug auf den Joomla-Benutzer.

*Übersetzt von openai.com*

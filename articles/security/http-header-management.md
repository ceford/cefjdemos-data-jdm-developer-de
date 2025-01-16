<!-- Filename: J4.x:Http_Header_Management / Display title: HTTP-Header -->

## Einführung

Joomla 4 hat ein HTTP-Header-System eingeführt, das Website-Besitzern helfen soll, HTTP-Sicherheitsheader zu konfigurieren. Es wird mithilfe eines *System - HTTP Headers* Plugins implementiert. Es gibt ein umfassendes [Benutzer](jdocmanual?article=user/security/http-headers) Tutorial zu diesem Thema. Dieses Tutorial ist kürzer und behandelt einige für Entwickler relevante Punkte.

## Das System - HTTP-Headers-Plugin

### Plugin-Registerkarte

Navigieren Sie zu **System → Plugins → System - HTTP-Headers**, um das Plugin-Konfigurationsformular zu öffnen.

![System http-Header-Plugin-Formular](../../../en/images/security/security-http-headers-plugin.png)

- **X-Frame-Options** Diese Option ist standardmäßig aktiviert, aber die [Dokumentation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options) besagt, dass sie veraltet ist und stattdessen eine *frame-ancestors* Richtlinie verwendet werden sollte.
- **Referrer-Policy** Der Standard ist *strict-origin-when-cross-origin*.
- **Cross-Origin-Opener-Policy** Der Joomla-Standardwert ist `same-origin`.
- **Erzwinge HTTP-Header** Es sind standardmäßig keine erzwungenen Header festgelegt. Hier kann eine Alternative zu *X-Frame-Options* angegeben werden. Der Wert von `Content-Security-Policy` kann einer der folgenden sein:
    - `'none'`
    - `'self' https://www.example.org;`
    - `'self' https://example.org https://example.com https://store.example.com;`

Verwenden Sie das Unterformular **Force HTTP Headers**, um die folgenden Header zu erzwingen:

- [Content-Security-Policy](https://scotthelme.co.uk/content-security-policy-an-introduction/)
- [Content-Security-Policy-Report-Only](https://scotthelme.co.uk/content-security-policy-an-introduction/#testingapolicy)
- [Cross-Origin-Opener-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cross-Origin-Opener-Policy)
- [Expect-CT](https://scotthelme.co.uk/a-new-security-header-expect-ct/)
- [Feature-Policy & Permissions-Policy](https://scotthelme.co.uk/a-new-security-header-feature-policy/)
- [NEL (Netzwerkantwortprotokollierung)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/NEL)
- [Referrer-Policy](https://scotthelme.co.uk/a-new-security-header-referrer-policy/)
- [Report-to](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/report-to)
- [Strict-Transport-Security](https://scotthelme.co.uk/hsts-the-missing-link-in-tls/)
- [X-Frame-Options](https://scotthelme.co.uk/hardening-your-http-response-headers/#x-frame-options)

### Tabreiter Strenge-Transport-Sicherheit (HSTS)

![strikte Transport-Sicherheits-Einstellungen](../../../en/images/security/security-http-headers-hsts.png)

Verwenden Sie die Schaltfläche *Inline-Hilfe umschalten* für Informationen zu jedem Parameter. Illustrierte Referenz:

[Formular zur Überprüfung oder Festlegung des HSTS-Vorlade-Status und der Berechtigung](https://hstspreload.org/)

### Content-Security-Policy (CSP)-Tab

![Inhaltsrichtlinienoptionen](../../../en/images/security/security-http-headers-csp.png)

Sobald aktiviert, können Sie den Client festlegen, bei dem Sie die konfigurierte CSP durchsetzen möchten, und es ermöglicht Ihnen, `site`, `administrator` oder `beide` einzustellen. Eine CSP sollte sowohl auf das Frontend als auch auf das Backend angewendet werden. Illustrierte Verweise:

- [Content-Sicherheitsrichtlinie](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)
- [Nonce](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/nonce)
- [Script- und Stil-Hashes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/script-src)
- [Richtlinien-Direktive](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/child-src) src Beschreibungen sind im Menü dieser Seite verfügbar.

Die letzte Option mit dem Namen *Richtlinie hinzufügen* ermöglicht es Ihnen, die Allowlist pro Richtlinie nach Ihren Bedürfnissen zu konfigurieren. Zum Beispiel sollte für die *script-src* Richtlinie das *Wert*-Feld die Ursprünge enthalten, von denen Sie das Laden von Skripten erlauben möchten.

## Notizen

Wenn Sie einige HTTP-Sicherheitsheader direkt auf dem Server konfiguriert haben, kann es zu doppelten Einträgen kommen.

Überprüfen Sie die Ausgabe Ihrer HTTP-Header in der Browser-Konsole. In Google Chrome oder Firefox: Untersuchen → Netzwerk → die Ausgabe unter Headern. Sie können dann die Header deaktivieren, die doppelte Einträge verursachen. Überprüfen Sie auch die Konsole Ihres Browsers auf mögliche Fehler.

## Erweiterungsentwickler

Der große Vorteil einer Content-Security-Policy tritt auf, wenn der Header alle Inline-JavaScripts und Inline-CSS blockiert, die JavaScript-Ereignishandler über HTML-Attribute beeinflussen. Mit aktivem Browserschutz werden auch Inline-JavaScripts und Inline-CSS für Erweiterungen blockiert. Dieser Schutz ist standardmäßig nicht aktiviert, kann aber von Benutzern aktiviert werden.

Wo Inline-JavaScript und CSS erforderlich sind, sind Nonce- und Hash-Unterstützung in den Dokument-APIs enthalten. Wenn sie verwendet werden, stellt der Kern sicher, dass sie auf der Positivliste stehen, blockiert jedoch weiterhin schädlichen Code.

### Wichtige Hinweise für Erweiterungsentwickler

Ab Joomla 4.0 die Content-Sicherheitsrichtlinie:

- wird mit dem Kern ausgeliefert.
- ist standardmäßig deaktiviert.
- kann von Seitenadministratoren aktiviert werden.
- es wird dringend empfohlen, dass Ihr Erweiterungs-Frontend und -Backend mit aktivierter Content Security Policy arbeiten.

Bei aktivierter strikter Content-Security-Policy werden die folgenden Funktionen blockiert:

- Die Ausführung von JavaScript über die HTML-Ereignishandler (onXXX-Handler wie onClick und ähnliche).
- Die Ausführung von In-Page-JavaScript, das nicht über die Document-API an die Seite übergeben wird.
- Die Ausführung von JavaScript-Code, der in DOM-APIs wie eval() injiziert wird.
- Die Verwendung von Inline-In-Page-CSS, das nicht über die Document-API an die Seite übergeben wird.
- Die Verwendung von Inline-CSS mit dem HTML-Style-Attribut.

Um Ihre Erweiterungen auch bei aktivierter strenger Content Security Policy zum Laufen zu bringen, ist es am einfachsten, die Document API zu verwenden, um Ihr Inline-JavaScript und CSS anzuwenden. Siehe die Beispiele unten.

### Hinzufügen von JavaScript mit der Joomla-API

```php
    use Joomla\CMS\Factory;

    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();

    // Add JavaScript from URL
    $wa->registerAndUseScript('com_example.sample', 'https://example.org/sample.js', [], ['defer' => true]);

    // Add inline JavaScript
    $wa->addInlineScript('
        document.addEventListener("DOMContentLoaded", function(event) {
            alert("An inline JavaScript Declaration");
        });
    ');
```

### Hinzufügen von CSS mit der Joomla-API

```php
    use Joomla\CMS\Factory;

    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();

    // Add Style from URL
    $wa->registerAndUseStyle('com_example.sample', 'https://example.org/sample.css');

    // Add inline Style
    $wa->addInlineStyle('
        body {
            background: #00ff00;
            color: rgb(0,0,255);
        }
    ');
```

## Zusätzliche Ressourcen

- [CSP-Spickzettel](https://scotthelme.co.uk/csp-cheat-sheet/)
- [Content Security Policy - Eine Einführung](https://scotthelme.co.uk/content-security-policy-an-introduction/)
- [Security Headers](https://securityheaders.com/) ist ein Teil von Probely und wurde ursprünglich von Scott Helme erstellt! Es ist ein kostenloses und einfach zu verwendendes Tool, das entwickelt wurde, um Ihnen zu helfen, moderne Sicherheitsfunktionen, die für Ihre Website verfügbar sind, besser einzusetzen und zu verstehen.
- [CSP Evaluator](https://csp-evaluator.withgoogle.com/)
- [Web Grundlagen Content Security Policy](https://developers.google.com/web/fundamentals/security/csp)
- [Googles CSP-Dokumentation](https://csp.withgoogle.com/docs/index.html)
- [CSP ist tot, lang lebe CSP!](https://research.google/pubs/pub45542/) Über die Unsicherheit von Whitelists und die Zukunft der Content Security Policy
- [web.dev Suche nach Sicherheit](https://web.dev/s/results?q=security#gsc.tab=0&gsc.q=security&gsc.sort=)

*Übersetzt von openai.com*

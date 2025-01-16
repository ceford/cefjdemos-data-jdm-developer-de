<!-- Filename: Working_with_git_and_github / Display title: Arbeiten mit Git und GitHub -->

## Einführung

Dieses Dokument liefert Informationen darüber, wie man mit Hilfe von Git und GitHub zum Joomla! CMS beitragen kann. Wenn Sie eine einfache Änderung vornehmen möchten (nur eine Datei), ist es einfacher, diese Dokumentation zu verwenden: [Verwendung der Github-Benutzeroberfläche zum Erstellen von Pull Requests](https://docs.joomla.org/Using_the_Github_UI_to_Make_Pull_Requests). Wenn Sie komplexere Änderungen hinzufügen möchten oder einfach daran interessiert sind, lesen Sie weiter!

## Was sind Git und GitHub?

Git ist ein verteiltes Versionskontrollsystem. Es ist ein System, das Änderungen in Dateien aufzeichnet und diese Änderungen in einer Historiendatei speichert. Du kannst jederzeit auf eine frühere Version deines Codes zurückblicken und Änderungen bei Bedarf wiederherstellen. Aufgrund des Historienarchivs ist Git sehr nützlich, wenn du mit vielen Leuten zusammen an demselben Projekt arbeitest.

Hier ist, wie GitHub verwendet werden kann. [GitHub](https://www.github.com) ist eine Website, die bei der Verwaltung von Git-Projekten auf visuelle Weise hilft. Als Inhaber eines Projekts können Sie den Code ändern und verschiedene Versionen vergleichen. Als Nutzer des Projekts können Sie durch das Erstellen von Pull-Requests Beiträge leisten. Ein Pull-Request ist eine Anfrage an den Inhaber, Code in das Projekt einzufügen. Sie bieten ein Stück Code an (vielleicht eine Lösung für einen Fehler) und fragen, ob der Projekteigentümer es verwenden möchte. Wenn es dem Inhaber gefällt, kann er es mit seinem Projekt zusammenführen (hinzufügen).

Joomla! verwendet GitHub und Git, um seinen Code zu pflegen. Jeder kann zum Joomla!-Software über das [Github-Repository](https://github.com/joomla/joomla-cms) beitragen.

## Melden Sie sich bei GitHub an und installieren Sie Git

Zuerst müssen Sie sich bei [Github registrieren](http://www.github.com). Es ist kostenlos und einfach. Folgen Sie einfach den angegebenen Schritten.

Sobald Sie sich angemeldet haben, müssen Sie Git auf dem Computer installieren, den Sie für die Entwicklung verwenden (Arbeitsstation oder Laptop). Befolgen Sie die Installationsanweisungen auf der [git](http://git-scm.com) Website. Git ist ein CLI (Command Line Interface) Programm. Am Anfang kann dies verwirrend und etwas beängstigend sein, aber dieses Dokument wird Sie durch den Prozess führen.

Wenn Sie kein fortgeschrittener Benutzer sind, führen Sie einfach das Installationsprogramm aus und drücken die "Weiter"-Tasten, bis das Programm installiert ist. Git wird Ihr System nicht beschädigen. Später können Sie es wie jedes andere Programm entfernen.

Mit installiertem Git öffnen Sie eine Terminal-Anwendung. Beginnen Sie damit, Ihren Git-Namen und Ihre E-Mail-Adresse zu setzen. Git wird diese Informationen verwenden, wenn Sie zu einem Projekt beitragen:

```sh
    git config --global user.name "Your name"
    git config --global user.email youremail@mail.com
```

In den obigen Befehlen und allen anderen Befehlen, die in dieser Dokumentation angegeben sind, ist jede Zeile ein neuer Befehl. Sie tippen also die erste Zeile ein, drücken die Eingabetaste und tippen dann die zweite Zeile ein und drücken erneut die Eingabetaste.

Sie sind jetzt bereit, Git zu verwenden und mit der Einrichtung Ihrer Testinstallation fortzufahren.

## Einrichten einer Testinstallation

Für Ihre Testinstallation benötigen Sie einen Software-Stack, damit Sie Joomla! auf Ihrem Computer installieren und ausführen können. Stacks wie MAMP, XAMP oder WAMP werden an anderer Stelle in dieser Dokumentation behandelt.

Nach der Installation Ihres Software-Stacks müssen Sie die neueste Version von Joomla! installieren. Dies ist nicht die letzte stabile Version. Die letzte Version von Joomla! ist der Entwicklungszweig auf GitHub.

## Joomla! forken und klonen

Auf GitHub kannst du Projekte in sogenannten Repositories finden. Innerhalb eines Projekts kannst du mehrere Versionen finden. Eine solche Version wird als Branch bezeichnet. Joomla! hat die folgenden Branches:

- **4.2-dev** Dateien für die Entwicklung der aktuellen Version.
- **4.3-dev** Dateien für die Entwicklung der nächsten kleinen Version.
- **4.4-dev** Dateien für die Entwicklung der übernächsten kleinen Version.
- **5.0-dev** Dateien für die Entwicklung der nächsten Hauptversion.

Auf Ihrem Testcomputer werden Sie den **4.2-dev**-Branch verwenden. Sie können diesen Branch jedoch nicht ändern, da Sie nicht der Besitzer sind. Sie müssen eine Kopie davon erstellen. Auf GitHub wird dies als Fork bezeichnet. Sie sind der Besitzer dieser Kopie, sodass Sie sie ändern können. Nach der Änderung Ihres Forks können Sie eine Pull-Anfrage für die von Ihnen vorgenommenen Änderungen stellen. Mehr dazu später. Sie können einen Branch forken, indem Sie die Schaltfläche Fork im [Joomla! CMS Github Repository](https://github.com/joomla/joomla-cms) drücken. Diese Schaltfläche befindet sich oben rechts auf der Seite.

![Gabel joomla in github](../../../en/images/getting-started/core-git-fork-joomla.png)

Nach dem Forken müssen Sie Joomla! auf Ihrem lokalen Computer installieren. Gehen Sie zu dem Ordner, in dem Sie Dateien platzieren können, die von Ihrem Webserver verwendet werden. Viele Programme verwenden einen Ordner namens `htdocs`. Einige verwenden `www` und andere verwenden ganz andere Ordner. Es hängt alles davon ab, ob Sie Windows, Mac oder Linux verwenden. Schließlich wird Ihr Web-Root verschiedene Ordner für verschiedene Websites enthalten. Sobald Sie sich in Ihrem Web-Root-Ordner befinden, verwenden Sie entweder in einem offenen Terminalfenster den Befehl cd, um das aktuelle Verzeichnis auf das Web-Root zu ändern. Oder, in Ihrem Dateiexplorer-GUI, finden Sie den Web-Root-Ordner, drücken Sie die rechte Maustaste und klicken Sie auf: „Git Bash Here“ oder „Terminal öffnen“ oder etwas Ähnliches.

Im Terminal, mit dem als aktueller Ordner festgelegten Stammordner der Seite, geben Sie den folgenden Befehl ein, um die Dateien von Ihrem Fork des Joomla! CMS auf Ihren Computer herunterzuladen. Bitte ersetzen Sie *username* durch den Benutzernamen, den Sie auf GitHub verwenden.

```sh
    git clone https://github.com/username/joomla-cms.git
```

Der Klonvorgang kann eine Weile dauern. Sobald er abgeschlossen ist, wird Ihr Web-Stammverzeichnis einen Ordner namens joomla-cms enthalten. Sie müssen diesen Ordner zum aktuellen Ordner machen, um Git-Befehle für diesen Ordner auszuführen:

```sh
    cd joomla-cms
```

Bitte beachten Sie, dass für andere Befehle in dieser Dokumentation.

## Joomla! installieren

Ihr heruntergeladener Klon Ihres geforkten Repositorys erfordert weitere Maßnahmen, bevor er gebrauchsfertig ist. Eine der heruntergeladenen Dateien trägt den Namen README.md. Öffnen Sie diese Datei mit einem Texteditor und folgen Sie den Anweisungen unter **So erhalten Sie eine funktionierende Installation aus dem Quellcode**.

Wenn Sie bereit sind, öffnen Sie Ihren Browser und gehen Sie zu der Installation auf Ihrem lokalen Host. Normalerweise ist die URL in etwa wie folgt:
`http://localhost/joomla-cms`. Sie werden nun den Standard-Installationsprozess von Joomla! sehen.

## Änderungen am Code vornehmen

Alle Änderungen, die Sie am Joomla-Code auf Ihrer lokalen Seite vornehmen, werden von Git registriert und überwacht. Sie können jederzeit den Befehl `git status` eingeben, um zu sehen, welche Dateien verändert oder nicht verfolgt sind. Nicht verfolgt bedeutet, dass die Datei an diesem Ort für Git neu ist.

In diesem Stadium ist es wahrscheinlich am besten, eine Integrierte Entwicklungsumgebung (IDE) zu verwenden, um am Joomla-Code zu arbeiten. Visual Studio Code (VSCode) wird dringend empfohlen. Es ist kostenlos und funktioniert auf allen Plattformen. Mit VSCode können Sie Änderungen vornehmen, diese **committen** zu Ihrem lokalen Git-Klon und an Ihren entfernten GitHub-Fork **pushen**.

## Änderungen in den Fork übertragen

Das Hochladen von Änderungen wird in Git-Begriffen als `push` bezeichnet. Bevor du das tun kannst, musst du etwas sehr Wichtiges tun. Du musst einen neuen Branch für deine Änderungen erstellen. (Ein Branch ist eine Version deines Projekts, erinnerst du dich?) Wenn du das nicht tust und deine Änderung direkt im aktuellen Branch vornimmst, wird es beim ersten Mal kein Problem geben. Aber wenn du ein zweites Mal Änderungen vornimmst und die Änderungen, die du beim ersten Mal gemacht hast, noch nicht zusammengeführt sind, werden all diese Änderungen ebenfalls als neue Änderungen registriert.

Sie können VSCode so einrichten, dass alle folgenden Befehle mit nur wenigen Klicks ausgeführt werden. Wenn Sie jedoch Git-Befehle über die Terminalbefehlszeile verwenden möchten:

Der erste Befehl, den Sie ausführen werden, erstellt einen neuen Zweig. Ersetzen Sie name-new-branch durch den Namen des neuen Zweigs. Dieser Name muss kurz sein und darf nur Kleinbuchstaben und Zahlen enthalten. Verwenden Sie **NICHT** Leerzeichen. Verwenden Sie stattdessen - (Minus).

```sh
    git checkout -b name-new-branch
```

Der nächste Befehl teilt git mit, dass alle Änderungen bereit zum Committen sind.

```sh
    git add --all
```

Der folgende Befehl fügt Ihre Änderung zum Branch hinzu. Bitte ersetzen Sie die Nachricht mit einer kurzen Beschreibung Ihrer Änderungen. Diese Beschreibung wird der Titel des Pull-Requests sein, den Sie erstellen werden.

```sh
    git commit -m "description"
```

Der letzte Befehl wird die Änderungen in Ihren Fork hochladen. Bitte ersetzen Sie name-new-branch mit dem Namen des Zweigs, den Sie ein paar Schritte zuvor erstellt haben.

```sh
    git push origin name-new-branch
```

## Vergleiche Forks und erstelle eine Pull-Anfrage

Nachdem Sie Ihre Änderung auf GitHub hochgeladen haben, gehen Sie zu Ihrem Fork des Joomla! CMS auf der GitHub-Seite. Wählen Sie Ihren Branch aus und erstellen Sie eine Pull-Anfrage.

```markdown
Wenn Sie fertig sind, wechseln Sie in Ihrem lokalen Klon zurück zu dem ursprünglichen Branch:
```

```sh
git checkout 4.2-dev
```

Sie können jetzt neue Änderungen vornehmen, ohne die Änderungen in Ihrem vorherigen Branch zu beeinflussen.

## Auf dem neuesten Stand bleiben

Da sich die aktuelle Version von Joomla! täglich ändern kann, ist es wichtig, Ihren Fork auf dem neuesten Stand zu halten. Sie können dies tun, indem Sie das ursprüngliche Joomla-Repository zu Ihrem geforkten Projekt hinzufügen:

```sh
    git remote add upstream https://github.com/joomla/joomla-cms.git
```

Aktualisieren Sie dann Ihren lokalen Klon mit dem folgenden Befehl:

```sh
    git pull upstream 4.2-dev
```

Und aktualisieren Sie Ihren entfernten Fork:

```sh
    git push
```

*Übersetzt von openai.com*
